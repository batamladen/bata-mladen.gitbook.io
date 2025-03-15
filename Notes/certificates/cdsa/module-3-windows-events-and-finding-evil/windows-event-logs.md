# Windows Event Logs

## **Introduction to Windows Event Logs**

* Windows Event Logs store system logs, application logs, ETW provider logs, service logs, etc.
* Useful for cybersecurity professionals to analyze system behavior and detect intrusions.
* Logs are categorized into:
  * **Application** (app-related events)
  * **System** (OS-related events)
  * **Security** (authentication, access control, etc.)
  * **Setup** (system configuration logs)
  * **Forwarded Events** (logs from other systems)



***

## **Accessing Event Logs**

* Use **Event Viewer** (`eventvwr.msc`) to explore logs.
* Event logs can also be accessed programmatically via Windows Event Log API.
* Logs can be saved as **.evtx** files and reopened later.



***

## **Structure of an Event Log Entry**

Each event contains:

* **Log Name** – Type of log (Application, Security, System, etc.).
* **Source** – The program or component that generated the event.
* **Event ID** – Unique identifier for the event (useful for research).
* **Task Category** – Classifies the event functionally.
* **Level** – Severity (Information, Warning, Error, Critical).
* **Keywords** – Tags for filtering logs (e.g., "Audit Success").
* **User** – The account linked to the event.
* **OpCode** – Specifies the operation reported.
* **Logged** – Timestamp of the event.
* **Computer** – Machine where the event occurred.
* **XML Data** – Contains all details in XML format for advanced analysis.



***

## **Analyzing Windows Event Logs**

* **Error Events**: Provide diagnostic details about issues.
* **Security Logs**: Important for tracking authentication and access activities.
* **Example: Event ID 4624 (Successful Logon)**
  * Logs when a user successfully logs in.
  * Contains details like Logon ID, Logon Type, and account used.



***

## **Filtering & Searching Logs with XML Queries**

* Use **Filter Current Log** → **XML** → **Edit Query Manually**.
* Example: Searching for logs with `SubjectLogonId = "0x3E7"` to track system logon events.
* Microsoft provides detailed documentation on advanced XML filtering.



***

## **Tracking Security Events**

* **Event ID 4907**: Audit policy change (modification of security logs).
* **SACL (System Access Control List)**: Defines access logging rules for files and registry keys.
* **Example:** A process (`SetupHost.exe`) modifying boot manager security logs.
* **Event ID 4672**: Special logon event (user granted elevated privileges).



***

## **Important Windows Event IDs**

#### **System Logs:**

* **Event ID 1074** – System shutdown/restart.
* **Event ID 6005** – Event Log service started (indicates system startup).
* **Event ID 6006** – Event Log service stopped (indicates system shutdown).
* **Event ID 6013** – System uptime.
* **Event ID 7040** – Service startup type changed.

#### **Security Logs:**

* **Event ID 1102** – Audit log cleared (potential evidence tampering).
* **Event ID 1116** – Windows Defender detected malware.
* **Event ID 1118** – Antivirus remediation started.
* **Event ID 1119** – Antivirus remediation succeeded.
* **Event ID 1120** – Antivirus remediation failed.



***

## **Summary**

* Windows Event Logs provide crucial system and security insights.
* Event Viewer allows easy navigation and analysis.
* XML filtering helps refine log searches for investigation.
* Certain event IDs are particularly useful for cybersecurity and system administration.

***

## **Quiz**

#### **Multiple Choice Questions** (Choose the correct answer)

1. Which of the following is NOT a default Windows Event Log category?\
   a) Application\
   b) Security\
   c) Network\
   d) System
2. What is the primary tool used to view Windows Event Logs?\
   a) Task Manager\
   b) Event Viewer\
   c) Windows Defender\
   d) Registry Editor
3. What does **Event ID 4624** indicate?\
   a) System restart\
   b) Successful logon\
   c) Service failure\
   d) Firewall rule change
4. Which field in an event log specifies the program or component that generated the event?\
   a) Event ID\
   b) OpCode\
   c) Source\
   d) Log Name
5. What does Event ID 1102 indicate in security logs?\
   a) Successful user logon\
   b) Failed authentication attempt\
   c) Audit logs cleared\
   d) System shutdown

#### **True/False Questions**

1. **True or False:** The Event Log service must be running for Windows to store logs.
2. **True or False:** Event logs can only be accessed through the graphical Event Viewer.
3. **True or False:** XML filtering in Event Viewer allows you to refine searches for specific logs.

#### **Fill in the Blanks**

9. The event log file format used by Windows is **\_\_\_\_\_\_\_\_**.
10. The **\_\_\_\_\_\_\_\_** field in an event log entry defines the severity level (e.g., Information, Warning, Error).

#### **Short Answer Questions**

11. Name two ways Windows Event Logs can be accessed other than the Event Viewer.
12. What is the significance of the **SACL (System Access Control List)** in security logs?
13. If a security analyst sees Event ID **4672**, what should they investigate?

***

Answers:

1. c) Network
2. b) Event Viewer
3. b) Successful logon
4. c) Source
5. c) Audit logs cleared
6. True
7. False
8. True
9. .evtx
10. Level
11. PowerShell, `wevtutil`
12. Defines audit events for access attempts
13. Investigate privileged admin logon (possible unauthorized access)
