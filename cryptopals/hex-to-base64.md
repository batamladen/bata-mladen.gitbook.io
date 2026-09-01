# Hex to Base64

## Task

String:

```
49276d206b696c6c696e6720796f757220627261696e206c696b65206120706f69736f6e6f7573206d757368726f6f6d
```

Should produce:

```
SSdtIGtpbGxpbmcgeW91ciBicmFpbiBsaWtlIGEgcG9pc29ub3VzIG11c2hyb29t
```

So go ahead and make that happen. You'll need to use this code for the rest of the exercises.



{% hint style="info" %}
#### Cryptopals Rule

Always operate on raw bytes, never on encoded strings. Only use hex and base64 for pretty-printing.
{% endhint %}



## Solve

```python
import base64

def hex_to_base64(hex_string):
    
    byte_data = bytes.fromhex(hex_string)
    
    base64_encoded = base64.b64encode(byte_data)
    
    return base64_encoded.decode('utf-8')

hex_value = input("Enter a hex string: ")
print(hex_to_base64(hex_value))
```





