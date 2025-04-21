# GPP Passwords (Group Policy Preferences)

* **SYSVOL** is a shared folder on all Domain Controllers that stores logon scripts and Group Policy data.
* GPP (introduced in Windows Server 2008) allowed storing **credentials** (usernames + encrypted passwords) in XML files for things like scheduled tasks.
* These XML files are stored in: **\<DOMAIN>\SYSVOL\<DOMAIN>\Policies**
* The encryption key used is **the same for all AD environments** and was made public by Microsoft.
* Because **Authenticated Users** (including normal users and computers) can read SYSVOL, **anyone can access and decrypt** the stored passwords.

<figure><img src="../../../.gitbook/assets/aes key.png" alt=""><figcaption></figcaption></figure>

This is now the encrypted pass looks like:\
!\[\[enc pass.png]]

***

## Attack

To abuse `GPP Passwords`, we will use the [Get-GPPPassword](https://github.com/PowerShellMafia/PowerSploit/blob/master/Exfiltration/Get-GPPPassword.ps1) function from `PowerSploit`, which automatically parses all XML files in the Policies folder in `SYSVOL`, picking up those with the `cpassword` property and decrypting them once detected

### Import GPPPassword

```powershell-session
PS C:\Users\bob\Downloads> Import-Module .\Get-GPPPassword.ps1
```

### Execute GPPPasword

```powershell-session
PS C:\Users\bob\Downloads> Get-GPPPassword
```

***

## Prevention

* In 2014, Microsoft released **patch KB2962486** to stop storing passwords in GPP.
* **Patched systems won't store new passwords**, but **old ones may still exist**.
* Many AD environments, even after 2015, still have credentials in SYSVOL.
* **Important:** The patch **does not remove existing passwords**, only blocks new ones.
* Regularly check and clean SYSVOL to ensure no credentials are exposed.

***

## Detection

* There is no need for anyone to open the XML file that contains the creds, so if someone does, an event will be created.
* EventID **4663**
* Or we can set logon types (`4624` (`successful logon`), `4625` (`failed logon`), or `4768` (`TGT requested`)) based if the password is up to date or not

***

## Honeypot

* We can use the SYSVOL share for a trap set-up.
* A semi-privileged use with a **wrong password**
* Service account passwords are often **old** and rarely changed.
* If the **GPP file is newer** than the last password change, the password is likely still valid.
* If the password changed **after** the file was modified, it probably won’t work.
* Run a **dummy task** with the account to generate recent login activity.

4771: failed pre-authentiaction\
4776: bad password

***

## Tasks

### Task 1

**Connect to the target and run the Powersploit Get-GPPPassword function. What is the password of the svc-iis user?**

When we run the import module command we will get and error, this is a security feature of windows that restricts script executions. We bypass it with the command below

<figure><img src="../../../.gitbook/assets/Pasted image 20250407114515.png" alt=""><figcaption></figcaption></figure>

Now we import and execute the script

<div align="left"><figure><img src="../../../.gitbook/assets/Pasted image 20250407114609.png" alt=""><figcaption></figcaption></figure></div>

<figure><img src="../../../.gitbook/assets/Pasted image 20250407114631.png" alt=""><figcaption></figcaption></figure>

**Answer: abcd@123**

***

### Task 2

After running the previous attack, connect to DC1 (172.16.18.3) as 'htb-student:HTB\_@cademy\_stdnt!' and look at the logs in Event Viewer. What is the Access Mask of the generated events?

From windows RDP connect and go to EventView\
Enter in the Security logs in filter the event ID 4663 and search the Details for access mask

**Answer: 0X80**
