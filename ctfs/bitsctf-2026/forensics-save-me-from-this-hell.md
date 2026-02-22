# Forensics - save me from this hell

### Description

"The smoke rises, carrying secrets within... Where there's smoke, there's fire..."

We intercepted an encrypted transmission from a mysterious server. The data appears to be lost, but rumor has it some ancient legend is somewhere in this image.

The original sender was obsessed with a programming language from hell itself - one where code mutates, logic inverts, and sanity is optional and insanity is permanence.



### Files Provided

{% file src="../../.gitbook/assets/SaveMeFromThisHell.zip" %}



### Walkthrough

we are provided with a single image:

<figure><img src="../../.gitbook/assets/Marlboro.jpg" alt=""><figcaption></figcaption></figure>



First as prolly everyone i runned exiftool on the image

```
bata@HP:~/Downloads/SaveMeFromThisHell$ exiftool Marlboro.jpg 
ExifTool Version Number         : 12.76
File Name                       : Marlboro.jpg
Directory                       : .
File Size                       : 4.7 MB
File Modification Date/Time     : 2026:01:23 14:49:14+01:00
File Access Date/Time           : 2026:02:20 11:53:21+01:00
File Inode Change Date/Time     : 2026:02:20 11:53:16+01:00
File Permissions                : -rw-r--r--
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
JFIF Version                    : 1.02
Resolution Unit                 : inches
X Resolution                    : 72
Y Resolution                    : 72
Profile CMM Type                : Linotronic
Profile Version                 : 2.1.0
Profile Class                   : Display Device Profile
Color Space Data                : RGB
Profile Connection Space        : XYZ
Profile Date Time               : 1998:02:09 06:49:00
Profile File Signature          : acsp
Primary Platform                : Microsoft Corporation
CMM Flags                       : Not Embedded, Independent
Device Manufacturer             : Hewlett-Packard
Device Model                    : sRGB
Device Attributes               : Reflective, Glossy, Positive, Color
Rendering Intent                : Perceptual
Connection Space Illuminant     : 0.9642 1 0.82491
Profile Creator                 : Hewlett-Packard
Profile ID                      : 0
Profile Copyright               : Copyright (c) 1998 Hewlett-Packard Company
Profile Description             : sRGB IEC61966-2.1
Media White Point               : 0.95045 1 1.08905
Media Black Point               : 0 0 0
Red Matrix Column               : 0.43607 0.22249 0.01392
Green Matrix Column             : 0.38515 0.71687 0.09708
Blue Matrix Column              : 0.14307 0.06061 0.7141
Device Mfg Desc                 : IEC http://www.iec.ch
Device Model Desc               : IEC 61966-2.1 Default RGB colour space - sRGB
Viewing Cond Desc               : Reference Viewing Condition in IEC61966-2.1
Viewing Cond Illuminant         : 19.6445 20.3718 16.8089
Viewing Cond Surround           : 3.92889 4.07439 3.36179
Viewing Cond Illuminant Type    : D50
Luminance                       : 76.03647 80 87.12462
Measurement Observer            : CIE 1931
Measurement Backing             : 0 0 0
Measurement Geometry            : Unknown
Measurement Flare               : 0.999%
Measurement Illuminant          : D65
Technology                      : Cathode Ray Tube Display
Red Tone Reproduction Curve     : (Binary data 2060 bytes, use -b option to extract)
Green Tone Reproduction Curve   : (Binary data 2060 bytes, use -b option to extract)
Blue Tone Reproduction Curve    : (Binary data 2060 bytes, use -b option to extract)
Image Width                     : 3903
Image Height                    : 5854
Encoding Process                : Progressive DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:2:0 (2 2)
Image Size                      : 3903x5854
Megapixels                      : 22.8

```

Nothing much. Than we tried strings and after the image ends with FFD9, there is some gibberish after it indicating a hidden file attached to the image.&#x20;

We run binwalk.

```
bata@HP$ binwalk Marlboro.jpg

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             JPEG image data, JFIF standard 1.02
382           0x17E           Copyright string: "Copyright (c) 1998 Hewlett-Packard Company"
3754399       0x39499F        Zip archive data, at least v2.0 to extract, compressed size: 982911, uncompressed size: 982761, name: smoke.png
4737377       0x484961        Zip archive data, at least v1.0 to extract, compressed size: 422, uncompressed size: 422, name: encrypted.bin
4738032       0x484BF0        End of Zip archive, footer length: 22

```

There are 2 files attached to the image, smoke.png and encrypted.bin Download them and extract.

```
bata@HP:~/Downloads/SaveMeFromThisHell$ binwalk -e Marlboro.jpg

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             JPEG image data, JFIF standard 1.02
382           0x17E           Copyright string: "Copyright (c) 1998 Hewlett-Packard Company"
3754399       0x39499F        Zip archive data, at least v2.0 to extract, compressed size: 982911, uncompressed size: 982761, name: smoke.png
4737377       0x484961        Zip archive data, at least v1.0 to extract, compressed size: 422, uncompressed size: 422, name: encrypted.bin
4738032       0x484BF0        End of Zip archive, footer length: 22

```

Opening the image we get this:

<figure><img src="../../.gitbook/assets/smoke.png" alt=""><figcaption></figcaption></figure>



Check the image with the file command to see aditional info:

```
bata@HP:~/Downloads/SaveMeFromThisHell/_Marlboro.jpg.extracted$ file smoke.png
smoke.png: PNG image data, 800 x 600, 8-bit/color RGB, non-interlaced

```

