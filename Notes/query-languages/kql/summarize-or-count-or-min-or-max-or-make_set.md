# summarize | count() | min | max | make\_set

### Summarize

It takes a bunch of rows and **collapses them** into a summary, using aggregation functions (like `count()`, `sum()`, `avg()`, `max()`, etc.).

**Basic syntax:**

```
Table
| summarize <aggregation> by <grouping column(s)>

```

***

#### summarize by count()



summarize by count() prints all the UNIQUE values in the specified column just like `distinct` command does, BUT in addition to that it also hows how many times the unique value was mentined in the table.

```kql
DeviceEvents
| project DeviceName, ActionType
| summarize count() by DeviceName
```

<figure><img src="../../.gitbook/assets/image (47).png" alt=""><figcaption></figcaption></figure>



***

### make\_set

It collects **all the distinct values** of a column into a single **array** (a list), per group. Duplicates are removed automatically (that's the "set" part — like a math set, unique values only).



To best understand this here is a practical example.

We have a **NetworkLogs** table

| country | domain      |
| ------- | ----------- |
| US      | example.com |
| US      | example.com |
| US      | test.com    |
| DE      | foo.net     |
| DE      | foo.net     |
| DE      | example.com |
| BG      | example.com |

When we use only summarize count() by country we can see all the distinct rows of the country column and now many times each value is repeated.

```
NetworkLogs
| summarize count() by country
```

| country | count\_ |
| ------- | ------- |
| US      | 3       |
| DE      | 3       |
| BG      | 1       |



Now if we add the `make_set` function to the `count()` function we will get the values that are either unique or repeated with the distinct countries.

```
NetworkLogs
| summarize count(), make_set(domain) by country
```

| country | count\_ | set\_domain                  |
| ------- | ------- | ---------------------------- |
| US      | 3       | \["example.com", "test.com"] |
| DE      | 3       | \["foo.net", "example.com"]  |
| BG      | 1       | \["example.com"]             |



***

### min() & max()

* **`min(col)`** → returns the **smallest** value in each group
* **`max(col)`** → returns the **largest** value in each group

They work on **numbers, dates/times, and even text** (for text, they are put in an alphabetical order A->Z and lenght does not matter here).



Example:

Lets use the `SigninLogs` table.

| user  | TimeGenerated    | BytesSent |
| ----- | ---------------- | --------- |
| alice | 2026-07-23 08:00 | 500       |
| alice | 2026-07-23 09:30 | 1200      |
| alice | 2026-07-23 11:00 | 300       |
| bob   | 2026-07-23 07:15 | 800       |
| bob   | 2026-07-23 10:45 | 2000      |

Lets see the min and max bytes sent by each user:

```
SigninLogs
| summarize min(BytesSent), max(BytesSent) by user
```

| user  | min\_BytesSent | max\_BytesSent |
| ----- | -------------- | -------------- |
| alice | 300            | 1200           |
| bob   | 800            | 2000           |



We can use min and max for time values as well.

```
SigninLogs
| summarize FirstSeen = min(TimeGenerated), LastSeen = max(TimeGenerated) by user
```

| user  | FirstSeen        | LastSeen         |
| ----- | ---------------- | ---------------- |
| alice | 2026-07-23 08:00 | 2026-07-23 11:00 |
| bob   | 2026-07-23 07:15 | 2026-07-23 10:45 |



