---
description: Domain Name System
---

# DNS

## **What is DNS**

DNS (Domain Name System) plays a crucial role in how the internet functions. The DNS allows us to search for domain names like [www.facebook.com](http://www.facebook.com) and reach the web servers that the hosting provider has assigned the IP address.

With DNS we do not have to remember all the IP addresess for our favorite sites. Its a lot easier to remember the names, that is why DNS is so important.

{% hint style="success" %}
Defnition:\
DNS is a system for resolving computer names into IP addresses, and it does not have a central database.
{% endhint %}

Its like a big phone book where the information is distributed over thousand of name servers globally placed.



### Types of DNS

There are several types of DNS servers used worldwide:

* DNS root server
* Authoritative name server
* Non-authoritative name server
* Caching server
* Forwarding server
* Resolver

<table><thead><tr><th width="192">Server Type</th><th>Description</th></tr></thead><tbody><tr><td>DNS Root Server</td><td>The root servers of the DNS are responsible for the top-level domains (TLD) such as .com, .net... As the last instance, they are only requested if the name server does not respond. Thus, a root server is a central interface between users and content on the Internet, as it links domain and IP address. There are 13 such root servers around the globe.</td></tr><tr><td>Authoritative Nameserver</td><td>Authoritative name servers hold authority for a particular zone. They only answer queries from their area of responsibility, and their information is binding. If an authoritative name server cannot answer a client's query, the root name server takes over at that point.</td></tr><tr><td>Non-authoritative Nameserver</td><td>Non-authoritative name servers are not responsible for a particular DNS zone. Instead, they collect information on specific DNS zones themselves, which is done using recursive or iterative DNS querying.</td></tr><tr><td>Caching DNS Server</td><td>Caching DNS servers cache information from other name servers for a specified period. The authoritative name server determines the duration of this storage.</td></tr><tr><td>Forwarding Server</td><td>Forwarding servers perform only one function: they forward DNS queries to another DNS server.</td></tr><tr><td>Resolver</td><td>Resolvers are not authoritative DNS servers but perform name resolution locally in the computer or router.</td></tr></tbody></table>

The DNS does not only store information about the IP address conected to the domain name, but can hold information such as, what is the host that is served as the email server for this specific domain name. Or what is the name of the host

<figure><img src="../.gitbook/assets/Untitled (2).png" alt=""><figcaption></figcaption></figure>

DNS is mainly unencrypted. Devices on the local WLAN and Internet providers can therefore hack in and spy on DNS queries. Since this poses a privacy risk, there are now some solutions for DNS encryption. By default, IT security professionals apply `DNS over TLS` (`DoT`) or `DNS over HTTPS` (`DoH`) here. In addition, the network protocol `DNSCrypt` also encrypts the traffic between the computer and the name server.

### **records of a DNS setting:**

<table><thead><tr><th width="142">DNS Record</th><th>Description</th></tr></thead><tbody><tr><td>A</td><td>Returns an IPv4 address of the requested domain as a result.</td></tr><tr><td>AAAA</td><td>Returns an IPv6 address of the requested domain.</td></tr><tr><td>MX</td><td>Returns the responsible mail servers as a result.</td></tr><tr><td>NS</td><td>Returns the DNS servers (nameservers) of the domain.</td></tr><tr><td>TXT</td><td>This record can contain various information. The all-rounder can be used, e.g., to validate the Google Search Console or validate SSL certificates. In addition, SPF and DMARC entries are set to validate mail traffic and protect it from spam.</td></tr><tr><td>CNAME</td><td>This record serves as an alias. If the domain www.hackthebox.eu should point to the same IP, and we create an A record for one and a CNAME record for the other.</td></tr><tr><td>PTR</td><td>The PTR record works the other way around (reverse lookup). It converts IP addresses into valid domain names.</td></tr><tr><td>SOA</td><td>Provides information about the corresponding DNS zone and email address of the administrative contact.</td></tr></tbody></table>

### get info for dns nzm

```bash
dig soa www.inlanefreight.com

```

***

## **Default Config**

### **All DNS servers work with three different types of configuration files:**

1. local DNS configuration files
2. zone files
3. reverse name resolution files



### look local DNS configuration on a bind9 server

```bash
root@bind9# cat /etc/bind/named.conf.local
root@bind9# cat /etc/bind/db.domain.com
```

### reverse name resolution zone file

```bash
root@bind9:~# cat /etc/bind/db.10.129.14
```

***

## **Dangerous Settings**

<table><thead><tr><th width="167">Options</th><th>Description</th></tr></thead><tbody><tr><td>allow-query</td><td>Defines which hosts are allowed to send requests to the DNS server.</td></tr><tr><td>allow-recursion</td><td>Defines which hosts are allowed to send recursive requests to the DNS server.</td></tr><tr><td>allow-transfer</td><td>Defines which hosts are allowed to receive zone transfers from the DNS server.</td></tr><tr><td>zone-statistics</td><td>Collects statistical data of zones.</td></tr></tbody></table>

***

## **Footprinting**

The footprinting at DNS servers is done as a result of the requests we send. So, first of all, the DNS server can be queried as to which other name servers are known. We do this using the NS record and the specification of the DNS server we want to query using the `@` character. This is because if there are other DNS servers, we can also use them and query the records. However, other DNS servers may be configured differently and, in addition, may be permanent for other zones.



### dig NS query

```
dig ns inlanefreight.htb @10.129.14.128
```

### dig version query

```
dig CH TXT version.bind 10.129.120.85
```

### view all available records

```
dig any inlanefreight.htb @10.129.14.128
```

{% hint style="warning" %}
<mark style="color:orange;">**Note:**</mark> We can use the option `ANY` to view all available records. This will cause the server to show us all available entries that it is willing to disclose. It is important to note that not all entries from the zones will be shown.
{% endhint %}

### DIG - AXFR Zone Transfer

```
dig axfr inlanefreight.htb @10.129.14.128
```

### DIG - AXFR Zone Transfer - Internal

```
dig axfr internal.inlanefreight.htb @10.129.14.128
```

The individual `A` records with the hostnames can also be found out with the help of a brute-force attack. To do this, we need a list of possible hostnames, which we use to send the requests in order. Such lists are provided, for example, by [SecLists](https://github.com/danielmiessler/SecLists/blob/master/Discovery/DNS/subdomains-top1million-5000.txt).

An option would be to execute a `for-loop` in Bash that lists these entries and sends the corresponding query to the desired DNS server.

### bash script for Subdomain Brute Forcing

```bash
for sub in $(cat /opt/useful/SecLists/Discovery/DNS/subdomains-top1million-110000.txt);do dig $sub.inlanefreight.htb @10.129.14.128 | grep -v ';\|SOA' | sed -r '/^\s*$/d' | grep $sub | tee -a subdomains.txt;done
```

Many different tools can be used for this, and most of them work in the same way. One of these tools is, for example [DNSenum](https://github.com/fwaeytens/dnsenum).

### DNSenum usage

```
dnsenum --dnsserver 10.129.14.128 --enum -p 0 -s 0 -o subdomains.txt -f /opt/useful/SecLists/Discovery/DNS/subdomains-top1million-110000.txt inlanefreight.htb
```

{% hint style="warning" %}
<mark style="color:orange;">**note:**</mark> for host brute forcing we can use the /home/kali/Downloads/SecList/Discovery/DNS/fience-host.txt file
{% endhint %}