Idk but chatgpt says 800x600 8-bit/color is ideal for stego. Random fact plug.

Anyways...

Exiftool that image and we get a base64 string in "Author"

```
bata@HP:~/Downloads/SaveMeFromThisHell/_Marlboro.jpg.extracted$ exiftool smoke.png
ExifTool Version Number         : 12.76
File Name                       : smoke.png
Directory                       : .
File Size                       : 983 kB
File Modification Date/Time     : 2026:01:23 13:43:38+01:00
File Access Date/Time           : 2026:02:20 12:02:47+01:00
File Inode Change Date/Time     : 2026:02:20 12:02:28+01:00
File Permissions                : -rw-r--r--
File Type                       : PNG
File Type Extension             : png
MIME Type                       : image/png
Image Width                     : 800
Image Height                    : 600
Bit Depth                       : 8
Color Type                      : RGB
Compression                     : Deflate/Inflate
Filter                          : Adaptive
Interlace                       : Noninterlaced
Author                          : aHR0cHM6Ly96YjMubWUvbWFsYm9sZ2UtdG9vbHMv
Image Size                      : 800x600
Megapixels                      : 0.480

```

Decrypt the b64:

```
bata@HP$ echo aHR0cHM6Ly96YjMubWUvbWFsYm9sZ2UtdG9vbHMv | base64 -d
https://zb3.me/malbolge-tools/
```

Its a link to a Malbolge language interpreter, i didnt even know this language exists wtf.

Anyways we will keep the website open, and if you run zsteg on the same image you will eventualy get a decryption key for the decrypted.bin.

```
bata@HP:~/Downloads/SaveMeFromThisHell/_Marlboro.jpg.extracted$ zsteg -a smoke.png

meta Author         .. text: "aHR0cHM6Ly96YjMubWUvbWFsYm9sZ2UtdG9vbHMv"
imagedata           .. file: MIPSEL-BE MIPS-III ECOFF executable - version -2.-2
b1,r,lsb,xy         .. text: "RfWWN\nvn&FSnCWcN#j"
b1,rgb,lsb,xy       .. text: "# Marlboro Decryption Key\n# Format: 32-byte XOR key in hexadecimal\nKEY=c7027f5fdeb20dc7308ad4a6999a8a3e069cb5c8111d56904641cd344593b657\n# Usage: XOR each byte of encrypted.bin with key[i % 32]\np"

```

so we got the key: KEY = c7027f5fdeb20dc7308ad4a6999a8a3e069cb5c8111d56904641cd344593b657

The challenge is telling us:

> Decrypt encrypted.bin using repeating XOR with this 32-byte key.

We prompt this to chat so he writes us a small python script to xor the encrypted.bin with the key.

solve.py:

```
from itertools import cycle

key_hex = "c7027f5fdeb20dc7308ad4a6999a8a3e069cb5c8111d56904641cd344593b657"
key = bytes.fromhex(key_hex)

with open("encrypted.bin", "rb") as f:
    data = f.read()

decrypted = bytes([b ^ k for b, k in zip(data, cycle(key))])

with open("decrypted.bin", "wb") as f:
    f.write(decrypted)

print("Done.")
```

Run the script and we get decrypted.bin. See what we got:

```
bata@HP:~/Downloads/SaveMeFromThisHell/_Marlboro.jpg.extracted$ cat decrypted.bin 
Content-Type: text/x-malbolge
Interpreter: https://zb3.me/malbolge-tools/#interpreter
Language: Malbolge

D'`$##\!<5|jWVT/eRcONp_oJJHHF!3gfBc!?a_{)(xwYon4rqpi/mlkdibaf_%]ba`_XWVz=YRQPOsSRQ3ONGkKD,BAe(DCBA@?>7[5432V6v.3,P*).-&Jk)"!&}C#cy~wv<zyxZpotm3qSi/gOkd*KJ`ed]\"`Y^]\[TxRQVOTSLpPONMLEiIH*)?>bB$@987[Z:38765.Rsr*/.-&%$#G'~f$#z@~}|{t:[wvonm3kSonmlkdihg`&GFbaZ~^@VUTYRQPtNMLKPImGFKJCBf)E>bB;@?>7[5432Vw/43,POp.'&+*#G!~D1

```

paste this string in the interpreter and hope for the best.

```
D'`$##\!<5|jWVT/eRcONp_oJJHHF!3gfBc!?a_{)(xwYon4rqpi/mlkdibaf_%]ba`_XWVz=YRQPOsSRQ3ONGkKD,BAe(DCBA@?>7[5432V6v.3,P*).-&Jk)"!&}C#cy~wv<zyxZpotm3qSi/gOkd*KJ`ed]\"`Y^]\[TxRQVOTSLpPONMLEiIH*)?>bB$@987[Z:38765.Rsr*/.-&%$#G'~f$#z@~}|{t:[wvonm3kSonmlkdihg`&GFbaZ~^@VUTYRQPtNMLKPImGFKJCBf)E>bB;@?>7[5432Vw/43,POp.'&+*#G!~D1
```

Aaaanwe got it:

<figure><img src="../../.gitbook/assets/Pasted image 20260220123315.png" alt=""><figcaption></figcaption></figure>



FLAG:

```
BITSCTF{d4mn_y0ur_r34lly_w3n7_7h47_d33p}
```



10/10 chall shoutout to the author Aur0r4. Had hella fun and learned.
