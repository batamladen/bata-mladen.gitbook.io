# power leak

**Category:** Forensics

***

### Challenge Description

> Power reveals the secret.\
> `EHAX{SHA256(secret)}`

We were given `power_traces.csv`.\
power\_traces.csv

***

### Understand the data

Opening the CSV revealed 5 columns:

```
position, guess, trace_num, sample, power_mW
```

* **position** — 6 positions (0–5), one per character of the secret
* **guess** — digits 0–9 being tested at each position
* **trace\_num** — 20 traces per guess
* **sample** — 50 power samples per trace
* **power\_mW** — the measured power consumption in milliwatts

This is a textbook **Differential Power Analysis (DPA)** setup. A device (e.g. a microcontroller checking a PIN) was measured while testing each digit guess at each position. The correct guess causes the device to do more work, which leaks as higher power consumption variance.

***

### Solution

The key insight: when the correct digit is tested, the device's power draw becomes more variable across traces compared to wrong guesses. So we simply find the guess with the **highest variance** at each position. Prompted chat for a quick script.

```python
import pandas as pd
import numpy as np
import hashlib

df = pd.read_csv('power_traces.csv')

secret = []
for pos in range(6):
    pos_df = df[df['position'] == pos]
    guess_vars = {}
    for g in range(10):
        g_df = pos_df[pos_df['guess'] == g]
        guess_vars[g] = g_df['power_mW'].var()
    best = max(guess_vars, key=guess_vars.get)
    secret.append(str(best))

secret_str = ''.join(secret)
flag = 'EHAX{' + hashlib.sha256(secret_str.encode()).hexdigest() + '}'
print(f'Secret: {secret_str}')
print(f'Flag: {flag}')
```

**Output:**

```
Position 0: guess 7  (var=69.06)
Position 1: guess 9  (var=68.97)
Position 2: guess 2  (var=68.21)
Position 3: guess 9  (var=69.28)
Position 4: guess 6  (var=67.24)
Position 5: guess 3  (var=67.65)

Secret: 792963
```

SHA256 the secret to get the flag format:

```
SHA256("792963") = 5bec84ad039e23fcd51d331e662e27be15542ca83fd8ef4d6c5e5a8ad614a54d
```

***

### Flag

```
EHAX{5bec84ad039e23fcd51d331e662e27be15542ca83fd8ef4d6c5e5a8ad614a54d}
```
