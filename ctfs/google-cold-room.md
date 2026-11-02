# Google Cold Room

## Deceptive Click

### Description

Curated Detections in the Denver Security Operations Center triggered on a case titled _"PowerShell Download And Execute"_. Tracing the adversary's actions revealed an initial drive-by compromise through a malicious browser update (FakeUpdates), in-memory process hollowing, internal Active Directory Kerberoasting, lateral movement across Windows servers, and data staging of corporate financial audits.

***

### Questions and Solutions



1. What is the hostname of the compromised endpoint in the Denver office? (expected format: FAKE-HOST-NAME-000.false.name)

Find the case and inspect its details, relying on gemini's summary we get the hostname where it all started - CYM-DEN-W10-042.CYMBAL.CORP.\
But the hostname is not accepted for the flag

<figure><img src="../.gitbook/assets/image (39).png" alt=""><figcaption></figcaption></figure>



Same solution would be searching through the alerts that the case included.

<figure><img src="../.gitbook/assets/image (40).png" alt=""><figcaption></figcaption></figure>



**Answer:** CYM-DEN-W10-042.cymbal.corp

time of download = 2026-10-15 20:55:50\
user = marcus.vance\
hostname = CYM-DEN-W10-042.cymbal.corp

***

2. The GTI Malware Profile states that FAKEUPDATES is delivered via compromised third-party websites injecting malicious JavaScript. By analyzing web proxy logs around the time of the download, what is the domain of the compromised blog that served as the referer for the malicious update archive? (expected format: domainname.com)

```
principal.user.userid = "marcus.vance" AND
metadata.event_type = "NETWORK_HTTP"
```

we check the event type network http for the user marcus.vence and in the grouped fields section we look for all the domains that are included in the results, inspection the ones that sound weird or suspicious, and once we inspect marketingtrends-insider.com only

<figure><img src="../.gitbook/assets/image (41).png" alt=""><figcaption></figcaption></figure>

The time was just a few minutes after the process creation event so it would make sense that this was the compromised website/blog that has the malicous js

<figure><img src="../.gitbook/assets/image (43).png" alt=""><figcaption></figcaption></figure>

mhm, you bet.

<figure><img src="../.gitbook/assets/image (44).png" alt=""><figcaption></figcaption></figure>

**answer:** marketingtrends-insider.com

***

3. The secondary payload d27e15eb.exe achieved privilege elevation by hollowing into a SYSTEM process. What was the Target Process ID of the specific svchost.exe instance injected into on the compromised endpoint? (expected format: 000)

Process hollowing/injection into svchost.exe maps to Sysmon Event ID 8 (CreateRemoteThread), and sometimes Event ID 10 (ProcessAccess).

<figure><img src="../.gitbook/assets/image (45).png" alt=""><figcaption></figcaption></figure>

The expected answer is 3 symbols long so we can already guess witch of the provided is the right answer.

**Answer:** 624

***

4. Following domain reconnaissance, the adversary conducted Kerberoasting against the Domain Controller (CYM-DC-01) to harvest service credentials. What is the full Service Principal Name (SPN) requested in the Kerberos TGS ticket? (expected format: XXXXXXX/FAKE-HOST-NAME-00.false.name:0000)

After domain recon, the attacker used the compromised account `marcus.vance` to request a Kerberos service ticket (TGS) for a service account. Any domain user can do this, and the ticket is encrypted with the service account’s password hash, so it can be cracked offline. On the DC this shows up as Windows Security Event ID 4769. I searched `metadata.product_event_type = "4769"` filtered on the compromised user (and the DC as target), opened the single result, and read the requested SPN from `target.application` in the Event Viewer:

```
metadata.product_event_type = "4769" AND
(target.hostname = /CYM-DC-01/ nocase OR principal.hostname = /CYM-DC-01/ nocase OR principal.user.userid = /marcus.vance/ nocase)
```

**answer:** MSSQLSvc/CYM-DB-01.cymbal.corp:1433

<figure><img src="../.gitbook/assets/image (46).png" alt=""><figcaption></figcaption></figure>

***

5. Our threat hunting reveals the attacker switched identities using a cracked Kerberos service account. What username was used for subsequent lateral movement? (expected format: username)

After Kerberoasting, the attacker authenticated as the cracked service account **svc\_backup**. The Kerberoast event (4769) listed the service SID ending in `-1088`. Searching for non-`marcus.vance` logins from the workstation IP (`10.240.32.115`) found a 4624 Logon Type 3 (NTLM) to the File Server `CYM-FS-01` with the same SID, which confirmed the identity switch.

```
principal.ip = "10.240.32.115" AND
NOT principal.user.userid = "marcus.vance" AND
metadata.event_type = "USER_LOGIN"
```

<figure><img src="../.gitbook/assets/image (47).png" alt=""><figcaption></figcaption></figure>

**Answer:** svc\_backup

***

6. What is the file name of the compressed zip archive created by the attacker on the file server to stage sensitive financial audit files? (expected format: File\_Name.extension)

After moving laterally to the file server as `svc_backup`, the attacker used PowerShell to compress sensitive financial audit files into `C:\Windows\Temp\Financial_Audit_2026.zip`. I found it by searching `FILE_CREATION` events on `CYM-FS-01` for paths ending in `.zip`

```
principal.hostname = "CYM-FS-01.cymbal.corp" AND
metadata.event_type = "FILE_CREATION" AND
target.file.full_path = /\.zip$/ nocase
```

<figure><img src="../.gitbook/assets/image (48).png" alt=""><figcaption></figcaption></figure>

**Answer:** Financial\_Audit\_2026.zip

***

With that question answered we finished the first out of the tree part scenario for this Cold Room case.

Edit: the only one since I didn't find the time to do the others.

