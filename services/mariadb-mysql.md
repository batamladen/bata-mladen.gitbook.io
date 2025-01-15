---
description: 'port: 3306'
---

# MariaDB/MySQL

## What is MariaDB?

MariaDB is an open-source relational database management system (RDBMS) that is a fork of MySQL. It was created by the original developers of MySQL after concerns arose over Oracle Corporation's acquisition of MySQL. MariaDB aims to maintain high compatibility with MySQL, ensuring that users and applications can easily transition from MySQL to MariaDB without significant changes.

The commands are the same as for MySQL.

***

## Installation

Install MariaDB

```sh
sudo apt install mariadb-server
```

Enable MariaDB

```bash
sudo systemctl enable mariadb
```

Start MariaDB

```bash
sudo systemctl start mariadb
```

***

## Connect to MariaDB

### Local

```bash
mysql -u root -pP4SSw0rd -h 10.129.14.128
```

### Remote

```bash
mysql -h <Hostname> -u root
mysql -h <Hostname> -u root@localhost
```

-u : user\
-p : password\
-h : host ip

***

## Footprinting



***

## Commands

{% tabs %}
{% tab title="Start" %}
```sql
show databases;    # see all databases on the device
use <database>;    # use a database
connect <database>;    # 
show tables;    # display tables in current database
describe <table_name>;    # see detailed info about the table
show columns from <table>;    # see detailed info about table  
```
{% endtab %}

{% tab title="Create Table" %}
```
CREATE TABLE <table name> (
    <Column Name> <Type>,
    <Column Name> <Type>
);
```

column datatypes:

VARCHAR(50) : \
INT :\
DATETIME :\
REAL :&#x20;

***

### Example

```sql
CREATE TABLE SHIPS (
    NAME VARCHAR(50),
    CLASS VARCHAR(50),
    LAUNCHED INT
);

CREATE TABLE BATTLES (
    NAME VARCHAR(50),
    DATE DATETIME
);

CREATE TABLE CLASSES (
    CLASS VARCHAR(50),
    TYPE VARCHAR(2),
    COUNTRY VARCHAR(20),
    NUMGUNS INT,
    BORE REAL,
    DISPLACEMENT INT
);

CREATE TABLE OUTCOMES (
    SHIP VARCHAR(50),
    BATTLE VARCHAR(50),
    RESULT VARCHAR(10)
);

```
{% endtab %}

{% tab title="Insert In Tables" %}
```sql
INSERT INTO <table_name> (<column>, <column>, <column>) VALUES
('values', 'values', values),
('values', 'values', values),
('values', 'values', values);
```

***

Example:

```sql
INSERT INTO SHIPS (NAME, CLASS, LAUNCHED) VALUES
('USS Constitution', 'Constitution', 1797),
('HMS Victory', 'Victory', 1765),
('USS Arizona', 'Pennsylvania', 1915);

INSERT INTO BATTLES (NAME, DATE) VALUES
('Battle of Tripoli', '1804-08-03'),
('Battle of Trafalgar', '1805-10-21'),
('Pearl Harbor', '1941-12-07');

INSERT INTO CLASSES (CLASS, TYPE, COUNTRY, NUMGUNS, BORE, DISPLACEMENT) VALUES
('Constitution', 'BB', 'USA', 90, 1.2, 2200),
('Victory', 'BB', 'UK', 104, 1.4, 3500),
('Pennsylvania', 'BB', 'USA', 100, 1.3, 3300);

INSERT INTO OUTCOMES (SHIP, BATTLE, RESULT) VALUES
('USS Constitution', 'Battle of Tripoli', 'Victory'),
('HMS Victory', 'Battle of Trafalgar', 'Victory'),
('USS Arizona', 'Attack on Pearl Harbor', 'Sunk');
```
{% endtab %}

{% tab title="undefined" %}
```sql
SELECT * FROM <table_name>;
```

***

Example:

```sql
SELECT * FROM SHIPS;
SELECT * FROM BATTLES;
SELECT * FROM CLASSES;
SELECT * FROM OUTCOMES;
```
{% endtab %}

{% tab title="Untitled" %}
```sql
SELECT SHIPS.NAME, SHIPS.CLASS, CLASSES.TYPE, CLASSES.COUNTRY
FROM SHIPS
JOIN CLASSES ON SHIPS.CLASS = CLASSES.CLASS;
```


{% endtab %}
{% endtabs %}









