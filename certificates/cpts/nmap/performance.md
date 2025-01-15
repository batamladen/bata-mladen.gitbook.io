# Performance

## **Timeouts**

### default scan

```
sudo nmap 10.129.2.0 -F
```

### optimized RTT

```
sudo nmap 10.129.2.0 -F --initial-rtt-timeout 50ms --max-rtt-timeout 100ms
```

{% hint style="warning" %}
note: setting a small initial RTT can cause of scanning less ip's, for the price of a faster scan.
{% endhint %}

***

## **Max Retries**

### default scan

```
sudo nmap 10.129.2.0/24 -F | grep "/tcp" | wc -l
```

### reduced retries

```
sudo nmap 10.129.2.0/24 -F --max-retries 0 | grep "/tcp" | wc -
```

***

## **Rates**

### default scan

```
sudo nmap 10.129.2.0/24 -F -oN tnet.default
```

### optimized scan

```
sudo nmap 10.129.2.0/24 -F -oN tnet.minrate300 --min-rate 300
```

***

## **Timing**

{% tabs %}
{% tab title="First Tab" %}
<mark style="color:orange;">**`-T 0`**</mark> / `-T paranoid`

<mark style="color:orange;">**`-T 1`**</mark> / `-T sneaky`

<mark style="color:orange;">**`-T 2`**</mark> / `-T polite`

<mark style="color:orange;">**`-T 3`**</mark> / `-T normal` (default in not specified different)

<mark style="color:orange;">**`-T 4`**</mark> / `-T aggressive`

<mark style="color:orange;">**`-T 5`**</mark> / `-T insane`
{% endtab %}
{% endtabs %}

### insane timing script

```
sudo nmap 10.129.2.0/24 -F -oN tnet.T5 -T 5
```

{% hint style="warning" %}
note: the more agressive the scan, the bigger chance we will be blocked bcs of sending a large amount of traffic.
{% endhint %}

