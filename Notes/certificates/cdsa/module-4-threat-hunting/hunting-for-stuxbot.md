# Hunting for Stuxbot

**tasks:**

`Hunt 1`: Create a KQL query to hunt for ["Lateral Tool Transfer"](https://attack.mitre.org/techniques/T1570/) to `C:\Users\Public`. Enter the content of the `user.name` field in the document that is related to a transferred tool that starts with "r" as your answer.

`Hunt 2`: Create a KQL query to hunt for ["Boot or Logon Autostart Execution: Registry Run Keys / Startup Folder"](https://attack.mitre.org/techniques/T1547/001/). Enter the content of the `registry.value` field in the document that is related to the first registry-based persistence action as your answer.

`Hunt 3`: Create a KQL query to hunt for ["PowerShell Remoting for Lateral Movement"](https://www.ired.team/offensive-security/lateral-movement/t1028-winrm-for-lateral-movement). Enter the content of the `winlog.user.name` field in the document that is related to PowerShell remoting-based lateral movement towards DC1.

***

### **hunt 1**

`event.code : 11 and "C:\Users\Public*"`

Event code 11 is for file creation, and the path.\
So it searches for created files in C:\Users\Public

Add file.name : "r\*" if you want, but there are 7 logs, you can manually search for a file name that starts with "r". and enter the user.name as answer.

**Answer: svc-sql1**

***

### **hunt 2**

event code 13 is for registry modification and we can specify a registry path that is asocciated with autostart execution when booted or logged on, in the above MITTRE link are mentioned default run keys path folders, we will just type /\* Run\* in registry path since it searches for the few that are registry related paths.

`event.code:13 AND registry.path: *Run*`

**Answer: LgvHsviAUVTsIN**

***

### **hunt 3**

Hint: laverage event code 4104 and powershell.file.script\_block\_text

Event code 4104 is for Remote PowerShell script execution

powershell.file.script\_block\_text field captures the content of executed PowerShell scripts.\
We have about 5 options to enter in this field and they are:

* `Enter-PSSession` (Interactive PowerShell Remoting)
* `Invoke-Command` (Execute commands on remote machines)
* `New-PSSession` (Establish a session)
* `-ComputerName` (Specifies a remote machine)
* `-Credential` (Possible privilege escalation attempt)
* `-EncodedCommand` (Obfuscation technique)

`event.code:4104 AND powershell.file.script_block_text: "Enter-PSSession"`

Search the documents that lateral move tovards the DC1

**Answer: svc-sql1**
