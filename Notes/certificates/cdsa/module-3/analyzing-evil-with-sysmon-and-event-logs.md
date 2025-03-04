# Analyzing Evil With Sysmon & Event Logs

## Introduction

Understanding and analyzing malicious events is crucial for cybersecurity. This lesson builds upon benign event analysis and introduces techniques for detecting malicious activity using Sysmon and Windows Event Logs.

## Sysmon Basics

**Sysmon (System Monitor)** is a Windows system service and device driver that provides detailed logs of system activity. It enhances Windows Event Logging by capturing additional details, making it a powerful tool for threat detection and forensic analysis.

### Sysmon Components:

* **Windows Service**: Monitors system activity.
* **Device Driver**: Captures activity data.
* **Event Log**: Stores captured events.

### Common Sysmon Event IDs:

* **Event ID 1**: Process Creation
* **Event ID 3**: Network Connection
* **Event ID 7**: Image Load (useful for DLL hijacking detection)
* Event ID 10: Process Accessed (when one process opens another process with specific access rights)
* **Event ID 4688**: New process creation (Security Log)

Sysmon logs details that standard Security Event logs may miss, making it highly effective for security monitoring.

***

## Key Security Concepts

* **DLL (Dynamic Link Library)**: A file containing code and functions that multiple programs can use. Attackers exploit DLL hijacking by tricking an application into loading a malicious DLL instead of the legitimate one.
* **LSASS (Local Security Authority Subsystem Service)**: A Windows process responsible for enforcing security policies and managing user authentication. Attackers target LSASS to dump credentials using tools like Mimikatz.
* **PowerShell and C# Injection**: Attackers use scripts to inject malicious code into running processes to execute commands stealthily.

***

## Sysmon Configuration

Sysmon uses an XML-based configuration file to filter event logging. Recommended configuration files:

* [SwiftOnSecurity Sysmon Config](https://github.com/SwiftOnSecurity/sysmon-config) (used in this lesson)
* [Sysmon Modular Config](https://github.com/olafhartong/sysmon-modular)

### Installation:

Download Sysmon from [Microsoft's official site](https://docs.microsoft.com/en-us/sysinternals/downloads/sysmon) and install it with:

```
C:\Tools\Sysmon> sysmon.exe -i -accepteula -h md5,sha256,imphash -l -n
```

To use a custom Sysmon configuration:

```
C:\Tools\Sysmon> sysmon.exe -c filename.xml
```

***

## Detection Example 1: Detecting DLL Hijacking

DLL hijacking occurs when a malicious DLL is loaded by a legitimate application.

### Steps:

1. Configure Sysmon to log Event ID 7 (module load events) by modifying `sysmonconfig-export.xml`.
2. Change `include` to `exclude` to capture all relevant DLL load events.
3.  Reload Sysmon with the modified configuration:

    ```
    C:\Tools\Sysmon> sysmon.exe -c sysmonconfig-export.xml
    ```
4. Open Event Viewer and navigate to `Applications and Services -> Microsoft -> Windows -> Sysmon`.
5. Filter logs for Event ID 7 and search for suspicious DLL loads.
6. Detect anomalies such as:
   * **calc.exe** running from a writable directory.
   * **WININET.dll** loaded outside `System32`.
   * Unsigned DLLs replacing Microsoft-signed ones.

These indicators help identify DLL hijacking attempts effectively.

***

## Detection Example 2: Detecting Unmanaged PowerShell/C# Injection

C# is a managed language requiring the Common Language Runtime (CLR). Anomalous use of CLR-related DLLs can indicate malicious activity.

### Steps:

1. Use **Process Hacker** to inspect processes.
2. Look for suspicious processes loading `clr.dll` or `clrjit.dll` (indicating C# execution in an unusual process).
3.  Inject a PowerShell-like DLL into `spoolsv.exe` using `Invoke-PSInject.ps1`:

    ```
    powershell -ep bypass
    Import-Module .\Invoke-PSInject.ps1
    Invoke-PSInject -ProcId [Process ID of spoolsv.exe] -PoshCode "V3JpdGUtSG9zdCAiSGVsbG8sIEd1cnU5OSEi"
    ```
4. Verify that `spoolsv.exe` transitions from unmanaged to managed.
5. Use Sysmon Event ID 7 to confirm suspicious module loads.

***

## Detection Example 3: Detecting Credential Dumping

Credential dumping tools like **Mimikatz** attempt to extract Windows credentials from `LSASS`.

### Steps:

1. Monitor suspicious access to `LSASS.exe` using Sysmon Event ID 10 (process access events).
2. Use Process Explorer or Sysmon to track `sekurlsa::logonpasswords` execution.
3. Block unauthorized access to LSASS with security policies.

## Conclusion

Sysmon and event logs provide essential telemetry for detecting cyber threats. Proper configuration and monitoring of key event IDs allow security analysts to identify and mitigate malicious activities efficiently.

***

## Quiz

**Multiple-Choice Questions**

1. **What is the primary purpose of Sysmon?**\
   a) To replace Windows Defender\
   b) To enhance Windows Event Logging with detailed system activity logs\
   c) To disable event logging for security reasons\
   d) To prevent users from executing scripts
2. **Which Sysmon Event ID is useful for detecting DLL hijacking?**\
   a) Event ID 1\
   b) Event ID 3\
   c) Event ID 7\
   d) Event ID 10
3. **What can Event ID 10 help detect?**\
   a) Process creation\
   b) Unauthorized access to `LSASS.exe`\
   c) Network connections\
   d) Image loads
4. **Which of the following event logs would be most useful for detecting PowerShell injection?**\
   a) Event ID 1\
   b) Event ID 7\
   c) Event ID 10\
   d) Event ID 4688
5. **Why is monitoring `clr.dll` and `clrjit.dll` important in threat detection?**\
   a) These DLLs indicate potential C# execution in unexpected processes\
   b) They are related to Windows Kernel activity\
   c) They are frequently used by ransomware to encrypt files\
   d) They are essential system files and should never be loaded

***

**True/False Questions**

1. **True or False?** Sysmon logs more detailed security events than the default Windows Security logs.
2. **True or False?** Sysmon can be configured using an XML-based configuration file to filter event logging.
3. **True or False?** Event ID 3 logs new process creation.
4. **True or False?** A modified Sysmon configuration file must be reloaded for changes to take effect.
5. **True or False?** The presence of unsigned DLLs loading in System32 is always a benign activity.

***

**Short Answer Questions**

1. **What command is used to install Sysmon with hash logging enabled?**
2. **How can you filter Sysmon logs to focus only on Event ID 7?**
3. **What is one key indicator of DLL hijacking in event logs?**
4. **How can attackers use `Invoke-PSInject.ps1` to execute malicious PowerShell code?**
5. **What is a simple way to detect credential dumping attempts against LSASS?**

Answers:

#### Quiz Questions

1. B
2. C
3. D
4. C
5. A
6. True
7. True
8. False
9. True
10. False
11. `sysmon.exe -i -accepteula -h md5,sha256,imphash -l -n`
12. Use Event Viewer filters or `Get-WinEvent` in PowerShell with `-FilterHashtable @{LogName='Microsoft-Windows-Sysmon/Operational'; ID=7}`.
13. Unexpected DLLs loading from non-standard directories.
14. It allows execution of PowerShell code within another process, bypassing security measures.
15. Monitoring Event ID 10 for unauthorized access to `LSASS.exe`.
