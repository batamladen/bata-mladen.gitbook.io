# Elastic Codes

## **Windows**\*

### event.code

* 1 - Process creation
* 13 - Registry modification (**Description:** Triggered when a registry key value is set (modified))
* 15 - FileCreateStreamHash (browser file download event)
* 11 - File create
* 3 - Network connection

#### 🔹 **Authentication & Logon Events**

* **4624** – Successful logon
* **4625** – Failed logon attempt
* **4634** – Logoff
* **4648** – Logon using explicit credentials
* **4672** – Special privileges assigned to a new logon (e.g., admin logins)
* **4776** – NTLM authentication attempt

#### 🔹 **Account & Privilege Changes**

* **4720** – A user account was created
* **4722** – A user account was enabled
* **4725** – A user account was disabled
* **4726** – A user account was deleted
* **4732** – A user was added to a privileged group
* **4733** – A user was removed from a privileged group

#### 🔹 **Process & System Monitoring**

* **4688** – A new process was created
* **4689** – A process was terminated
* **4657** – A registry value was modified
* **4697** – A service was installed on the system

#### 🔹 **Security & Threat Detection**

* **1102** – Security audit log cleared (potential sign of tampering)
* **4621** – Administrator recovered from a crash
* **4728** – A user was added to the Administrators group
* **4768** – A Kerberos authentication ticket was requested
* **4769** – A Kerberos service ticket was requested
* 4104 - Remote PowerShell script execution

#### 🔹 **File & Object Access**

* **4663** – Access to an object was requested (file/folder access)
* **4656** – A handle to an object was requested

### process

* process.command\_line: -
* process.parent.name: - app that runs the process
* process.parent.command\_line - command executed when started

***

## Zeek\*

* source.ip - ip adress
* dns.question.name - queried domain
