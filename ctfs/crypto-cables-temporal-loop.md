# Crypto - Cable's Temporal Loop

```
#!/usr/bin/env python3
"""
Padding Oracle Attack on "Cable's Temporal Loop"

Key observations:
1. Server gives us A (=a), S_0, and flag_ct on connect
2. math_test with d=0 reveals b (since (a*0+b)%p = b)
3. decrypt endpoint is a padding oracle BUT requires:
      int.from_bytes(ct, 'big') % p == current_state
   where current_state = (a*s+b)%p, and s updates after each call
4. p is only 32-bit, so we can prepend 4 bytes to any ciphertext
   to satisfy the mod constraint without affecting AES decryption
   (the extra bytes just get parsed by the algebraic check, not AES)

Wait - actually the entire `cb` is passed to `_dc(k, cb)` which treats
cb[0:16] as IV and cb[16:] as ciphertext. So prepending bytes WOULD
shift the IV and mess up decryption.

Better approach: we need to CRAFT the first 16 bytes (IV region) such that:
  - int(our_ct, 'big') % p == q
  - AES decryption of the blocks we care about still works

Since p is 32-bit (4 bytes), only the last 4 bytes of the integer matter
for the modular constraint. But int.from_bytes is big-endian, so only
the LAST 4 bytes of the ciphertext affect the low-order bits... no wait,
the ENTIRE value mod p matters.

Simpler: p ~ 2^32. Our ciphertext is say 48 bytes = 384 bits.
  int(ct) % p  -- we can freely modify the IV (first 16 bytes) 
  to tune this value mod p, since IV bytes don't affect decryption
  of later blocks (CBC: only P_i depends on C_{i-1}, and IV = C_0
  only affects P_1).

So for attacking block C_2 (second ciphertext block), we modify C_1 bytes
for the oracle, and tune IV bytes to satisfy the mod constraint.
For attacking C_1 (first block), we modify IV bytes for oracle... but those
same bytes are what we'd tune. Conflict! 

Resolution: When attacking the IV-dependent block (block 1), we need a 
different approach. We can add a dummy prefix block:
  craft_ct = new_iv (16 bytes) || modified_C0 (16 bytes) || C1 || ...
where new_iv is tuned for the mod constraint and modified_C0 is our
oracle probe. This shifts everything by one block.

Let me implement the full attack:
"""

import socket
import json
import sys

HOST = 'chals2.apoorvctf.xyz'
PORT = 13424

def recvline(sock):
    data = b''
    while True:
        c = sock.recv(1)
        if not c or c == b'\n':
            break
        data += c
    return data

def send_recv(sock, obj):
    sock.sendall(json.dumps(obj).encode() + b'\n')
    return json.loads(recvline(sock))

def connect():
    s = socket.socket()
    s.connect((HOST, PORT))
    s.settimeout(10)
    banner = json.loads(recvline(s))
    return s, banner

def get_b(sock, a, p):
    """Use math_test with d=0 to recover b"""
    r = send_recv(sock, {"option": "math_test", "data": 0})
    b = r['result']  # (a*0 + b) % p = b (assuming b < p, which it is since b<0xFFFF < p)
    return b

def compute_state(a, s, b, p):
    return (a * s + b) % p

def find_prefix_to_satisfy_constraint(target_ct_bytes, q, p):
    """
    Find a 4-byte big-endian prefix X such that
    int(X || target_ct_bytes, 'big') % p == q
    
    int(X || rest) = X * 256^len(rest) + int(rest)
    We need: (X * M + R) % p == q
    where M = 256^len(rest) % p, R = int(rest) % p
    => X * M ≡ (q - R) mod p
    => X = (q - R) * modinv(M, p) mod p
    Since p < 2^32, X fits in 4 bytes
    """
    n = len(target_ct_bytes)
    M = pow(256, n, p)
    R = int.from_bytes(target_ct_bytes, 'big') % p
    diff = (q - R) % p
    # Need modinv(M, p)
    X = (diff * pow(M, -1, p)) % p
    return X.to_bytes(4, 'big')

def make_oracle_ct(iv, blocks, q, p, s, a, b):
    """
    Build a ciphertext with a 4-byte prefix to satisfy modular constraint.
    Structure: [4-byte prefix] [IV 16 bytes] [ciphertext blocks...]
    
    BUT wait - _dc treats first 16 bytes as IV and rest as ciphertext.
    If we prepend 4 bytes, the 'IV' seen by AES = prefix[4:] + iv[:12]
    which breaks things.
    
    CORRECT approach: tune the IV itself.
    The IV (first 16 bytes) doesn't affect decryption of C1, C2, etc.
    It only affects P1 (the XOR after decrypting C1 gives P1 = Dec(C1) XOR IV).
    
    For padding oracle on block Ci (i>=2):
      - We control C_{i-1} for the oracle probe
      - We can set IV freely to tune the mod constraint
      - P1 will be garbage but we don't care about P1 for these blocks
    
    For padding oracle on block C1:
      - We need to control IV for the oracle probe
      - Can't also use IV to tune mod constraint
      - Solution: insert a dummy block before C1
        Structure: [tuning_iv] [dummy_block=oracle_probe] [C1] 
        Now AES sees: IV=tuning_iv, C1=dummy_block (garbled P1), C2=original_C1
        We're actually doing oracle on original_C1 using dummy_block as the "C1"
        probe. The padding check is on ALL blocks, specifically the LAST block.
        
    Actually _dc just checks if unpadding succeeds on the last block.
    So we need the LAST block to have valid PKCS7 padding after decryption.
    
    For CBC: P_last = Dec(C_last) XOR C_{last-1}
    We control C_{last-1} in our oracle probe.
    """
    ct_bytes = iv + b''.join(blocks)
    # Tune IV to satisfy mod constraint
    # iv is first 16 bytes; we'll adjust last 4 bytes of iv
    # keeping first 12 bytes of iv fixed, adjust last 4
    # int(iv[0:12] + X + blocks) % p == q
    # Treat everything after iv[0:12] as the "rest"
    rest = iv[12:] + b''.join(blocks)  # 4 + all blocks
    n = len(rest)
    M = pow(256, n, p)
    R = int.from_bytes(rest, 'big') % p
    # We're setting iv[12:16] = X, a 4-byte value
    prefix_fixed = iv[:12]
    fixed_val = int.from_bytes(prefix_fixed, 'big')
    fixed_contribution = (fixed_val * pow(256, n+4, p)) % p  # fixed 12 bytes shift
    
    # Actually let's just do it cleanly:
    # full_ct = iv_tuned (16 bytes) + cipher_blocks
    # int(full_ct) % p == q
    # We tune iv_tuned[-4:] (last 4 bytes of IV)
    # full_ct = iv[:12] + X(4 bytes) + blocks
    suffix = b''.join(blocks)
    # int(iv12 + X + suffix) where iv12=iv[:12]
    # = int(iv12) * 2^(4+len(suffix))*8 + int(X)*2^(len(suffix)*8) + int(suffix)
    n_suffix = len(suffix)
    M_x = pow(256, n_suffix, p)  # multiplier for X (4 bytes before suffix)
    n_iv12 = 12
    iv12_int = int.from_bytes(iv[:12], 'big')
    M_iv12 = pow(256, 4 + n_suffix, p)
    R_total = (iv12_int * M_iv12 + int.from_bytes(suffix, 'big')) % p
    # (R_total + X * M_x) % p == q
    # X * M_x ≡ (q - R_total) mod p
    diff = (q - R_total) % p
    X = (diff * pow(M_x, -1, p)) % p
    x_bytes = X.to_bytes(4, 'big')
    return iv[:12] + x_bytes + suffix

def padding_oracle(sock, a, b, p, s_ref, flag_ct_bytes):
    """
    Perform padding oracle attack.
    flag_ct_bytes = IV (16) + C1 (16) + C2 (16) + ...
    
    We decrypt each block from last to first (except we recover plaintext).
    
    For each target block Ci, we manipulate C_{i-1} byte by byte.
    We tune the IV (first 16 bytes of our sent ciphertext) to satisfy mod constraint.
    
    Current LCG state is tracked here.
    """
    s = [s_ref]  # mutable reference
    
    def get_q():
        return compute_state(a, s[0], b, p)
    
    def update_s():
        s[0] = get_q()
    
    def oracle(ct_bytes):
        """Send ciphertext, returns True if padding is valid"""
        q = get_q()
        # We need int(ct_bytes) % p == q
        # ct_bytes = [iv(16)] + [our_blocks...]
        # Tune last 4 bytes of IV
        iv = ct_bytes[:16]
        rest_blocks = ct_bytes[16:]
        
        suffix = rest_blocks
        n_suffix = len(suffix)
        M_x = pow(256, n_suffix, p)
        iv12_int = int.from_bytes(iv[:12], 'big')
        M_iv12 = pow(256, 4 + n_suffix, p)
        R_total = (iv12_int * M_iv12 + int.from_bytes(suffix, 'big')) % p
        diff = (q - R_total) % p
        X = (diff * pow(M_x, -1, p)) % p
        x_bytes = X.to_bytes(4, 'big')
        final_ct = iv[:12] + x_bytes + suffix
        
        # Verify
        assert int.from_bytes(final_ct, 'big') % p == q, "Mod constraint failed!"
        
        r = send_recv(sock, {"option": "decrypt", "ct": final_ct.hex()})
        
        if 'error' in r:
            if 'expected_state' in r:
                # State desync - shouldn't happen if we computed correctly
                print(f"[!] State desync: {r}")
                return None
            return None
        
        # After successful call (math_ok), state updates
        update_s()
        return r.get('oracle') == 'padding_ok'
    
    blocks = []
    n = len(flag_ct_bytes)
    for i in range(0, n, 16):
        blocks.append(flag_ct_bytes[i:i+16])
    
    # blocks[0] = IV, blocks[1..] = ciphertext blocks
    print(f"[*] Flag CT has {len(blocks)-1} cipher blocks")
    
    plaintext = b''
    
    # Decrypt each ciphertext block
    for block_idx in range(1, len(blocks)):
        print(f"[*] Decrypting block {block_idx}/{len(blocks)-1}")
        target_block = blocks[block_idx]
        prev_block = blocks[block_idx - 1]
        
        # intermediate[i] = Dec_AES(target_block)[i] 
        # P[i] = intermediate[i] XOR prev_block[i]
        intermediate = bytearray(16)
        
        for byte_pos in range(15, -1, -1):
            pad_byte = 16 - byte_pos
            
            # Build the C' (modified prev block)
            # For positions already solved: C'[j] = intermediate[j] XOR pad_byte
            c_prime = bytearray(16)
            for j in range(byte_pos + 1, 16):
                c_prime[j] = intermediate[j] ^ pad_byte
            
            found = False
            for guess in range(256):
                c_prime[byte_pos] = guess
                
                # Build full ciphertext: [random_iv(16)] [c_prime(16)] [target_block(16)]
                # We use a fresh random-ish IV (will be tuned anyway)
                test_iv = bytearray(16)  # zeros, will be tuned
                test_ct = bytes(test_iv) + bytes(c_prime) + target_block
                
                result = oracle(test_ct)
                if result is None:
                    print(f"[!] Oracle error at block {block_idx}, pos {byte_pos}, guess {guess}")
                    continue
                
                if result:
                    # Verify it's not a false positive (only matters for last byte)
                    if byte_pos == 15 and pad_byte == 1:
                        # Try flipping byte 14 to confirm
                        c_prime2 = bytearray(c_prime)
                        c_prime2[14] ^= 1
                        test_ct2 = bytes(test_iv) + bytes(c_prime2) + target_block
                        result2 = oracle(test_ct2)
                        if not result2:
                            continue  # false positive, keep trying
                    
                    intermediate[byte_pos] = guess ^ pad_byte
                    print(f"  [+] byte {byte_pos}: intermediate=0x{intermediate[byte_pos]:02x}, "
                          f"plain=0x{intermediate[byte_pos] ^ prev_block[byte_pos]:02x} "
                          f"({chr(intermediate[byte_pos] ^ prev_block[byte_pos]) if 32<=intermediate[byte_pos]^prev_block[byte_pos]<127 else '?'})")
                    found = True
                    break
            
            if not found:
                print(f"  [!] Failed to find byte at position {byte_pos}")
                intermediate[byte_pos] = 0
        
        # Recover plaintext for this block
        block_plain = bytes(intermediate[i] ^ prev_block[i] for i in range(16))
        plaintext += block_plain
        print(f"[*] Block {block_idx} plaintext: {block_plain}")
    
    return plaintext

def main():
    print("[*] Connecting...")
    sock, banner = connect()
    print(f"[*] Banner: {banner}")
    
    a = banner['lcg_params']['A']
    s0 = banner['lcg_params']['S_0']
    flag_ct_hex = banner['flag_ct']
    flag_ct = bytes.fromhex(flag_ct_hex)
    
    print(f"[*] a={a}, S_0={s0}")
    print(f"[*] Flag CT ({len(flag_ct)} bytes): {flag_ct_hex[:32]}...")
    
    # Step 1: Recover b and p
    # math_test with d=0 gives b % p = b (b < 0xFFFF < p for 32-bit prime)
    r0 = send_recv(sock, {"option": "math_test", "data": 0})
    b = r0['result']
    print(f"[*] Recovered b={b}")
    
    # math_test with d=1 gives (a+b) % p
    r1 = send_recv(sock, {"option": "math_test", "data": 1})
    ab = r1['result']
    # ab = (a + b) % p
    # If a+b < p: ab = a+b, so p > a+b (no info directly)
    # We need p. Use two more queries:
    # d=p would give 0, but we don't know p yet.
    # Better: math_test with d=s0 gives (a*s0+b)%p = first LCG state
    # We can also get p from: query large d values
    
    # To find p: query d such that a*d+b overflows mod p.
    # Or: we know a (16-bit), b (16-bit), p (32-bit prime)
    # From d=0: b = b0 (=b since b < p)
    # From d=1: (a+b) % p = ab
    # If a+b < p: ab = a+b (no wrap), else ab = a+b-p
    # Query d=2: 2a+b % p
    # We can recover p by: send d = floor(p/a) type queries
    # 
    # Simpler: send d = (p-b)//a would give 0, but...
    # 
    # Actually: a*d+b = q*p + r where r is result
    # For d=0: b = r0, so b%p = r0
    # For large d, a*d+b will wrap. 
    # We can find p by:
    #   result(d) = (a*d + b) % p
    #   result(d + p) = (a*(d+p) + b) % p = (a*d + b + a*p) % p = result(d)
    # So period is p/gcd(a,p). Since p is prime and a<p, period=p.
    # 
    # Alternative: use two queries to get a linear equation.
    # r1 - r0 = a (mod p)  [if no wrap between d=0 and d=1]
    # But we already know a! So: r1 = (a + b) % p
    # If r1 >= a+b: impossible (results are non-negative)
    # r1 = a + b - k*p for some k in {0,1}
    # Since a,b < 0xFFFF=65535, a+b < 131070 < 2^17
    # p is a 32-bit prime, so p > 2^31
    # Therefore a+b < p always! So r1 = a+b exactly, no mod.
    # Wait... but b = r0 = b%p. Since b<0xFFFF < 2^31 < p, b=r0.
    # And a+b < 131070 < p. So r1 = a+b. But we know a already!
    # 
    # To find p we need bigger values. Let's use d = a large number.
    # Query d = 2^31 (just over half of p's range)
    # result = (a * 2^31 + b) % p
    # a * 2^31 can be up to 65535 * 2^31 ≈ 2^48, so it wraps p many times.
    # 
    # Best approach: use GCD method.
    # If we query d1, d2: 
    #   (a*d1+b) % p = r1
    #   (a*d2+b) % p = r2
    #   a*(d1-d2) ≡ r1-r2 (mod p)
    #   p | a*(d1-d2) - (r1-r2)
    # Use multiple d values, compute GCD of all (a*di+b - ri) values.
    
    print("[*] Recovering p via GCD method...")
    test_values = []
    for d in [0, 1, 100, 10000, 100000, 1000000, 10000000, 100000000, 
              0xFFFF, 0xFFFFFF, 0xFFFFFFFF]:
        r = send_recv(sock, {"option": "math_test", "data": d})
        # a*d + b - result = k*p for some integer k
        val = a * d + b - r['result']
        if val > 0:
            test_values.append(val)
    
    from math import gcd
    from functools import reduce
    p = reduce(gcd, test_values)
    print(f"[*] Recovered p={p}")
    
    # Verify p is prime-ish (32-bit)
    if p < 2**31:
        print("[!] p seems too small, might be a factor. Trying to find actual prime...")
        # p might be a factor; do a few more queries to refine
        for d in [0x1FFFFFFF, 0x3FFFFFFF, 0x7FFFFFFF, 0xDEADBEEF, 0xCAFEBABE]:
            r = send_recv(sock, {"option": "math_test", "data": d})
            val = a * d + b - r['result']
            if val > 0:
                test_values.append(val)
        p = reduce(gcd, test_values)
        print(f"[*] Refined p={p}")
    
    print(f"[*] Parameters: a={a}, b={b}, p={p}, s0={s0}")
    
    # Verify: compute_state(a, s0, b, p)
    expected_q = (a * s0 + b) % p
    print(f"[*] Expected first q (for decrypt): {expected_q}")
    
    # Step 2: Padding Oracle Attack
    print("\n[*] Starting padding oracle attack...")
    plaintext = padding_oracle(sock, a, b, p, s0, flag_ct)
    
    print(f"\n[*] Raw plaintext: {plaintext}")
    # Remove PKCS7 padding
    pad_len = plaintext[-1]
    if pad_len <= 16:
        plaintext = plaintext[:-pad_len]
    print(f"[*] FLAG: {plaintext.decode('utf-8', errors='replace')}")
    
    sock.close()

if __name__ == '__main__':
    main()
```





Flag:

`apoorvctf{T1m3_trAv3l_w1ll_n0t_h3lp_w1th_st4t3_crypt0}`
