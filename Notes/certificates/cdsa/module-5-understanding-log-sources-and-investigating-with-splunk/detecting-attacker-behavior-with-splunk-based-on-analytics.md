# Detecting Attacker Behavior With Splunk Based On Analytics

In the previous lesson we saw how we can dettect attacker behaviuour based on TTP's.\
As mentioned the second method of the detection is relying a lot on statistical analysis and anomaly detection.

By profiling the **normal** behavior, we set a baseline of what is notmal, and what is not.

A good example of this approach in Splunk is the use of the `streamstats` command. This command allows us to perform real-time analytics on the data, which can be useful for identifying unusual patterns or trends.

**number of network connections initiated by a process within a certain time frame**

```spl
index="main" sourcetype="WinEventLog:Sysmon" EventCode=3 | bin _time span=1h | stats count as NetworkConnections by _time, Image | streamstats time_window=24h avg(NetworkConnections) as avg stdev(NetworkConnections) as stdev by Image | eval isOutlier=if(NetworkConnections > (avg + (0.5*stdev)), 1, 0) | search isOutlier=1
```

In this search:

* We start by focusing on network connection events (`EventCode=3`), and then group these events into hourly intervals (`bin` can be seen as a `bucket` alias). For each unique process image (`Image`), we calculate the number of network connection events per time bucket.
* We then use the `streamstats` command to calculate a rolling average and standard deviation of the number of network connections over a 24-hour period for each unique process image. This gives us a dynamic baseline to compare each data point to.
* The `eval` command is then used to create a new field, `isOutlier`, and assigns it a value of `1` for any event where the number of network connections is more than 0.5 standard deviations away from the average. This labels these events as statistically anomalous and potentially indicative of suspicious activity.
* Lastly, the `search` command filters our results to only include the outliers, i.e., the events where `isOutlier` equals `1`.

***

## Crafting SPL Searches Based On Analytics

### **Example 1: Detection Of Abnormally Long Commands**

```shell-session
index="main" sourcetype="WinEventLog:Sysmon" Image=*cmd.exe | eval len=len(CommandLine) | table User, len, CommandLine | sort - len
```

After reviewing the results, if we notice some benign activity that can be filtered out to reduce noise. We can apply the following modifications to the search.

```shell-session
index="main" sourcetype="WinEventLog:Sysmon" Image=*cmd.exe ParentImage!="*msiexec.exe" ParentImage!="*explorer.exe" | eval len=len(CommandLine) | table User, len, CommandLine | sort - len
```

***

### **Example 2: Detection Of Abnormal cmd.exe Activity**

The following search identifies unusual `cmd.exe` activity within a certain time range. It uses the `bucket` command to group events by hour, calculates the `count`, `average`, and `standard deviation` of `cmd.exe` executions, and flags outliers.

```shell-session
index="main" EventCode=1 (CommandLine="*cmd.exe*") | bucket _time span=1h | stats count as cmdCount by _time User CommandLine | eventstats avg(cmdCount) as avg stdev(cmdCount) as stdev | eval isOutlier=if(cmdCount > avg+1.5*stdev, 1, 0) | search isOutlier=1
```

***

### **Example 3: Detection Of Processes Loading A High Number Of DLLs In A Specific Time**

```shell-session
index="main" EventCode=7 | bucket _time span=1h | stats dc(ImageLoaded) as unique_dlls_loaded by _time, Image | where unique_dlls_loaded > 3 | stats count by Image, unique_dlls_loaded
```

After reviewing the results, if we notice some benign activity that can be filtered out to reduce noise. We can apply the following modifications to the search.

```shell-session
index="main" EventCode=7 NOT (Image="C:\\Windows\\System32*") NOT (Image="C:\\Program Files (x86)*") NOT (Image="C:\\Program Files*") NOT (Image="C:\\ProgramData*") NOT (Image="C:\\Users\\waldo\\AppData*")| bucket _time span=1h | stats dc(ImageLoaded) as unique_dlls_loaded by _time, Image | where unique_dlls_loaded > 3 | stats count by Image, unique_dlls_loaded | sort - unique_dlls_loaded
```

