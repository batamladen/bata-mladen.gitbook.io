# and | or | limit | top

### and

and is a boolean or logical operator.

instead of using a command 2 times, you can use `and` to simplify the query.

```kql
VMConnection
| where ProcessName =="python3"
| where SourceIp == "127.0.0.1"
```

:x:

```kql
VMConnection
| where ProcessName =="python3"
    and SourceIp == "127.0.0.1"
```

:white\_check\_mark:



***

### or

or is also a boolean or logical operator.

It is used when we need at least one condition correct.



```kql
VMConnection
| where RemoteLatitude >= 60 or ResponseTimeMax > 1000
```



***

### and or combination

We can use the operators in combination with each other using parenthasis to define the query nice.

```kql
WindowsFirewall
| where (DestinationPort == '80' and Protocol == "TCP") or DestinationIP startswith "20"
| project DestinationPort, Protocol, DestinationIP
```

Here we search for destination port 80 and protocol tcp rows to be returned, or rows where destination ip starts with '20'.

Any of the two conditions meet is returned.



***

### limit

As we have the sort by command and the order by command do the same thing,\
so do take and limit do the same thing.

limit command does the same thing as take, it takes the defined number and returnes random rows.



```kql
VMConnections
| take 10
| limit 10
// both do the same thing.
```



***

### top

while take gives random results.\
top gives a set number or records based on how the dataset is ordered.



```
SigninLogs
| top 10 by TimeGenerated desc
```

```
VMConnection
| where * has '389'
| top 10 by TimeGenerated desc
```
