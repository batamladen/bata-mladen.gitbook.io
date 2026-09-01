# Fixed XOR

## Task

Write a function that takes two equal-length buffers and produces their XOR combination.

If your function works properly, then when you feed it the string:

```
1c0111001f010100061a024b53535009181c
```

... after hex decoding, and when XOR'd against:

```
686974207468652062756c6c277320657965
```

... should produce:

```
746865206b696420646f6e277420706c6179
```



## Solve

```python
buffer_1 = input("Enter the first hex string: ")
buffer_2 = "686974207468652062756c6c277320657965"


def fixed_xor(hex_string1, hex_string2):
    #convert hex to bytes
    byte1 = bytes.fromhex(hex_string1)
    byte2 = bytes.fromhex(hex_string2)

    print(byte1)
    print(byte2)

    #perform XOR operation
    xor_result = bytes(a ^ b for a, b in zip(byte1, byte2))

    return xor_result.hex()

print(fixed_xor(buffer_1, buffer_2))

```
