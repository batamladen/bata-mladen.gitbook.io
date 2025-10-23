---
description: CTF made by Stanislav Atanasov
---

# Linux Forensics

#### The zip file that was for download contained:

ctf.E01\
ctf.E02\
ctf.E03\
ctf.E04\
ctf.E05\
ctf.E06\
ctf.E07\
ctf.E08\
ctf.E09\
ctf.E10\
ctf.E11\
ctf.E12\
ctf.E13



#### Questions to answer:

1. What is the MD5 Checksum of the case?&#x20;
2. What is the case number?
3. How many partitions are in the archive?&#x20;
4. What is the password for the LUKS?&#x20;
5. What is the OS and version?&#x20;
6. What is the second command executed by root?&#x20;
7. What is the username of the normal user for the system?&#x20;
8. What is the flag hidden in the root’s directory?&#x20;
9. What is displayed if you access the apache server (and I don’t mean the default page)?&#x20;
10. Which is the user’s favorite Social network (there is a logo in one of the folders)

***

## UTP CTF

The CTF consists of 10 questions that need to be answered. No question can be answered without answering the one before.

I am doing this ctf on my Kali Linux machine.

And we begin...

***

### Pre-Requisits

... after downloading the zip file from the cloud drive, and unziping the files inside it, we get a file called UTP CTF with 13 images inside.

&#x20;

first, we want to make sure we have the right tools.

Since we are working with .e01,02... witch are all parts of one image, we want to have the right tools for examination.\


I will use ewf for analyzing, examination. (Another popular option on linux is autopsy)\


To download the tools we run:

```sh
sudo apt-get install ewf-tools
```

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

***

### Examination

Now we can examine the data associated with the image by running:

```shell
ewfinfo /home/kali/Downloads/UTP\ CTF/ctf.E01
```

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
When all the parts of the image are extracted in the same folder, the OS will se it as one whole image, so you only need to specify the first part of the image to refer to the whole. (ctf.E01)
{% endhint %}



Here we can see a ton of ussefull information that will be useful for further investigation, and our first answers to the questionaree.

The case number is 755585

Notes on some passwords.

MD5 checksum

&#x20;

So we acctually found the answers to the first 2 questions.

<mark style="color:yellow;">1. What is the MD5 Checksum of the case? –</mark> <mark style="color:yellow;"></mark><mark style="color:yellow;">**0df5c2b36344631471556be619367c21**</mark>

<mark style="color:yellow;">2. What is the case number? –</mark> <mark style="color:yellow;"></mark><mark style="color:yellow;">**755585**</mark>



***

### Raw Image Mount

Lets create a mount point that we will use to mount the E01 as a raw device

```sh
mkdir -p /mnt/ewf_mount
```

This creates a directory in the mnt, called "ewf\_mount"

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

Mount the E01 image:

```sh
sudo ewfmount /home/kali/Downloads/UTP\ CTF/ctf.E01 /mnt/ewf_mount
```

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

The "ewfmount 20140814" output means that the mount was successful.



***

### 3. How many partitions are in the archive?

After mounting the E01 image, we can utalize the mmls command to examine the partition structure of the mounted image:

```sh
mmls /mnt/ewf_mount/ewf1
```

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

The output provided a breakdown of the partition layout for E01, we can see from the output that 004, 005 and 006 are valid partitions.

The 004 partition is the EFI system, 005 and 006 are likely to be filled with user inputed data so we will check them next.

And we see the answer to the 3rsd question

<mark style="color:yellow;">3. How many partitions are in the archive? -</mark> <mark style="color:yellow;"></mark><mark style="color:yellow;">**3**</mark>



***

### 4. What is the password for the LUKS?&#x20;

Next, we attempted to set up a loop device to access one of the partitions using the losetup command:

```sh
losetup -o 537919488 /dev/loop1 /mnt/ewf_mount/ewf1
```

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

Btw the number 537919488 I got using this formula:

> _Calculate the Offset in Bytes: Each sector is 512 bytes, so:_
>
> _Offset for Partition number: start(1050624 in my case for the partition 005) \* 512 = 537919488 bytes_

With the loop device set up, we proceeded to mount the newly configured loop device to access its contents:

```sh
mount -o ro /dev/loop1 /mnt/logical_mount
```

And remember that we configured only the 005 partition on the loop device.

If the command is executed successfuly you should be able to see some content of the partition:

```sh
ls /mnt/logical_mount
```

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

Since the next question is the LUKS password, lets open the 006 partition, this one is not encrypted.

Lets mount the 006 partition. We repeat the losetup and mount commands.

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

