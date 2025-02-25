# Defeating Anti-Forensic Techniques

## Module Objectives

After compromising a system, attackers often try to destroy or hide all traces of their activities; this makes forensic investigation extremely challenging for investigators. The use of various techniques by cyber-criminals to destroy or hide traces of illegal activities and hinder forensic investigation processes are referred to as anti-forensics.

Forensic investigators need to overcome/defeat anti-forensics so that an investigation yields concrete and accurate evidence that helps identify and prosecute the perpetrators.

***

## Understand Anti-forensics and its Techniques

As discussed previously, anti-forensics techniques are used by cyber-criminals to complicate or prevent proper forensics investigation. Cyber-criminals employ these techniques to make evidence collection extremely difficult and to compromise the accuracy of a forensics report.

***

### What is Anti-forensics?

Anti-forensics, also known as counter forensics, is a set of techniques that attackers or perpetrators use in order to avert or sidetrack the forensic investigation process or try to make it much harder. These techniques negatively affect the quantity and quality of evidence from a crime scene, thereby making the forensic investigation process difficult.

Therefore, the investigator might have to perform additional steps in order to fetch the data, thereby causing a delay in the investigation process.

Goals of anti-forensics are listed below:

* Interrupt and prevent information collection
* Make the investigator’s task of finding evidence difficult
* Hide traces of crime or illegal activity
* Compromise the accuracy of a forensics report or testimony
* Delete evidence that an anti-forensics tool has been run

***

### Anti-forensics Techniques

Anti-forensic techniques are the actions and methods that hinder the forensic investigation process in order to protect the attackers and perpetrators from prosecution in a court of law. These techniques act against the investigation process such as detection, collection, and analysis of evidence files and sidetrack the forensic investigators.

Given below are the types of Anti-forensics techniques:

1. Data/File Deletion
2. Password Protection
3. Steganography
4. Data Hiding in File System Structures
5. Trail Obfuscation
6. Artifact Wiping
7. Overwriting Data/Metadata
8. Encryption
9. Program Packers
10. Minimizing Footprint
11. Exploiting Forensics Tool Bugs
12. Detecting Forensics Tool Activities

***

#### Anti-forensics Technique: Data/File Deletion

When a file is deleted from the hard drive, the pointer to the file is removed by the OS and the sectors containing the deleted data are marked as available, which means the contents of the deleted data remain on the hard disk until they are overwritten by new data. Forensic investigators use data recovery tools such as **Autopsy, Recover My Files, EaseUS Data Recovery Wizard Pro, R-Studio, etc.**, to scan the hard drive and analyze the file system for successful data recovery.

***

**What Happens When a File is Deleted in Windows?**

FAT File System:

* The OS replaces the first letter of a deleted file name with a hex byte code: E5h
* E5h is a special tag that indicates that the file has been deleted
* The corresponding cluster of that file in FAT is marked unused, although it will continue to contain the information untill overwritten

NTFS File System

* When a user deletes a file, the OS just marks the file entery as unallocated but does not delete the actual file contents (It does not do this bcs it would take 2 much time, it is way faster just to overwrite)
* The cluster allocated to the deleted file are ,arled as free in the <mark style="color:yellow;">$ BitMap</mark> ($BitMap file is a record of all used and unused clusters)
* The computer now notices those empty clusters and avails that space for storing a new file

Note: On a windows system, performing normal Delete operation sends the file to Recycle Bin. Whereas performing the **Shift+Delete** operation bypasses the Recycle Bin.

***

**Recycle Bin in Windows**

Recycle Bin temporarily stores deleted files. When a user deletes an item, it is sent to Recycle Bin. However, it does not store items deleted from removable media such as a USB drive or network drive. The items present in Recycle Bin still consume hard disk space and are easy to restore. Users can use the restore option in Recycle Bin to retrieve deleted files and send them back to their original location. Even if files are deleted from Recycle Bin, they continue to consume hard disk space until the locations are overwritten by the OS with new data. When Recycle Bin becomes full, Windows automatically deletes the older items. Windows OS assigns one specific space on each hard disk partition for storing files in Recycle Bin. The system does not store larger items in Recycle Bin; rather, it deletes them permanently.

On Windows Vista and later versions, it is located in Drive: <mark style="color:yellow;">$Recycle.Bin\<SID></mark>

***

**Recycle Bin in Windows (Cont’d)**

There is no size limit for Recycle Bin in Vista and later versions of Windows whereas the older versions had a maximum limit of 3.99 GB. Recycle Bin cannot store items larger than its storage capacity. When a user deletes a file or folder, Windows stores all the details of the file, such as its name and the path where it was stored, in INFO2, which is a hidden file found in Recycle Bin.

