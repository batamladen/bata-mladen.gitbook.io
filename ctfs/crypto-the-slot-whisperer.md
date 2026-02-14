# Crypto - The Slot Whisperer

We were given a remote service:

```
nc chall.0xfun.org 37246
```

It prints 10 numbers and asks us to predict the next 5 spins.

We are also given the source code:

```python
class SlotMachineLCG:
    def __init__(self, seed=None):
        self.M = 2147483647
        self.A = 48271
        self.C = 12345
        self.state = seed if seed is not None else 1

    def next(self):
        self.state = (self.A * self.state + self.C) % self.M
        return self.state

    def spin(self):
        return self.next() % 100
```

***

***

## 🧪 Solver Script

```python
M = 2147483647
A = 48271
C = 12345

observed = [53,69,99,55,16,48,45,80,10,6]

for k in range(M // 100):
    state = 100*k + observed[0]

    ok = True
    tmp = state

    for i in range(1, len(observed)):
        tmp = (A * tmp + C) % M
        if tmp % 100 != observed[i]:
            ok = False
            break

    if ok:
        print("Found state:", state)

        # predict next 5
        for _ in range(5):
            tmp = (A * tmp + C) % M
            print(tmp % 100, end=" ")
        break
```
