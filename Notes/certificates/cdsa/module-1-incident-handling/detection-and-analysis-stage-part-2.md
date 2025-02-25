# Detection & Analysis Stage (Part 2)

## Overview

When an investigation is initiated, our goal is to determine what happened and how it occurred. To analyze incident-related data effectively, incident response team members require extensive technical knowledge and experience. A common question arises: "Why should we focus on how an incident occurred instead of just restoring the impacted systems and moving on?"

If we fail to identify the attack vector and the affected assets, our remediation efforts may not prevent the attacker from regaining access. However, if we determine precisely how the adversary infiltrated the system, what tools they employed, and which systems were compromised, we can design a remediation strategy that ensures the attack cannot be repeated.

***

## The Investigation

The investigation begins with limited initial information about the incident. From this starting point, we engage in a three-step iterative process:

1. **Creation and usage of Indicators of Compromise (IOCs)**
2. **Identification of new leads and impacted systems**
3. **Data collection and analysis from the new leads and impacted systems**

This cycle continues until a comprehensive understanding of the incident is achieved.

<figure><img src="../../../.gitbook/assets/image (35).png" alt=""><figcaption></figcaption></figure>

***

### **Initial Investigation Data**

A successful investigation relies on valid leads gathered throughout the entire process. The incident response team must continuously uncover new leads rather than fixating on a single piece of evidence, such as a known malicious tool. Over-focusing on a specific activity may result in incomplete findings and an inadequate assessment of the overall impact.

***

### **Creation & Usage of IOCs**

Indicators of Compromise (IOCs) are signs that an incident has occurred. IOCs are documented in a structured format and represent forensic artifacts of a security breach. Examples of IOCs include:

* Malicious IP addresses
* Hash values of malicious files
* Specific file names associated with known attacks

IOCs can be created and shared using standard languages such as **OpenIOC** and **Yara**. Free tools like **Mandiant's IOC Editor** can be used to create or modify IOCs. These IOCs can also be obtained from third-party sources when dealing with known adversaries or attack methods.

To utilize IOCs effectively, an **IOC-obtaining/searching tool** must be deployed (either built-in or third-party, potentially at scale). Common methods for handling IOCs in Windows environments include **WMI** and **PowerShell**. However, it is crucial to avoid exposing privileged credentials when investigating potentially compromised systems.

**Security Consideration:**

* Windows logons with **logon type 3 (Network Logon)** do not cache credentials on the remote system.
* Tools like **PsExec** behave differently depending on how they are used:
  * If PsExec is executed **with explicit credentials**, those credentials are cached.
  * If PsExec is executed **without credentials**, it relies on the existing session and does not cache credentials.

Understanding how tools operate is crucial to avoiding unintended exposure.

***

### **Identification of New Leads & Impacted Systems**

After executing IOC searches, relevant hits should be analyzed to determine additional systems exhibiting signs of compromise. However, these hits may not always be directly associated with the incident being investigated. Some IOCs might be too broad, leading to false positives.

Key considerations:

* **Eliminate false positives** to avoid distractions.
* **Prioritize hits** that provide the most valuable forensic insights.
* **Identify artifacts** that can generate new investigative leads.

***

### **Data Collection & Analysis from New Leads & Impacted Systems**

Once compromised systems are identified, their state should be preserved for further analysis. Depending on the scenario, data collection can follow one of two approaches:

1. **Live Response** - Gathering real-time forensic data while the system is running.
2. **Post-Shutdown Analysis** - Turning off the system before analysis (rarely recommended due to potential data loss in volatile memory).

**Live response** is preferred since shutting down a system may result in the loss of valuable forensic artifacts stored in **RAM memory**. Regardless of the method chosen, it is critical to **minimize system interaction** to prevent accidental modification of evidence.

**Types of Forensic Analysis:**

* **Malware analysis** - Identifying and understanding malicious code.
* **Disk forensics** - Examining system files and storage for signs of compromise.
* **Memory forensics** - Analyzing RAM to uncover active threats (increasingly relevant for advanced attacks).

#### **Maintaining Chain of Custody**

During the forensic process, all collected evidence must be properly documented to maintain the **chain of custody**. This ensures that the data remains court-admissible should legal action be pursued against the attacker.

***

## Quiz

### Multiple Choice:

1. What is the primary reason for investigating an incident instead of just rebuilding the system?
   * a) To prevent future attacks by understanding the attack path
   * b) To create a report for internal documentation
   * c) To notify all employees about the incident
   * d) To remove all IOCs from the system
2. Which of the following is NOT an example of an Indicator of Compromise (IOC)?
   * a) IP addresses
   * b) Hash values of files
   * c) File names
   * d) CPU temperature
3. What is the main purpose of using Yara rules?
   * a) To identify IOCs in a structured manner
   * b) To automatically patch vulnerabilities
   * c) To create a firewall rule set
   * d) To configure system backups

### True/False Questions:

1. (True/False) Live response is the least common approach when collecting forensic data from a compromised system.
2. (True/False) PsExec always caches credentials on a remote system, regardless of how it is used.

### Short Answer Questions:

3. Why is it important to keep track of the chain of custody during an investigation?
4. What is one key reason why shutting down a compromised system may not always be the best approach?
5. How can IOCs help in identifying new leads during an investigation?