The OS uses this information to restore the deleted file to its original location. The Recycled hidden folder contains files deleted from a Windows machine.

In earlier versions of Windows, the deleted files were renamed by the OS using the following format: \
D original drive letter of file><#>.>original extension

For example, in the case of a Dxy.ext file in the Recycled folder, “x” denotes the name of drive such as “C,” “D,” and others; “y” denotes the sequential number starting from one; and “ext” is the extension of the original file.

In Windows Vista and later versions, the deleted files are renamed by the OS using the following format: $R<#>.>original extension>, where <#> represents a set of random letters and numbers

At the same time, a corresponding metadata file is created which is named as: $I<#>.>original extension>, where <#> represents a set of random letters and numbers the same as used for $R

The $R and $I files are located at C:$Recycle.Bin\<USER SID>\\&#x20;

$I file contains following metadata:&#x20;

✓ Original file name \
✓ Original file size \
✓ The date and time the file was delete

In Windows versions newer than Vista and XP, the OS stores the complete path and file or folder name in a hidden file called INFO2. This file remains inside the Recycled or Recycler folder and stores information about the deleted files. INFO2 is the master database file and is very crucial for the recovery of data. It contains various details of deleted files such as their original file name, original file size, date and time of deletion, unique identifying number, and the drive number in which the file was stored.

***

**Recycle Bin Forensics**

* The original files pertaining to the $I files are not visible in the Recycle Bin folder when, o $I file is corrupted or damaged o The attacker/insider deletes $I files from the Recycle Bin
* During forensic investigation, the investigator should check for the $R files in the Recycle Bin directory to counter the anti-forensic technique used by the attacker

<figure><img src="../.gitbook/assets/Pasted image 20250113153000.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Pasted image 20250113153014.png" alt=""><figcaption></figcaption></figure>

***

**Recycle Bin Forensics (Con’t)**

If the metadata files related to the original files are not present in the folder, then the investigator can use ‘copy’ command to recover the deleted files (R files)

Command: _copy <$R(or File name)> Destination Directory_\*

In case, the metadata of Recycle Bin is lost, or the data is hidden intentionally by the perpetrator, the investigator can follow above steps to recover the deleted files from Recycle Bin for further analysis

***

#### File Carving

File carving refers to a technique that is used to recover deleted/lost files and fragments of files from the hard disk when file system metadata is missing. By allowing forensic investigators to retrieve files that have been deleted/lost from a hard disk, file carving helps extract valuable artifacts related to a case of cyber-crime for further examination. A perpetrator may also attempt to delete an entire partition from the hard disk and then merge the deleted partition’s unallocated space with the system’s primary partition to prevent an investigator from identifying the lost partition.

For example, a suspect may try to hide an image from detection by investigators by changing the file extension from .jpg to .dll. However, changing the file extension does not change the file header, which can reveal the actual file format. A file format is confirmed as .jpg if it shows “JFIF” in the file header and hex signature as “4A 46 49 46“.

File carving methods may vary based on different elements such as the fragments of data present, deletion technique used, type of storage media, etc. This process depends on information about the format of the existing files of interest. Investigators can analyze the file headers to verify the file format using tools such as 010 Editor, CI Hex Viewer, Hexinator, Hex Editor Neo, Qiew, WinHex, etc.

***

**File Carving on Windows**

Windows tracks its files/folders on a hard drive using the pointers that tells the system where the file begins and ends. When a file/folder is deleted from a drive, the contents related to the file/folder remain on the hard disk, but the pointers to the file are deleted. In other words, the deleted files can be recovered from the hard disk until the sectors containing the contents of the file are overwritten with new data. This allows forensic tools such as Autopsy, Recall My Files, EaseUS Data Recovery Wizard Pro, and R-Studio for Windows, to recover such deleted files/folders from the hard disk. In SSDs, the recovery of deleted files becomes difficult as TRIM is enabled by default.

***

**File Carving on Linux**

