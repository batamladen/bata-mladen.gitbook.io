# Detecting Attacker Behavior With Splunk Based On TTPs

In crafting detection-related SPL (Search Processing Language) searches in Splunk, we utilize two main approaches:

* We know some bad guys' tricks, so we watch for things that look like those tricks. It's like spotting a villain because they wear a black hat—we recognize them from before. ( known adversary TTPs)
* We look for anything that seems weird or different from normal behavior. It's like noticing someone acting strangely in a crowd, even if we don’t know exactly why. We use math to find these odd things. (anomalies)

***

## Crafting SPL Searches Based On Known TTPs

As mentioned above, the first approach revolves around a comprehensive understanding of known attacker behavior and TTPs.

### Example: Detection Of Reconnaissance Activities Leveraging Native Windows Binaries

Attackers often leverage native Windows binaries (such as `net.exe`) to gain insights into the target environment, identify potential privilege escalation opportunities, and perform lateral movement. `Sysmon Event ID 1` can assist in identifying such behavior.

```shell
index="main" sourcetype="WinEventLog:Sysmon" EventCode=1 Image=*\\ipconfig.exe OR Image=*\\net.exe OR Image=*\\whoami.exe OR Image=*\\netstat.exe OR Image=*\\nbtstat.exe OR Image=*\\hostname.exe OR Image=*\\tasklist.exe | stats count by Image,CommandLine | sort - count
```

***

### Example: Detection Of Requesting Malicious Payloads/Tools Hosted On Reputable/Whitelisted Domains (Such As githubusercontent.com)

Attackers frequently exploit the use of `githubusercontent.com` as a hosting platform for their payloads. This is due to the common whitelisting and permissibility of the domain by company proxies. `Sysmon Event ID 22` can assist in identifying such behavior.

```shell
index="main" sourcetype="WinEventLog:Sysmon" EventCode=22  QueryName="*github*" | stats count by Image, QueryName
```

***

### Example: Detection Of PsExec Usage

**PsExec** is a command-line tool that lets you run programs or commands on a remote Windows computer—kind of like remote control for IT admins.

It’s like telling another computer to do something from far away (e.g., restart, install software, or run a script).

**Good guys** (like IT teams) use it to manage computers in a network.**Bad guys** (hackers) sometimes misuse it to spread malware or steal data.

`Sysmon Event ID 13`, `Sysmon Event ID 11`, and `Sysmon Event ID 17` or `Sysmon Event ID 18` can assist in identifying usage of PsExec.

#### Case 1:Leveraging Sysmon Event ID 13

this query is looking for instances where the `services.exe` process has modified the `ImagePath` value of any service. The output will include the details of these modifications, including the name of the modified service, the new ImagePath value, and the time of the modification.

```shell-session
index="main" sourcetype="WinEventLog:Sysmon" EventCode=13 Image="C:\\Windows\\system32\\services.exe" TargetObject="HKLM\\System\\CurrentControlSet\\Services\\*\\ImagePath" | rex field=Details "(?<reg_file_name>[^\\\]+)$" | eval reg_file_name = lower(reg_file_name), file_name = if(isnull(file_name),reg_file_name,lower(file_name)) | stats values(Image) AS Image, values(Details) AS RegistryDetails, values(_time) AS EventTimes, count by file_name, ComputerName
```

#### Case 2: Leveraging Sysmon Event ID 11

```shell-session
index="main" sourcetype="WinEventLog:Sysmon" EventCode=11 Image=System | stats count by TargetFilename
```

#### Case 3: Leveraging Sysmon Event ID 18

```shell-session
index="main" sourcetype="WinEventLog:Sysmon" EventCode=18 Image=System | stats count by PipeName
```

***

### Example: Detection Of Utilizing Archive Files For Transferring Tools Or Data Exfiltration

Attackers may employ `zip`, `rar`, or `7z` files for transferring tools to a compromised host or exfiltrating data from it.

```shell-session
index="main" EventCode=11 (TargetFilename="*.zip" OR TargetFilename="*.rar" OR TargetFilename="*.7z") | stats count by ComputerName, User, TargetFilename | sort - count
```

***

### Example: Detection Of Utilizing PowerShell or MS Edge For Downloading Payloads/Tools

Attackers may exploit PowerShell to download additional payloads and tools

```shell-session
index="main" sourcetype="WinEventLog:Sysmon" EventCode=11 Image="*powershell.exe*" |  stats count by Image, TargetFilename |  sort + count
```

```shell-session
index="main" sourcetype="WinEventLog:Sysmon" EventCode=11 Image="*msedge.exe" TargetFilename=*"Zone.Identifier" |  stats count by TargetFilename |  sort + count
```

first is for powershell and second for ms edge

The `*Zone.Identifier` is indicative of a file downloaded from the internet or another potentially untrustworthy source. Windows uses this zone identifier to track the security zones of a file. The `Zone.Identifier` is an ADS (Alternate Data Stream) that contains metadata about where the file was downloaded from and its security settings.

***

### Example: Detection Of Execution From Atypical Or Suspicious Locations

The following SPL search is designed to identify any process creation (`EventCode=1`) occurring in a user's `Downloads` folder.

```shell-session
index="main" EventCode=1 | regex Image="C:\\\\Users\\\\.*\\\\Downloads\\\\.*" |  stats count by Image
```

***

### Example: Detection Of Executables or DLLs Being Created Outside The Windows Directory

The following SPL identifies potential malware activity by checking for the creation of executable and DLL files outside the Windows directory.

```shell-session
index="main" EventCode=11 (TargetFilename="*.exe" OR TargetFilename="*.dll") TargetFilename!="*\\windows\\*" | stats count by User, TargetFilename | sort + count
```

***

### Example: Detection Of Misspelling Legitimate Binaries

Attackers often disguise their malicious binaries by intentionally misspelling legitimate ones to blend in and avoid detection. The purpose of the following SPL search is to detect potential misspellings of the legitimate `PSEXESVC.exe` binary, commonly used by `PsExec`. By examining the `Image`, `ParentImage`, `CommandLine` and `ParentCommandLine` fields, the search aims to identify instances where variations of `psexe` are used, potentially indicating the presence of malicious binaries attempting to masquerade as the legitimate PsExec service binary.

```shell-session
index="main" sourcetype="WinEventLog:Sysmon" EventCode=1 (CommandLine="*psexe*.exe" NOT (CommandLine="*PSEXESVC.exe" OR CommandLine="*PsExec64.exe")) OR (ParentCommandLine="*psexe*.exe" NOT (ParentCommandLine="*PSEXESVC.exe" OR ParentCommandLine="*PsExec64.exe")) OR (ParentImage="*psexe*.exe" NOT (ParentImage="*PSEXESVC.exe" OR ParentImage="*PsExec64.exe")) OR (Image="*psexe*.exe" NOT (Image="*PSEXESVC.exe" OR Image="*PsExec64.exe")) |  table Image, CommandLine, ParentImage, ParentCommandLine
```

***

### Example: Detection Of Using Non-standard Ports For Communications/Transfers

Attackers often utilize non-standard ports during their operations. The following SPL search detects suspicious network connections to non-standard ports by excluding standard web and file transfer ports (80, 443, 22, 21). The `stats` command aggregates these connections, and they are sorted in descending order by `count`.

```shell-session
index="main" EventCode=3 NOT (DestinationPort=80 OR DestinationPort=443 OR DestinationPort=22 OR DestinationPort=21) | stats count by SourceIp, DestinationIp, DestinationPort | sort - count
```
