# Forensics/ Operation Pensieve Breach - 1

### Description:

The SOC of the Ministry of Magic received multiple critical alerts from the Domain Controller.\
\
Everything seems to be out of control.\
\
![](https://heroctf.fr-par-1.linodeobjects.com/DC01-DCSync-meme.png)\
\
It seems that a critical user has been compromised and is performing nasty magic using the DCsync spell.\
\
You're mandated to investigate the Principal Domain Controller event logs to find:\
\- sAMAccountName (lowercase) of the compromised account performing bad stuff.\
\- Timestamp of the beginning of the attack, format: DD/MM/YYYY-11:22:33 SystemTime.\
\- Source IP address used for this attack.\
\- The last legitimate IP used to login before the attack.



### Walkthrough

We are provided with logs from the domain controller logs.

There are a bunch of them but looking at the size most were empty, except the security logs, so we open them first.



#### Task 1: sAMAccountName (lowercase) of the compromised account performing bad stuff

Since DCSync is detected with event 4662, thats exactly what we search for. (A good dcsznc detection article: [https://blog.blacklanternsecurity.com/p/detecting-dcsync](https://blog.blacklanternsecurity.com/p/detecting-dcsync) )

We see onlt 3 logs and all tree are by the same user.

<figure><img src="../../.gitbook/assets/image (37).png" alt=""><figcaption></figcaption></figure>

**Answer**: albus.dumbledore



#### Timestamp of the beginning of the attack, format: DD/MM/YYYY-11:22:33 SystemTime.

Same log just search in "Details" to see the SystemTime and not our local.

**Answer:** 22/11/2025-23:13:41



#### Source IP address used for this attack

Filter for successfull logons with the same logonID as 4662 logs, since the ip is not shown in this logs. (0x420B75).

Looking around the time we saw the attack started we find the ip address of albus account.\
**Answer**: 192.168.56.200



#### The last legitimate IP used to login before the attack.

Sort by time and search 4225 event logs before the attacks time and search for the last logged in user.\
We see that was neville.longbottom with the ip 192.168.56.230

**Answer**: 192.168.56.230



### Flag

Hero{albus.dumbledore;22/11/2025-23:13:41;192.168.56.200;192.168.56.230}



### <br>