In Linux, users can delete files using /bin/rm/ command, wherein the inode pointing to the file is deleted but the file remains on the disk. Hence, the forensic investigator can use third-party tools such as Stellar Phoenix Linux Data Recovery, R-Studio for Linux, TestDisk, PhotoRec, Kernel for Linux Data Recovery, etc., to recover deleted data from the disk. If a user removes a file that is being used by any running processes, the contents of the file would occupy a disk space that cannot be reclaimed by any other files or programs. The second extended file system (ext2) is designed in such a way that it shows several places where data can be hidden. It is noteworthy that if an executable erase itself, its contents can be retrieved from a /proc memory image. The command cp/proc/$PID/exe/tmp/file creates a copy of a file in /tmp. Unlike Windows, Linux can access and retrieve data from a variety of machines. The Linux kernel supports a large number of file systems including VxFS, UFS, HFS, NTFS, and FAT file systems. Some file systems are not readable in a Windows environment, and users can easily recover such files using a bootable Linux distro such as Knoppix. Third-party tools such as Stellar Phoenix Linux Data Recovery, R-Studio for Linux, TestDisk, PhotoRec, and Kernel for Linux Data Recovery can be used to recover deleted files from Linux.

***

**SSD File Carving on Linux**

File System Forensic workstation used: Windows 10

The forensically acquired image from TRIM disabled SSD should be examined using file carving tools such as Autopsy, R-Studio, etc. In Autopsy, the carved data from the forensic evidence file is displayed under the appropriate data source with heading “$CarvedFiles”

<figure><img src="../.gitbook/assets/Pasted image 20250113154248.png" alt=""><figcaption></figcaption></figure>

***

#### Recovering Deleted Partitions

When a user deletes a partition from the disk, the data allocated to the partition is lost, and the deleted partition is converted to unallocated space on the disk. The MBR partition table stores the records of the primary and extended partitions available on the disk. Therefore, whenever a partition is deleted from the disk, the entries pertaining to the partitions are removed from the MBR partition table. An attacker/insider with a malicious intent may delete a partition and merge it with the primary partition.

During forensic investigation, if the investigator cannot find the deleted partition in Disk Management, then the investigator uses third-party tools such as R-Studio and EaseUS Data Recovery Wizard to scan the hard disk to discover and recover the deleted partition and its contents. These automated tools perform full disk scan, looks for deleted partition information and reconstruct the partition table entry for deleted partition.

***

### Anti-forensics Technique: Password Protection

Passwords are important because they are the gateway to most computer systems. Password protection shields information, protects networks, applications, files, documents, etc. from unauthorized users. Many organizations and individuals, who do not want others to access their data, resources, and other products, employ passwords and strong cryptographic algorithms as security measures.

Attackers and intruders use these protection techniques to hide evidence data, prevent reverse engineering of applications, hinder information extraction from network devices, and prevent access to system and hard disk. This can make forensic investigators’ work difficult. Encryption is a preferred technique for deterring forensic analysis. In such cases, investigators can use password cracking tools such as ophcrack and RainbowCrack to circumvent the password protection.

***

#### Password Types

Computing devices can store and transmit passwords as **cleartext, obfuscated, and hashed passwords**, of which only hashed passwords need cracking while the other password types can assist in the cracking phase.\


**Cleartext passwords**&#x20;

* Passwords are sent and stored in plaintext without any alteration&#x20;
* Example: Windows Registry stores automatic logon password in cleartext in the registry, \
  HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon&#x20;
* Investigators can use tools such as Cain and Ettercap to sniff cleartext passwords\


**Obfuscated passwords** &#x20;

* Passwords are stored or communicated after a transformation&#x20;
* When transformation is reversible, password becomes unreadable when user applies an algorithm \
  and on application of reverse algorithm, it returns to cleartext form\


**Password hashes**&#x20;

* Password hashes are a signature of the original password generated using a oneway algorithm. \
  &#x20;  Passwords are hashed using hash algorithms (MD5, SHA, etc.), which are not reversible

***

#### Password Cracking Techniques

There are three popular techniques for password cracking and they are discussed below:

**Dictionary Attacks** In a dictionary attack, a dictionary file is loaded into the cracking application that runs against user accounts. A dictionary is a text file that contains several dictionary words or predetermined character combinations. The program uses every word present in the dictionary to find the password. Dictionary attacks are more useful than a bruteforce attack. However, this attack does not work against a system that uses passphrases or passwords not contained within the dictionary used.

This attack is applicable in two situations:

* In cryptanalysis, it helps find out the decryption key for obtaining the plaintext from the ciphertext
* In computer security, it helps avoid authentication and access to a computer by guessing passwords

Methods to improve the success of a dictionary attack:

* Using more number of dictionaries, such as technical dictionaries and foreign dictionaries, can help retrieve the correct password
* Using string manipulation; for example, if the dictionary contains the word, “system,” then try string manipulation and use “metsys” and others

**Brute-Forcing Attacks**

