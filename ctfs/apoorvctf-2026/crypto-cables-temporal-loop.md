# Crypto - Cable's Temporal Loop

```python
#!/usr/bin/env python3

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
