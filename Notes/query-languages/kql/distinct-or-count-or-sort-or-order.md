# distinct | count | sort | order

### sort by

sort command returns our query in order based on the field of interest that we specify.\
We can specify if we want and asc or desc order.

The order is defined by the type of value contained in the field, but if there are numbers and letter if we sort in ascending order we will get results starting from '0' and going to letter 'z'

FunFact: capital letters are odered before small letters in ascending order.



***

### order by

This is the same as sort by just different syntax for people comming from SQL.



***

### distinct

Helps us find unique values in any field.



```
cluster('help').database('SecurityLogs').ProcessEvents
| where parent_process_name == 'excel.exe'
| distinct parent_process_name
```

result:

```
CMD.exe
cmd.exe
sc.exe
explorer.exe
conhost.exe
```

***

### count

Returns the number of rows as a result.

```
cluster('help').database('SecurityLogs').ProcessEvents
| where parent_process_name == 'excel.exe'
| distinct parent_process_name
| count
```

result:

```
5
```



***

### Homework

from the help cluster, the SecurityLog database, Employees table, sort the table by role name, from A-Z