Cryptographic algorithms must be hard enough to prevent a brute-force attack. RSA defines brute-force attack as “\[a] basic technique for trying every possible key in turns until the correct key is identified.” A brute-force attack is essentially a cryptanalytic attack used to decrypt any encrypted data (which may be referred to as a cipher). In other words, it involves testing all possible keys in an attempt to recover the plaintext, which is the base for producing a particular ciphertext.

Brute-force attacks need more processing power compared to other attacks. The detection of keys or plaintext at a rapid pace in a brute-force attack results in breaking of the cipher. A cipher is secure if no method exists to break it other than a brute-force attack. Mostly, all ciphers lack mathematical proof of security.

Some considerations for brute-force attack are as follows:

* It is a time-consuming process
* It can eventually trace all passwords
* An attack against Networking Technology (NT) hash is much harder against LAN Manager (LM) hash

**Rule-based Attack**

Attackers use a rule-based attack when they know some credible information about the password such as rules of setting the password, algorithms involved, or the strings and characters used in its creation. For example, if the attackers know that the password contains a two- or three-digit number, then they will use some specific techniques for faster extraction of the password.

By obtaining useful information, such as use of numbers, the length of password, and special characters, the attacker can enhance the performance of the cracking tool and thereby reduce the time required for retrieving the password. This technique involves brute-force, dictionary, and syllable attacks (a syllable attack combines both a brute-force attack and a dictionary attack and is often used to crack those passwords which do not consist of an actual word but a mix of characters and syllables).

The attackers may use multiple dictionaries, brute-force techniques, or simply try to guess the password.

***

### Anti-forensics Technique: Steganography

One of the shortcomings of various detection programs is their primary focus on streaming text data. What if an attacker bypasses normal surveillance techniques and is still able to steal or transmit sensitive data? In a typical situation, after an attacker manages to get inside a firm as a temporary or contract employee, he/she surreptitiously seeks out sensitive information. While the organization may have a policy that does not allow removable electronic equipment in the facility, a determined attacker can still find ways to do so using techniques such as steganography.

Steganography refers to the art of hiding data “behind” other data without the target’s knowledge, thereby hiding the existence of the message itself. It replaces bits of unused data in usual files such as graphic, sound, text, audio, video, etc. with some other surreptitious bits. The hidden data can be plaintext or cipher text, or it can be an image. Utilizing a graphic image as a cover is the most popular method to conceal data in files. Steganography is preferred by attackers because, unlike encryption, it is not easy to detect.

For example, attackers can hide a keylogger inside a legitimate image; therefore, when the victim clicks on the image, the keylogger captures the victim’s keystrokes.

Attackers also use steganography to hide information when encryption is not feasible. In terms of security, it hides the file in an encrypted format so that even if the attacker decrypts it, the message will remain hidden. Attackers can insert information such as source code for a hacking tool, list of compromised servers, plans for future attacks, communication and coordination channel, etc. The forensic investigator examines the Stego-Object to extract the hidden information.

***

### Anti-forensics Technique: Alternate Data Streams

ADS, or an alternate data stream, is a NTFS file system feature that helps users find a file using alternate metadata information such as author title. It allows files to have more than one stream of data, which are invisible to Windows Explorer and require special tools to view. Thus, ADS makes it easy for perpetrators to hide data within files and access them when required. Attackers can also store executable files in ADS and execute them using the command line utility. ADS allows attacker to hide any number of streams into one single file without modifying the file size, functionality, etc., except the file date. However, the file date can be modified using antiforensics tools like TimeStomp. In some cases, these hidden ADS can be used to remotely exploit a web server.

An ADS contains metadata such as access timestamps, file attributes, etc. Investigators need to find ADS and extract the information present in it. As the system cannot modify ADS data, retrieving it can offer raw details of the file and execution of malware.

Apart from using the above-mentioned methods, investigators can also use software tools to identify ADS files and extract the additional streams.

***

### Anti-forensics Technique: Trail Obfuscation

The purpose of trail obfuscation is to confuse and mislead the forensics investigation process.

Attackers mislead investigators via log tampering, false e-mail header generation, timestamp modification, and various file headers’ modification.

Attackers can do this by using various tools and techniques such as those listed below: \
✓ Log cleaners \
✓ Zombie accounts\
✓ Spoofing \
✓ Trojan commands \
✓ Misinformation

Traffic content obfuscation can be attained by means of VPNs and SSH tunneling. In this process, the attackers delete or modify metadata of some important files in order to confuse the investigators. They modify modify header information and file extensions using various tools.

***

#### Anti-forensics Technique: Trail Obfuscation (Cont’d)

