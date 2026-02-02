---
description: I just discovered bitwise operators, so I guess 1 XOR 1 = 1?
---

# Crypto - XorD

The flag is encrypted byte-by-byte using XOR with a random key. The Python random module is used with a fixed seed (1337), making the keystream predictable.

XOR is reversible.\
random.seed(1337) makes the random numbers deterministic\
The same random keystream can be regenerated to decrypt the flag



We have xord.py and output.txt from the challenge.

#### xord.py

```
import os
import random

def xor(a, b):
    return bytes([a ^ b])

flag = os.getenv('FLAG', 'pascalCTF{REDACTED}')
encripted_flag = b''
random.seed(1337)

for i in range(len(flag)):
    random_key = random.randint(0, 255)
    encripted_flag += xor(ord(flag[i]), random_key)

with open('output.txt', 'w') as f:
    f.write(encripted_flag.hex())
```

#### output.txt

```
cb35d9a7d9f18b3cfc4ce8b852edfaa2e83dcd4fb44a35909ff3395a2656e1756f3b505bf53b949335ceec1b70e0
```



#### Solve.py

```
import random

cipher_hex = "cb35d9a7d9f18b3cfc4ce8b852edfaa2e83dcd4fb44a35909ff3395a2656e1756f3b505bf53b949335ceec1b70e0"
cipher = bytes.fromhex(cipher_hex)

random.seed(1337)

flag = ""
for b in cipher:
    flag += chr(b ^ random.randint(0, 255))

print(flag)
```



FLAG: \
pascalCTF{1ts\_4lw4ys\_4b0ut\_x0r1ng\_4nd\_s33d1ng}