The loop3 is a loop device that we will place our partition on, the partition is defined by the start \* 512, and the /mnt/ewf\_mount/ewf1 is our whole raw image.

As seen on the picture, this partition is encrypted and we need to decrypt it before we can mount it.

Install the neccesary packages for opening the luks encrypted partition:

<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

&#x20;Using the installed package for opening, we can see below that we need a passphrase. We saw some hints about it at the beginning.

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

User is lazy. Password is part of the XKCD comics 936.

The hint we have indicates that the password is related to XKCD comic number 936, which is titled "Password Strength." In this comic, it is explained how using a passphrase composed of multipe, easy to remember words, is way more secure than using a complex single password.

The one used in the comic was: correct horse battery staple\


Having in mind the hint from class "_One letter is missing_" i made a wordlist form the phrase and ereased every letter that could be ereased and made around 20-30 length wordlist.

And since I suck at bash – prompted chatGPT to create one for trying the wordlist so I don’t write it mannualy – automation at it’s finest…

<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

And after blankly watching at the screen for 30 minutes, wondering why none of the passphrases were correct i remembered that the USER WAS LAZY. the example for a “bad” password in the comic was Tr0ub4dor&3. now i made this wordlist:

<figure><img src="../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

and the 6th one worked. the password is **Tr0b4dor&3**.

So we know the password, lets decrypt the partition:

```sh
sudo cryptsetup luksOpen /dev/loop2 decrypted_partition
```

Enter passphrase for /mnt/ewf\_mount/ewf1: Tr0b4dor&3

<figure><img src="../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

Now if we check dev/mapper we will see the following content:

<figure><img src="../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

There is our so called “decrypted partition”.\
lets see the partition tree:

<figure><img src="../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

So we can see the "decrypted partition" has some logical volumes, lets mount the vgxubuntu-root and explore it since we have acces to it thanks to the password we entered earlier.

After trying to mount the root LV to mnt/logical\_mount directory i faced some errors like:

<figure><img src="../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

so i checked if the LV were active with _vgchange-ay_

<figure><img src="../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

And checked the filesystem on VG with file:

<figure><img src="../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

it outputs a link to dev/dm-1 so lets check the filesystem there

<figure><img src="../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

its is ext4. So it should work, we proceed with trying to mount from /dev/dm-1

<figure><img src="../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

Nop. Lets just mount with read only as we did in the above examples.

```sh
sudo mount -o ro /dev/dm-1 /mnt/logical_mount
```

<figure><img src="../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

And we can now see the contents of the encrypted partition in mnt/logical\_mount

<figure><img src="../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

***

### 5. What is the OS and version?

So the next questions asks us to tell the OS and version.\
lets seek that info in the etc/os-release

<figure><img src="../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

<mark style="color:yellow;">5. What is the OS and version? –</mark> <mark style="color:yellow;"></mark><mark style="color:yellow;">**Ubuntu 23.10 (Mantic Minotaur)**</mark>



***

### 6. What is the second command executed by root?

The next question is to see what was the second command executed by root.\
To see this we can go in /root and search for it there. its ussualy _.bash\_history_ or _.zsh\_history_.

<figure><img src="../.gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>

<mark style="color:yellow;">6. What is the second command executed by root? -</mark> <mark style="color:yellow;"></mark><mark style="color:yellow;">**apt install apache2**</mark>



***

### 7. What is the username of the normal user for the system?

Now we need to see what is the name of the reguar user for the system.\
We talked about this on the class, when we open the /etc/psswd we can see all the users and info.

<figure><img src="../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

<mark style="color:yellow;">7. The name for the regular user is:</mark> <mark style="color:yellow;"></mark><mark style="color:yellow;">**ctf**</mark>



***

### 8. What is the flag hidden in the root’s directory?

The flag hidden in the root dir under /capture me: <mark style="color:yellow;">**e0NURl9MRl9Zb3VfR290X01lIX0=**</mark>

{% hint style="warning" %}
The flag needs to be decrypted
{% endhint %}

***

### 9. What is displayed if you access the apache server (and I don’t mean the default page)?

What is displayed in the apache server, and not the default page is the next question, from the command history we could see that there were some commands for apache and php.&#x20;

If we go to var/www/html/index.php we can se the display is: <mark style="color:yellow;">**CTF\_LF\_php**</mark>



***

<figure><img src="../.gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>

***

### 10. Which is the user’s favorite Social network (there is a logo in one of the folders)

the last question is to write what is the users favorite social media, there is a logo somewhere.\
We can see it in the /home/ctf

The answer is: <mark style="color:yellow;">**Twitter**</mark>

<figure><img src="../.gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>
