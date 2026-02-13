# Syntax and Query Structure

### Some important rules:

1. You can put as many spaces you want btw the pipe and the command

```kql
cluster('help').database('SecurityLogs').Email
|     where subject == "jasade"
```

2. You can put many pipe commands in one line

```kql
cluster('help').database('SecurityLogs').Email
| where subject == "jasade" | sort by event_time asc
```

3. Double slash to comment '//'

```kql
//this is a comment
cluster('help').database('SecurityLogs').Email
| where subject == "jasade" | sort by event_time asc
```

4. KQL is mostly case-sensitive
5. No quotes around field names



### Comparison operators

* `==` equals to



* `!=` not equal to

```
Table
| where FIrstName != "peter" //all people not named peter are returned
```



* \=\~ not case sensitive

```kql
Table
| where FIrstName =~ "peter" //all uppercase, lowercase, combined is returned
```



### Query order

How you filter your data matters!

If you filter first for the random 10 rows with 'take 10' and after that you search for all the users named 'peter' you wont get the same result as if you first searched for the name peter and than took random 10 rows.
