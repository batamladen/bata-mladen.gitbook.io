# Nmap Scripting Engine

## default script

```bash
sudo nmap {Target IP} -sC
```

## scpecific scripts category

```bash
sudo nmap {Target IP} --script {script name}
```

## defined scripts

```bash
sudo nmap {Target IP} --script {script name},{script name}
```

```bash
sudo nmap 10.129.2.28 -p 25 --script banner,smtp-commands
```

## agressive scan

```bash
sudo nmap 10.192.2.28 -p 80 -A
```

## **vulnerability assesment**

### vuln category

```bash
sudo nmap 10.129.2.28 -p 80 -sV --script vuln
```
