# Host and Port Scanning

## Scan top 10 ports

```bash
sudo nmap 10.129.2.28 --top-ports=10
```

## Trace the Packets

```bash
sudo nmap 10.129.2.28 -p 21 --packet-trace -Pn -n --disable-arp-ping
```

## TCP connect scan

```bash
sudo nmap 10.129.2.28 -p 443 --packet-trace --disable-arp-ping -Pn -n --reason -sT
```

## UDP Port Scan

```bash
sudo nmap 10.129.2.28 -F -sU
```

