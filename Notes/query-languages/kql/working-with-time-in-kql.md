# Working with time in KQL

You want to define the time at the beginning at the query since it will drasticaly cut off a lot of logs.

It is used with the `where` command and the `TimeGenerated` field and a time command (ago, between, now...)



### ago

if used with > the ago will return logs NEWER than time defined.

if used with < the ago will return logs OLDER than time defined.



#### syntax

```
where TimeGenerated [operator] ago([number][d=days, m=minutes, s=seconds])
```



Example:

```
FileCreationEvents
| where TimeGenerated > ago(1d)
```

result: empty



example:

```
FileCreationEvents
| where TimeGenerated < ago(1d)
```

result:

|                            |              |               |
| -------------------------- | ------------ | ------------- |
| 2022-01-03 20:08:12.451382 | B5HS-LAPTOP  | 15.jpg        |
| 2022-01-03 20:09:25.180969 | AXFE-DESKTOP | resources.pri |
| etc                        | etc          | etc           |



***

### between

the kql accept a few options to define the date time.



#### syntax:

```
where TimeGenerated [operator] between (datetime(date) .. datetime(date))
```



#### Example

```
SignInLOgs
| where TimeGenerated between (datetime(2025-7-5T02:00:00) .. datetime(2025-7-6T15:00:00))
```



You can define only date no time

```
SignInLOgs
| where TimeGenerated between (datetime(2025-7-5) .. datetime(2025-7-6))
```



***

### now

now function means... now like... current time.



#### Syntax

```
where TimeGenerated between (datetime() .. now())
```



#### Example

```
SignInLOgs
| where TimeGenerated between (datetime(2025-7-5) .. now())
```

```
SignInLOgs
| where TimeGenerated between (datetime(2025-7-5) .. now(-4))
```
