# contains | has | startswith | endswith

### contains

contains command is case-insensitive, its similar to the '==' sign but the difference is that the '==' sign expect exact mathces while contains returns parts as well.



#### example

```kql
cluster('help').database('SecurityLogs').InboundBrowsing
| where user_agent contains 'firefox'
```

this query will return rows with the example below:

'firefox'

'abcfirefox'

'firefox/1'

'abcfirefox/1'



while == would return only fields with exact match.



***

### has

has command is also case-insensitive, it is similar to contains, but does not return delimeters like contains does.

Delimeter examples: / . \[ { ( , < space



Strings in KQL are tokenized, meaning if you use fire,fox this is read by kql as

```kql
fire
fox
```

so == would not work here if we dont enter the exact string.



#### example

```kql
cluster('help').database('SecurityLogs').InboundBrowsing
| where user_agent has 'firefox'
```

this query will return rows with the example below:

'firefox' :white\_check\_mark:

'abcfirefox' :x:

'firefox/1' :white\_check\_mark:

'abcfirefox/1' :x:



Why has does not return abcfirefox/1 but return firefox/1?

beacuse kql reads the words like this because of the delimeters:

```kql
// abcfirefox/1 is read:
abcfirefox
1

//firefox/1 is read:
firefox
1
```



So has will return the rows that contain firefox/1 but not abcfirefox/1.



***

### startswith & endswith

this commands are case-insensitive (not case-sensitive) and are the same thing as if you use asteriks (\*) in your searches.



#### example

```kql
cluster('help').database('SecurityLogs').InboundBrowsing
| where user_agent startswith 'fire'
```

this will return all the rows that have the user\_agent field string starting whit "fire"



```kql
cluster('help').database('SecurityLogs').InboundBrowsing
| where user_agent endswith 'fox'
```

this will return all the rows that have the user\_agent field string ending whit "fox"



***

### negatives

all of the comands above can be used with the ! symbol to search for queries that DO NOT contain what we search for.



***

### case-sensitive

To make the contains and has commands case sensitive just add '\_cs' to the commands syntax.

This method is faster since it uses less resources.



```kql
cluster('help').database('SecurityLogs').InboundBrowsing
| where user_agent has_cs 'Firefox'

cluster('help').database('SecurityLogs').InboundBrowsing
| where user_agent contains_cs 'Firefox'
```



***

### practice

Lets write a query that will return all the websites that use https from the table InboundBrowsing from the 'url' field.



It does not make sense to use == here since the url field never contains only http.

<figure><img src="../../.gitbook/assets/image (37).png" alt=""><figcaption></figcaption></figure>



It also makes no sense in using contains since it includes everything after http, this it will return all the https websites as well.

<figure><img src="../../.gitbook/assets/image (38).png" alt=""><figcaption></figcaption></figure>

\
That's why we will use has command, since it searches for the exact string we enter no matter if there are other strings, and it wont return fields containing https.

<figure><img src="../../.gitbook/assets/image (39).png" alt=""><figcaption></figcaption></figure>



***

#### do it yourself

from the SecurityLogs database, from the InboundBrowsing table, find all the iphone users.

