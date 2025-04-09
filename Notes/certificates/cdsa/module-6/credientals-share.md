# Credientals Share

We moved from "don't leave a sticky note with your password on your computer to dont leave your passwords in a shared file"

We often find credentials in network shares within scripts and configuration files.

***

## Attack

* The first step is to see what shared files exist.
* We can use [Invoke-ShareFinder](https://github.com/darkoperator/Veil-PowerView/blob/master/PowerView/functions/Invoke-ShareFinder.ps1) to do this
* The final output contains a list of non-default shares that the current user account has at least read access to

### Find shared files

```powershell-session
PS C:\Users\bob\Downloads> Invoke-ShareFinder -domain eagle.local -ExcludeStandard -CheckShareAccess
```

If we see a share with a dollar sign, the file explorer wont show the contents because of the dollar sign.

<figure><img src="../../../.gitbook/assets/file share.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/file share 2.png" alt=""><figcaption></figcaption></figure>

However, since we have the `UNC` path from the output, if we browse to it, we will be able to see the contents inside the share:

<figure><img src="../../../.gitbook/assets/fileshare 3.png" alt=""><figcaption></figcaption></figure>

A few automated tools exist, such as [SauronEye](https://github.com/vivami/SauronEye), which can parse a collection of files and pick up matching words. But if there are a few, like in our scenariu, we can do it manually (Living of the Land) and use the findstr command

### Search for Passwords in Scripts/Config Files on Network Share

```powershell-session
PS C:\Users\bob\Downloads> cd \\Server01.eagle.local\dev$
PS Microsoft.PowerShell.Core\FileSystem::\\Server01.eagle.local\dev$> findstr /m /s /i "pass" *.bat
PS Microsoft.PowerShell.Core\FileSystem::\\Server01.eagle.local\dev$> findstr /m /s /i "pass" *.cmd
PS Microsoft.PowerShell.Core\FileSystem::\\Server01.eagle.local\dev$> findstr /m /s /i "pass" *.ini
setup.ini
PS Microsoft.PowerShell.Core\FileSystem::\\Server01.eagle.local\dev$> findstr /m /s /i "pass" *.config
4\5\4\web.config
```

* `/s` forces to search the current directory and all subdirectories
* `/i` ignores case in the search term
* `/m` shows only the filename for a file that matches the term. We highly need this in real production environments because of the huge amounts of text that get returned. For example, this can be thousands of lines in PowerShell scripts that contain the `PassThru` parameter when matching for the string `pass`.
* The `term` that defines what we are looking for. Good candidates include `pass`, `pw`, and the `NETBIOS` name of the domain. In the playground environment, it is `eagle`. Attractive targets for this search would be file types such as `.bat`, `.cmd`, `.ps1`, `.conf`, `.config`, and `.ini`. Here's how `findstr` can be executed to display the path of the files with a match that contains `pass` relative to the current location

If we remove the "/m" it will display the exact file location:

```powershell-session
PS Microsoft.PowerShell.Core\FileSystem::\\Server01.eagle.local\dev$> findstr /m /s /i "pw" *.config

5\2\3\microsoft.config
PS Microsoft.PowerShell.Core\FileSystem::\\Server01.eagle.local\dev$> findstr /s /i "pw" *.config
5\2\3\microsoft.config:pw BANANANANANANANANANANANANNAANANANANAS
```

<figure><img src="../../../.gitbook/assets/fileshare 4.png" alt=""><figcaption></figcaption></figure>

### Search for Domain/User Mentions in PowerShell Scripts

```powershell-session
PS Microsoft.PowerShell.Core\FileSystem::\\Server01.eagle.local\dev$> findstr /m /s /i "eagle" *.ps1

2\4\4\Software\connect.ps1
PS Microsoft.PowerShell.Core\FileSystem::\\Server01.eagle.local\dev$> findstr /s /i "eagle" *.ps1
2\4\4\Software\connect.ps1:net use E: \\DC1\sharedScripts /user:eagle\Administrator Slavi123
```

One obvious and yet uncommon search term is the `NetBIOS` name of the domain. Commands such as `runas` and `net` take passwords as a positional argument on the command line instead of passing them via `pass`, `pw`, or any other term. It is usually defined as `/user:DOMAIN\USERNAME PASSWORD`.

> \[!NOTE] Note\
> Running findstr is noted by Windows Defender

***

## Prevention

* Lock down every share
* Perform scans of the shared files

***

## Detection

* Understanding and analyzing users' behavior is the best detection technique for abusing discovered credentials in shares.
* Best detection method = **understanding normal user behavior**
* Analyze:
  * **Login time**
  * **Login location/device**
* **4624** – Successful login
* **4625** – Failed login
* **4768** – Kerberos TGT request (used in domain logins)

<figure><img src="../../../.gitbook/assets/fislehsare 5.png" alt=""><figcaption></figcaption></figure>

***

## Honeypot

* Use a **semi-privileged service account** (e.g., `svc-iis`)
* Created **2+ years ago**
* Last password change: **at least 1 year ago**
* File with **fake password** must be **newer** than last password change
* Account must still be **active**
* Script/file should look **realistic** (e.g., MSSQL connection string with fake password)
* Since the password is wrong → expect **failed logon attempts**
* Monitor **Windows Event IDs**:
  * **4625** – Failed login
  * **4771** – Kerberos pre-auth failed
  * **4776** – NTLM authentication failed

***

## Task

### Task 1

&#x20;Connect to the target and enumerate the available network shares. What is the password of the Administrator2 user?

Import the Powerview.ps1 script to utilize Invoke-ShareFinder for identifying domain shares, also enable script execution.

<figure><img src="../../../.gitbook/assets/fileshare 6.png" alt=""><figcaption></figcaption></figure>

Search all the shares but we will try first the dev$ share for the Administrator2 password.\
cd into the dev$ share. And run the following command.

```
findstr /m /s /i "Administrator2" *.ps1
```

<figure><img src="../../../.gitbook/assets/file share 7.png" alt=""><figcaption></figcaption></figure>

**Answer: Slavi920**
