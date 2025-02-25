---
description: (Infrastructure Based Enumeration)
---

# Domain Information

{% tabs %}
{% tab title="Key Notes" %}
* This type of information is gathered passively without direct and active scans. In other words, we remain hidden and navigate as "customers" or "visitors" to avoid direct connections to the company that could expose us.
* &#x20;The first thing we should do is scrutinize the company's `main website`
* Then, we should read through the texts, keeping in mind what technologies and structures are needed for these services
* The first point of presence on the Internet may be the `SSL certificate` from the company's main website. Often, such a certificate includes more than just a subdomain, and this means that the certificate is used for several domains, and these are most likely still active
* Another source to find more subdomains is [crt.sh](https://crt.sh/).
{% endtab %}
{% endtabs %}



## Save output from crt.sh in JSON format

```bash
curl -s https://crt.sh/\?q\=inlanefreight.com\&output\=json | jq
```

## Sort output by name from crt.sh from the JSON format

```bash
curl -s https://crt.sh/\?q\=inlanefreight.com\&output\=json | jq . | grep name | cut -d":" -f2 | grep -v "CN=" | cut -d'"' -f2 | awk '{gsub(/\\n/,"\n");}1;' | sort -u
```

Next, we can identify the hosts directly accessible from the Internet and not hosted by third-party providers (This is because we are not allowed to test the hosts without the permission of third-party providers)

## Company hosted servers

```bash
for i in $(cat subdomainlist);do host $i | grep "has address" | grep inlanefreight.com | cut -d" " -f1,4;done 
```

Once we see which hosts can be investigated further, we can generate a list of IP addresses with a minor adjustment to the `cut` command and run them through Shodan

## Shodan - IP list

```bash
for i in $(cat subdomainlist);do host $i | grep "has address" | grep inlanefreight.com | cut -d" " -f4 >> ip-addresses.txt;done
for i in $(cat ip-addresses.txt);do shodan host $i;done
```

Now, we can display all the available DNS records where we might find more hosts.

## DNS records

```bash
dig any inlanefreight.com(domain name)
```
