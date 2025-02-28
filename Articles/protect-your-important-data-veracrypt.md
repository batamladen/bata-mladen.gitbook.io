---
cover: .gitbook/assets/veracrypt.jpg
coverY: 90.59436913451512
layout:
  cover:
    visible: true
    size: full
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# 👁️‍🗨️ Protect Your Important Data (VeraCrypt)

Are you extremely paranoid and even though you encrypted your files, you are still scared that it might get revealed somehow? Than, continue reading.

In VeraCrypt, next to the option to create a regular encrypted partition, there is a option to create a "Hidden Volume" within the standard one.

The main usage of this hidden part of the encrypted partition is to store the data that you actually want to encrypt and hide, and the Outer Volume" is a decoy.\
Lets say someone forces you to decrypt the partition, you simply enter the password that will unlock the outer volume, and not the hidden one. Just put some fake important docs in the outer volume.

***

## How safe is it?

VeraCrypt uses AES encryption algorithm. When analyzed, the hidden volume can not be seen because it is developed to be a part of the whole encrypted partition, that is - if set the size of the partition to be 10MB, within those 10MB you would specify the amount of MB to be separated for the hidden part.



This is how it is explained on the official website:

> Even when the outer volume is mounted, it should be impossible to prove whether there is a hidden volume within it or not\*, because free space on _any_ VeraCrypt volume is always filled with random data when the volume is created\*\* and no part of the (unmounted) hidden volume can be distinguished from random data. Note that VeraCrypt does not modify the file system (information about free space, etc.) within the outer volume in any way.



<figure><img src=".gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

This feature is awesome hence it provides **plausible deniability** (even if someone forces you to reveal the password for the outer volume, the existence of the hidden partition remains undetectable because of the random data that is persistent in the free space of any VeraCrypt partition)**.**

***

## How to make a hidden encrypted partition?

### Text

#### <mark style="color:yellow;">Step 1</mark>

Open VeraCrypt and click "Create Volume". You will be prompted with 3 options:

1. Create a volume that will be somewhere among other files and needs to be mannualy mounted.
2. Encrypt a external partition such as a flash drive.
3. Encrypt a whole partition or whole drive.

Most likely you would go for the first one.

***

#### <mark style="color:yellow;">Step 2</mark>

Select "Hidden Volume" and select the location you would want the volume to be. Place it somewhere where there are a lot of other files, somewhere deep. Give it a name, a misleading one.\
A bad example of a volume name would be "Encrypted" - are you dumb? Give it a name that would be boring to the eye, something that you would skip even if you see it.

In the video below, the location and name are awful for simplicity and understandment.

***

#### <mark style="color:yellow;">Step 3</mark>

Next, select the encryption and hash algorithm. The default one will do good.

***

#### <mark style="color:yellow;">Step 4</mark>

Set the outer volume size (the hidden volume will be within this size so don't make it too small)

***

#### <mark style="color:yellow;">Step 5</mark>

Set up a password. And you guessed it - don't let it be a stupid one. \
\
As a matter of fact, don't even make it a complex one. Make it a PASSPHRASE.\
A long 3 4 word passphrase that you will remember. It is a lot safer because it is longer and would be harder to brute force rather that setting up a complex one with a lot of symbols and upper cases. The passphrase still beats this, even it the passphrase is all in lower cases.

Select file system and "full format" and  go crazy with that mouse.

***

#### <mark style="color:yellow;">Step 6</mark>

Add the decoy documents

***

#### <mark style="color:yellow;">Step 7</mark>

Select the encryption and hash algorithm for the hidden volume. The default ones will do good.

***

#### <mark style="color:yellow;">Step 8</mark>

Set the size of the hidden volume.\
It will be in the boundaries of the outer volume.

***

#### <mark style="color:yellow;">Step 9</mark>

Password for the hidden volume. \
When you mount the volume, what password you enter - that partition will open.

***

#### <mark style="color:yellow;">Step 10</mark>

Select filesystem

***

#### <mark style="color:yellow;">Step 11</mark>

Done. The last thing is to populate the hidden part with the files that you want. Enjoy.

***

### Video

{% embed url="https://files.catbox.moe/bgadkq.mp4" %}

***

## Disclaimer

Have in mind, that no matter how new, complex or safe does a software/feature seem - <mark style="color:red;">**it can and will be exploited eventually**</mark>.

Hackers, techniques and tools are getting more advanced every day. That's why it is important to patch all the gaps before its too late.
