# Crypto - Ice Creamer

Description:\
Elia’s swamped with algebra but craving a new ice-cream flavor, help him crack these equations so he can trade books for a cone!

`nc cramer.ctf.pascalctf.it 5002`<br>

We have main.py from the chall.

```python
import os
from random import randint

def generate_variable():
    flag = os.getenv("FLAG", "pascalCTF{REDACTED}") # The default value is a placeholder NOT the actual flag
    flag = flag.replace("pascalCTF{", "").replace("}", "")
    x = [ord(i) for i in flag]
    return x

def generate_system(values):
    for _ in values:
        eq = []
        sol = 0
        for i in range(len(values)):
            k = randint(-100, 100)
            eq.append(f"{k}*x_{i}")
            sol += k * values[i]

        streq = " + ".join(eq) + " = " + str(sol)
        print(streq)


def main():
    x = generate_variable()
    generate_system(x)
    print("\nSolve the system of equations to find the flag!")

if __name__ == "__main__":
    main()
```



When nc to the challenge we get a long output of equations.

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>



plan for this challenge was to:\
\- Copy the equations from the server output\
\- Build matrix A (coefficients) and vector b (right-hand side)\
\- Solve the linear system\
\- Convert each solution x\_i to a character\
\- Wrap the result in pascalCTF{}



#### Solve.py

```
import re
import numpy as np

equations = open("equations.txt").read().strip().splitlines()

A = []
b = []

for line in equations:
    left, right = line.split("=")
    b.append(int(right.strip()))

    coeffs = [0] * 28
    terms = left.split("+")
    for t in terms:
        k, var = t.strip().split("*")
        idx = int(var.replace("x_", ""))
        coeffs[idx] = int(k)

    A.append(coeffs)

A = np.array(A, dtype=np.int64)
b = np.array(b, dtype=np.int64)

# Solve system
x = np.linalg.solve(A, b)
x = [int(round(v)) for v in x]

flag = "".join(chr(v) for v in x)
print("pascalCTF{" + flag + "}")
```



Note: THe server output was first put in a file "equations.txt".



FLAG:

pascalCTF{0h\_My\_G0DD0\_too\_much\_m4th\_:O}

