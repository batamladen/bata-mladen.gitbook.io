# Overview

**Active directory attacks are:**

* `Kerberoasting`
* `AS-REProasting`
* `GPP Passwords`
* `Misconfigured GPO Permissions (or GPO-deployed files)`
* `Credentials in Network Shares`
* `Credentials in User Attributes`
* `DCSync`
* `Kerberos Golden Ticket`
* `Kerberos Constrained Delegation attack`
* `Print Spooler & NTLM Relaying`
* `Coercing attacks & Kerberos Unconstrained Delegation`
* `Object ACLs`
* `PKI Misconfigurations` - `ESC1`
* `PKI Misconfigurations` - `ESC8` (`Coercing` + `Certificates`)

**Our Invirement Structure:**\
The environment consists of the following machines and their corresponding IP addresses:

* `DC1`: `172.16.18.3`
* `DC2`: `172.16.18.4`
* `Server01`: `172.16.18.10`
* `PKI`: `172.16.18.15`
* `WS001`: `DHCP or 172.16.18.25` (depending on the section)
* `Kali Linux`: `DHCP or 172.16.18.20` (depending on the section)

**Below, you may find guidance (from a Linux host):**

* How to connect to the Windows box WS001
* How to connect to the Kali box
* How to transfer files between WS001 and your Linux attacking machine

***

## Connect to WS001 via RDP

xfreerdp is the same software as the windows machines have just for linux.\
To access the machine, we will use the user account Bob whose password is 'Slavi123'.

```shell
xfreerdp /u:eagle\\bob /p:Slavi123 /v:TARGET_IP /dynamic-resolution
```

***

## Connect to Kali via SSH

we can access the Kali machine via SSH.\
The credentials of the machine are the default 'kali/kali'

```shell
ssh kali@TARGET_IP
```

## Connect to Kali via RDP

If RDP is enabled on the kali machines, always connect using the rdp first.

```shell
xfreerdp /v:TARGET_IP /u:kali /p:kali /dynamic-resolution
```

***

## Moving files between WS001 and your Linux attacking machine

On the WS there are sharable files (right click on a file -> properties -> sharing)

To access the folder from the Kali machine, you can use the 'smbclient' command\
Accessing the folder requires authentication, so you will need to provide credentials.

```shell
smbclient \\\\TARGET_IP\\Share -U eagle/administrator%Slavi123
```

Once connected, you can utilize the commands `put` or `get` to either upload or download files.
