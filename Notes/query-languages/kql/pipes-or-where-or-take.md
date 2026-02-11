# pipes | where | take

### pipes

Basic as it is, pipes are used in every query language.

Pipes take the output of the previous command and serve it as input in the next command.

#### Example

```
Table Name
| first command
| second command
```

The goal it to narrow our search to exactly what we need.

***

### where

The where command is used to select entry's from specific fields.

#### Example

Customers table:

<figure><img src="../../.gitbook/assets/Pasted image 20260211131803.png" alt=""><figcaption></figcaption></figure>

The table has 18000+ records.

If we want to search for customers that have the name 'Peter' and are from 'Europe' we would use the where command with the 'First name' and the 'ContinentName' fields.

```kql
Customers
| where FirstName == "Peter"
| where ContinentName == "Europe"
```

<figure><img src="../../.gitbook/assets/Pasted image 20260211131953.png" alt=""><figcaption></figcaption></figure>

***

### take

Nothing special of a command, just takes the amount of rows you specify.

#### Example

```
Customers
| take 10
```

This takes the first 10 rows of the 'Customers' table.

***

### Different Data Bases

If we use one database, we can specify all the tables that we want in that db.

But if we have a second db and we want to search a table from there? We can specify it like this:

```
cluster('cluster name').database('db name').table('table name')
| normal piped commands
```

#### Example

If we have a few queries for the 'ContosoSales' db, and want to search smth from the 'Samples' db as well, we can do it like this:

```
cluster('help').database('Samples').table('PopulationData')
| where State == "ARKANSAS"
```

Just specify the exact 'path' that leads to the table you want and thats it, the pipe commands below are syntax'd the same as always.

<figure><img src="../../.gitbook/assets/Pasted image 20260211132547.png" alt=""><figcaption></figcaption></figure>
