# Saving The Results

{% tabs %}
{% tab title="Flags" %}
<mark style="color:orange;">**-oN**</mark> - .nmap file extension (normal output)

<mark style="color:orange;">**-oG**</mark> - .gnmap file extension (grepable output)

<mark style="color:orange;">**-oX**</mark> - .xml nmap extension (XML output)

<mark style="color:orange;">**-oA**</mark> - save result in all of formats
{% endtab %}
{% endtabs %}

## save results in all formats

```bash
sudo nmap 10.129.2.28 -oA {set file name} {define file path}
```

{% hint style="warning" %}
<mark style="color:orange;">note:</mark> if no path is specified, nmap will save results in current directory
{% endhint %}

## Convert .XML file to HTML page using xsltproc

```bash
xsltproc target.xml -o target.html
```

{% hint style="warning" %}
note: open the file with browser to see a nice html page
{% endhint %}

## Open a file with browser through terminal

```bash
xdg-open {filename.html} firefox
```

