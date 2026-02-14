# Crypto - Leonine Misbegotten

### Challenge Summary

The challenge gives you a huge file. Looking at `chall.py`, we see that the flag is **encoded 16 times** in layers using a random choice of one of four encoding schemes:

* Base16 (`b16encode`)
* Base32 (`b32encode`)
* Base64 (`b64encode`)
* Base85 (`b85encode`)

After each encoding, the program **appends the SHA-1 hash of the data before encoding**.

The `output.txt` file is therefore:

```
ENCODED_DATA + SHA1
ENCODED_DATA + SHA1
...
```

And the goal is to recover the original flag.

***

### Solve.py

```python
from base64 import b16decode, b32decode, b64decode, b85decode
from hashlib import sha1

SCHEMES = [b16decode, b32decode, b64decode, b85decode]

with open("output", "rb") as f:
    data = f.read()

for round in range(16):
    new_data = None
    checksum = data[-20:]  # SHA-1 is 20 bytes
    candidate = data[:-20]

    for dec in SCHEMES:
        try:
            decoded = dec(candidate)
            if sha1(decoded).digest() == checksum:
                new_data = decoded
                break
        except Exception:
            pass

    if new_data is None:
        raise Exception("Failed at round", round)

    data = new_data

print(data.decode())

```



Flag:\
`0xfun{p33l1ng_l4y3rs_l1k3_an_0n10n}`

