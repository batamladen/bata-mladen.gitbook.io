# Host Discovery

## Scan Network Range

```shell
 sudo nmap 10.129.2.0/24 -sn -oA tnet | grep for | cut -d" " -f5
```

{% hint style="warning" %}
<mark style="color:orange;">**note:**</mark> this scanning method works only if the firewalls of the hosts allow it. Otherwise, we can use other scanning techniques to find out if the hosts are active or not.
{% endhint %}

## Scan active hosts from IP List

```bash
sudo nmap -sn -oA tnet -iL hosts.lst | grep for | cut -d" " -f5
```

{% hint style="warning" %}
<mark style="color:orange;">**note:**</mark> if we are provided with hosts that need to be tested (hosts.lst) use the predefined list to scan them.
{% endhint %}

## Scan Multiple IPs

```
sudo nmap -sn 10.129.2.18 10.129.2.19 10.129.2.20
sudo nmap -sn 10.129.2.18-20
```

{% hint style="warning" %}
<mark style="color:orange;">**note:**</mark> sometimes we wont need to scan the whole network.
{% endhint %}

## Check if host is up

```bash
sudo nmap -sn 10.129.2.18
```

{% hint style="warning" %}
<mark style="color:orange;">**note:**</mark> if we dont use port scan (-sn) nmap defaultly sends ICMP ping scans () and not ICMP echo requests.
{% endhint %}

