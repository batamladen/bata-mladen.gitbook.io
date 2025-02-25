# Service Enumeration

## get status message of nmap scan every 5 sec

```bash
sudo nmap 10.129.2.28 -p- -sV --stats-every=5s
```

## get a message the moment the open port is found

```bash
sudo nmap 10.129.2.28 -p- -sV -v
```

***

## **Banner grabbing**

### with nmap

```bash
sudo nmap 10.129.2.28 -p- -sV
```

but nmap sometimes does not handle the info well

so we can mannualy connect using nc , grab the banner, and intercept network traffic with tcpdump in the background **(first set up tcpdump and than connect with nc)**

### with nc

```bash
nc -nv 10.129.2.28 25
```

intercept the network traffic with tcpdump

```bash
sudo tcpdump -i eth0 host 10.10.14.2 and 10.129.2.28
```