Timestomp, which is part of the Metasploit Framework, is a trail obfuscation tool that attackers use to modify, edit, and delete the date and time metadata on a file and make it useless for the investigators. Transmogrify is another example of such a tool. • Timestomp

Timestomp is one of the most widely used trail obfuscation tools that allow deletion or modification of timestamp-related information on files. Procedure to defeat this technique is covered in “Detecting Overwritten Data/Metadata” section.

***

### Anti-forensics Technique: Artifact Wiping

Artifact wiping refers to the process of deleting or destroying the evidence files permanently using file-wiping and disk-cleaning utilities and disk degaussing/destruction techniques. The attacker permanently eliminates particular files or the file system itself.

1. **Disk-wiping Utilities** Disk wiping involves erasing data from the disk by deleting its links to memory blocks and overwriting the memory contents. In this process, the application overwrites the contents of MBR, partition table and other sectors of the hard drive with characters such as null character or any random character several times (using data wiping standards). In this case, the forensic investigator finds it difficult to recover data from the storage device. Some of the commonly used disk wiping utilities include BCWipe Total WipeOut, CyberScrub cyberCide, DriveScrubber, ShredIt, etc.
2. File Wiping Utilities These utilities delete individual files from an OS in a short span and leave a much smaller signature when compared with the disk-cleaning utilities. However, some experts believe that many of these tools are not effective, as they do not accurately or completely wipe out the data and also require user involvement. The commonly used file-wiping utilities are BCWipe, R-Wipe & Clean, CyberScrub Privacy Suite, etc.

***

### Anti-forensics Technique: Overwriting Data/Metadata

Perpetrators use different techniques to overwrite data or metadata (or both) on a storage media thereby posing a challenge to the investigators while performing data recovery.

Overwriting programs (disk sanitizers) work in three modes: \
✓ Overwrite entire media \
✓ Overwrite individual files \
✓ Overwrite deleted files on the media

**Overwriting Metadata** \
❑ Investigators use metadata to create a timeline of the perpetrator’s actions by arranging all the computer’s timestamps in a sequence \
❑ Although attackers can use tools to wipe the contents of media, that action itself might draw the attention of investigators. Therefore, attackers cover their tracks by overwriting the metadata (i.e. access times), rendering the construction of timeline difficult. \
❑ Ex: Timestomp (part of the Metasploit Framework) is used to change MACE (Modified- Accessed-Created-Entry) attributes of the file

***

### Anti-forensics Technique: Encryption

Encryption is an effective way to secure data that involves the process of translating data into a secret code that only authorized personnel can access. To read the encrypted file, users require a secret key or a password that can decrypt the file. Due to its effectiveness and ease of usage, most attackers prefer to use encryption techniques for anti-forensics. Intruders use strong encryption algorithms to encrypt data of investigative value, which renders it virtually unreadable without the designated key. Some algorithms are capable of averting the investigation processes by performing additional functions including use of a key file, full-volume encryption, and plausible deniability.

Listed below are the built-in encryption utilities provided by Microsoft for Windows 7 and later: • BitLocker—encrypts an entire volume • Encrypting File System (EFS)—encrypts individual files and directories

***

## Discuss Anti-forensics Countermeasures

Anti-forensics Countermeasures Investigators can overcome the anti-forensic techniques discussed in this module through improved monitoring of devices and using upgraded CFTs. Some of the important countermeasures against anti-forensic techniques are listed below:

• Train and educate the forensic investigators about anti-forensics \
• Validate the results of examination using multiple tools \
• Impose strict laws against illegal use of anti-forensics tools \
• Understand the anti-forensic techniques and their weaknesses \
• Use latest and updated CFTs and test them for vulnerabilities \
• Save data in secure locations \
• Use intelligent decompression libraries to defend against compression bombs \
• Replace weak file identification techniques with stronger ones

Note: It is best not to completely depend on specific tools, as the tools themselves are not immune to attacks.

***

### Anti-forensics Tools

Some anti-forensics tools are listed as follows:&#x20;

* Steganography Studio (http://stegstudio.sourceforge.net)&#x20;
* CryptaPix (https://www.briggsoft.com)&#x20;
* GiliSoft File Lock Pro (http://gilisoft.com) wbStego (https://wbstego.wbailer.com)&#x20;
* Data Stash (https://www.skyjuicesoftware.com)&#x20;
* OmniHide PRO (https://omnihide.com)&#x20;
* Masker (http://softpuls.weebly.com)&#x20;
* DeepSound (http://jpinsoft.net)&#x20;
* DBAN (https://dban.org)&#x20;
* east-tec InvisibleSecrets (https://www.east-tec.com)
