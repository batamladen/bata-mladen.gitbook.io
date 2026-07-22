# Data Types

#### getschema

You can use this commmand piped at the end of any table to see the table schema.

```kql
VMComputer
| getschema
```



#### List of KQL datatypes

* Bool
* Datetime
* Dynamec
* GUID
* Int
* Long
* Real
* String
* Timespan
* Decimal

***

#### Bool

Field that has either Yes No or null value.





#### Datetime

Represents an instance of time. It represents data + time but can display only date or only time as well. UTC is default timezone.



#### Decimal

128bit lenght decimal number.

Slower procesing time for this datatype than regular number.



#### Dynamic

dynamic in KQL is a flexible data type that can store **JSON-like data** instead of a single value.



```kql
let x = dynamic({
    "Name": "Mladen",
    "Age": 25,
    "Skills": ["KQL", "Sentinel"]
});
```

You can than access the values liek this:

```
x.Name
x.Skills[0]
```



Dynamic datatype is heavy, and does not work with some operators.



#### GUID

Not common, global unique identifier.

Ussualy a unique string datatype.



#### Int

Integeer

32bits and represent a whole number. No decimals





#### Long

64bits in size. not used that much only in special situation.



#### Real

64bits in size, but allow for a decimal point and more ised than a decimal type.



#### String

Mostu used datatype. Sequence of unicode chars.

Is btw brackets. Can contain kletters, numbers and symbols



Special chars ( / )

if you want to notate someone, you can use a slash before the quotes so that the quotes print as well.

```kql
print("John said /'KQL is cool/'!")
```





#### Timespan

Messures the lenght of time btw two intervals.

Can be messured in days, hours, muinutes, second, miliseconds...



Example:

```
SigninLogs
| project TimeGenerated, CreatedDateTime
| extend Difference = TimeGenerated - CreatedDateTime
| getschema
```

We can see that the difference column got a "timespan" datatype.

<figure><img src="../../.gitbook/assets/image (41).png" alt=""><figcaption></figcaption></figure>



***

### Null



Sometimes a field is blank, if its a string datatype, than its just empty, if it anything else its 'Null'

```
SiginLogs
| where TimeGenerated ≥ ago(3d)
| where isnull(MfaDetail)
| project-reorder Timegenerated, MfaDetails
```

<figure><img src="../../.gitbook/assets/image (42).png" alt=""><figcaption></figcaption></figure>



```
SigninLogs
| where TimeGenerated >= ago(3d)
| extend IsEmpty = isempty(ResultDescription)
| project TimeGenerated, IsEmpty, ResultDescription
| take 100
```

<figure><img src="../../.gitbook/assets/image (43).png" alt=""><figcaption></figcaption></figure>



***

### Casting

A term used when changing a datatype from one to another



Below we have 2 integers, we will chnage them to string datatypes.



```
VMComputer
| project Field1=Cpus, Field2=CpuSpeed
| extend AdditionOffFields1and2 = Field1 + Field2

```

<figure><img src="../../.gitbook/assets/image (44).png" alt=""><figcaption></figcaption></figure>



```
VMComputer
| project tostring (Field1=Cpus), (Field2=CpuSpeed)
| extend AdditionOffFields1and2 = Field1 + Field2
```



Since we changed datatypes now we can not perform math operations on the strings.



<figure><img src="../../.gitbook/assets/image (45).png" alt=""><figcaption></figcaption></figure>





