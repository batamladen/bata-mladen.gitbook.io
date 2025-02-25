---
description: (Oracle Transparent Network Substrate)
---

# Oracle TNS

Port: 1521,1522 (can be changed)

***

## **About**

The **Oracle Transparent Network Substrate** (TNS) server is a communication protocol that facilitates communication between Oracle databases and applications over networks.

TNS supports various networking protocols between Oracle databases and client applications, such as `IPX/SPX` and `TCP/IP` protocol stacks.

It is a popular opinion in managing large and complex databases. In addition to the security it has a encryption mechanism ensuringing security of the data transmitted. Over time, TNS has been updated to support newer technologies, including `IPv6` and `SSL/TLS` encryption

Oracle TNS is often used with other Oracle services like Oracle DBSNMP, Oracle Databases, Oracle Application Server, Oracle Enterprise Manager, Oracle Fusion Middleware, web servers, and many more.

***

## **Footprinting**

The TNS inclued a few basic security futures such as username/password authentication, and the listener will use Oracle Net Services to encrypt the communication between the client and the server.

The configuration files for TNS are tnsnames.ora and listener.ora. which are tippicaly located in `$ORACLE_HOME/network/admin`.

**tnsnames.ora** file contains the necessary information for clients to connect to the service.

**listener.ora** file is a server-side configuration file that defines the listener process's properties and parameters, which is responsible for receiving incoming client requests and forwarding them to the appropriate Oracle database instance.

In short, the client-side Oracle Net Services software uses the `tnsnames.ora` file to resolve service names to network addresses, while the listener process uses the `listener.ora` file to determine the services it should listen to and the behavior of the listener.

The oracle databases can be protected by using a so called  so-called PL/SQL Exclusion List (PlsqlExclusionList). Whitch is basically a user-defined blacklist that needs to be placed in $ORACLE\_HOME/sqldeveloper.

***

### **Vulnerabilities**

There have been made many changes for the default installation of Oracle services. For example, Oracle 9 has a default password, `CHANGE_ON_INSTALL`, whereas Oracle 10 has no default password set. The Oracle DBSNMP service also uses a default password, `dbsnmp`. Another example would be that many organizations still use the `finger` service together with Oracle, which can put Oracle's service at risk and make it vulnerable when we have the required knowledge of a home directory.

A nice to have tool for Oracle TNS is ODAT (Oracle Database Attacking Tool), a pentest enumeration tool for oracle databases.

Here is a nice lil bash script to download the ODAT

```python
#!/bin/bash

sudo apt-get install libaio1 python3-dev alien -y
git clone https://github.com/quentinhardy/odat.git
cd odat/
git submodule init
git submodule update
wget https://download.oracle.com/otn_software/linux/instantclient/2112000/instantclient-basic-linux.x64-21.12.0.0.0dbru.zip
unzip instantclient-basic-linux.x64-21.12.0.0.0dbru.zip
wget https://download.oracle.com/otn_software/linux/instantclient/2112000/instantclient-sqlplus-linux.x64-21.12.0.0.0dbru.zip
unzip instantclient-sqlplus-linux.x64-21.12.0.0.0dbru.zip
export LD_LIBRARY_PATH=instantclient_21_12:$LD_LIBRARY_PATH
export PATH=$LD_LIBRARY_PATH:$PATH
pip3 install cx_Oracle
sudo apt-get install python3-scapy -y
sudo pip3 install colorlog termcolor pycrypto passlib python-libnmap
sudo pip3 install argcomplete && sudo activate-global-python-argcomplete
```

***

### Enumeration

nmap scan&#x20;

```bash
sudo nmap -p1521 -sV 10.129.204.235 --open
```

nmap scan with NDE script&#x20;

```bash
sudo nmap -p1521 -sV 10.129.204.235 --open --script oracle-sid-brute
```

usage of ALL modules from odat tool&#x20;

```bash
./odat.py all -s 10.129.204.235
```

To connect to a oracle sql we use SQL Plus. Here is the syntax for connecting using sqlplus:

```sh
sqlplus username/password@ip_address/SID
```

***

**To navigate through a database we will need to know some commands:**

[Database SQL Language Quick Reference](https://docs.oracle.com/cd/E11882_01/server.112/e41085/sqlqraa001.htm#SQLQR985)

