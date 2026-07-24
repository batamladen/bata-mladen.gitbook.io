# summarize | bin |dcount |avg | countif

### bin

**`bin()`** groups values into buckets (bins) of a specified size.

When using bin we should specify 2 params, the column that we want to make buckets of and the timeframe.

```
DeviceEvents
| summarize count() by bin(Timestamp,1h)
```

<figure><img src="../../.gitbook/assets/image (48).png" alt=""><figcaption></figcaption></figure>



another example:

Guest sign-ins in the past month separated by day

```
SigninLogs
| where Timestamp >= ago(30d)
| where UserType == 'Guest'
| summarize count() by bin(TimeGenerated, 1h)
| sort by TimeGenerated asc
```



***

### avg()

**`avg()`** = calculates the **average (mean)** value of a column.

| Response Time |
| ------------- |
| 100           |
| 200           |
| 300           |

```
| summarize avg(ResponseTime)
```

Result: 200



Example:

```kql
VMConnection
| project TimeGenerated, BytesSent
| summarize count(), min(BytesSent), max(BytesSent) by bin(TimeGenerated, 1h)
| sort by TimeGenerated asc
```

<figure><img src="../../.gitbook/assets/image (49).png" alt=""><figcaption></figcaption></figure>



if we now add the avg, it will add another field that will display the average maxbytes sent per count.

```
VMConnection
| project TimeGenerated, BytesSent
| summarize count(), min(BytesSent), max(BytesSent), avg(BytesSent) by bin(TimeGenerated, 1h)
| sort by TimeGenerated asc
```



***

### countif

**`countif()`** = counts only the rows where a condition is true.



| User    | Result  |
| ------- | ------- |
| Alice   | Failed  |
| Bob     | Failed  |
| Charles | Success |
| David   | Success |

```
| summarize countif(Result == "Failed")
```

Returns: `2`



Example:

Find the total number of bytes in each hourly bucket, also count records that have value more than 0.

```
VMConnection
| project TimeGenerated, BytesSent
| summarize RecordsPerHour=count(), RecordsGreatherThanZero=countif(BytesSent > 0), min(BytesSent), max(BytesSent), TotalBytesSent=sum(BytesSent), avg(BytesSent) by bin(TimeGenerated, 1h)
| sort by TimeGenerated asc
```

<figure><img src="../../.gitbook/assets/image (50).png" alt=""><figcaption></figcaption></figure>