* `index="main" EventCode=7 NOT (Image="C:\\Windows\\System32*") NOT (Image="C:\\Program Files (x86)*") NOT (Image="C:\\Program Files*") NOT (Image="C:\\ProgramData*") NOT (Image="C:\\Users\\waldo\\AppData*")`: This part of the query is responsible for fetching all the events from the `main` index where `EventCode` is `7` (Image loaded events in Sysmon logs). The `NOT` filters are excluding events from known benign paths (like "Windows\System32", "Program Files", "ProgramData", and a specific user's "AppData" directory).
* `| bucket _time span=1h`: This command is used to group the events into time buckets of one hour duration. This is used to analyze the data in hourly intervals.
* `| stats dc(ImageLoaded) as unique_dlls_loaded by _time, Image`: The `stats` command is used to perform statistical operations on the events. Here, `dc(ImageLoaded)` calculates the distinct count of DLLs loaded (`ImageLoaded`) for each process image (`Image`) in each one-hour time bucket.
* `| where unique_dlls_loaded > 3`: This filter excludes the results where the number of unique DLLs loaded by a process within an hour is `3 or less`. This is based on the assumption that legitimate software usually loads DLLs at a moderate rate, whereas malware might rapidly load many different DLLs.
* `| stats count by Image, unique_dlls_loaded`: This command calculates the number of times each process (`Image`) has loaded `more than 3 unique DLLs` in an hour.
* `| sort - unique_dlls_loaded`: Finally, this command sorts the results in descending order based on the number of unique DLLs loaded (`unique_dlls_loaded`).

***

### **Example 4: Detection Of Transactions Where The Same Process Has Been Created More Than Once On The Same Computer**

We want to correlate events where the same process (`Image`) is executed on the same computer (`ComputerName`) since this might indicate abnormalities depending on the nature of the processes involved. As always, context and additional investigation would be necessary to confirm if it's truly malicious or just a benign occurrence.

```shell-session
index="main" sourcetype="WinEventLog:Sysmon" EventCode=1 | transaction ComputerName, Image | where mvcount(ProcessGuid) > 1 | stats count by Image, ParentImage
```

* `index="main" sourcetype="WinEventLog:Sysmon" EventCode=1`: This part of the query fetches all the Sysmon process creation events (`EventCode=1`) from the `main` index. Sysmon event code 1 represents a process creation event, which includes details such as the process that was started, its command line arguments, the user that started it, and the process that it was started from.
* `| transaction ComputerName, Image`: The transaction command is used to group related events together based on shared field values. In this case, events are being grouped together if they share the same `ComputerName` and `Image` values. This can help to link together all the process creation events associated with a specific program on a specific computer.
* `| where mvcount(ProcessGuid) > 1`: This command filters the results to only include transactions where more than one unique process GUID (`ProcessGuid`) is associated with the same program image (`Image`) on the same computer (`ComputerName`). This would typically represent instances where the same program was started more than once.
* `| stats count by Image, ParentImage`: Finally, this stats command is used to count the number of such instances by the program image (`Image`) and its parent process image (`ParentImage`).

<figure><img src="../../../.gitbook/assets/transactions detection.png" alt=""><figcaption></figcaption></figure>



Let's dive deeper into the relationship between `rundll32.exe` and `svchost.exe` (since this pair has the highest `count` number).

```shell-session
index="main" sourcetype="WinEventLog:Sysmon" EventCode=1  | transaction ComputerName, Image  | where mvcount(ProcessGuid) > 1 | search Image="C:\\Windows\\System32\\rundll32.exe" ParentImage="C:\\Windows\\System32\\svchost.exe" | table CommandLine, ParentCommandLine
```

> \[!NOTE] Note\
> By establishing a profile of "normal" behavior and utilizing a statistical model to identify deviations from a baseline, we could have detected the compromise of our environment more rapidly, especially with a thorough understanding of attacker tactics, techniques, and procedures (TTPs). However, it is important to acknowledge that relying solely on this approach when crafting queries is inadequate.

***

Explanation for Standard Deviation

If you eat 1 apple on monday, 2 on thursday, 1 on wensday, 3 on saturday, 2 on friday, and 2 in the weekend, you eat an **average** of 2 apples a day.

(4+2+1+3+2+2) / 7 = 14/7 = 2

Now if you eat another week 16 apples and the week after that 14, that is plus or minus 2 apples from the average, so **the standard deviation for the apples you eat per week would be 2**\
So if you eat 15, that is normal, if you eat 13, that is also normal.

But if you eat 20 apples per week, that is above the average, but that is ok, the porblem is that it is also above the standard deviation that is 2 (+ or - 2 apples a week).\
So that would be counted as an **anomaly**

This is a simple example that can be represented in a real world scenario.

If an average thread that a process creates is 5, and the standard deviation is 3 (everythin between 2 and 8 threads per process is normal), but we see in our SIEM that some proces created 20 threads, that would be indicated as an anomaly and a potential malware activity that needs to be checked.

Here is a graph that helped me understand this concept better

<figure><img src="../../../.gitbook/assets/excali apple.png" alt=""><figcaption></figcaption></figure>

Here we can see that we eated in the week 4 9 apples, witch is out of our standard deviation, we need to check what is this, what happened, were we brute forced to eat more???

now back to the task that we have left undone.

***

## Tasks

### Task 1

Navigate to http://\[Target IP]:8000, open the "Search & Reporting" application, and find through an analytics-driven SPL search against all data the source process images that are creating an unusually high number of threads in other processes. Enter the outlier process name as your answer where the number of injected threads is greater than two standard deviations above the average. Answer format: _answer_.exe

```
index="main" sourcetype="WinEventLog:Sysmon" EventCode=8  
| stats count by SourceImage  
| eventstats avg(count) as avg stdev(count) as stdev  
| eval isOutlier=if(count > (avg + (2*stdev)), 1, 0)  
| search isOutlier=1  
| table SourceImage count avg stdev isOutlier
```

<figure><img src="../../../.gitbook/assets/task1 5.png" alt=""><figcaption></figcaption></figure>

**answer: randomfile.exe**
