# Detection & Analysis Stage (Part 1)

## **Overview**

* This stage focuses on detecting security incidents using various tools, sensors, logs, and trained personnel.
* Includes sharing information and utilizing context-based threat intelligence.
* Network segmentation and visibility are critical.

### **Sources of Threat Detection**

Threats can be detected through:

1. Employees noticing abnormal behavior.
2. Alerts from security tools (EDR, IDS, Firewall, SIEM, etc.).
3. Threat hunting activities.
4. Third-party notifications about a compromise.

### **Levels of Detection**

To enhance detection, organizations categorize their networks into different levels:

1. **Network Perimeter:** Firewalls, network intrusion detection/prevention systems, DMZ.
2. **Internal Network:** Local firewalls, host intrusion detection/prevention systems.
3. **Endpoint Level:** Antivirus systems, endpoint detection & response systems.
4. **Application Level:** Application logs, service logs.

***

## **Initial Investigation**

When a security incident is detected:

* Conduct an initial investigation before calling an organization-wide incident response.
* Gather key information:
  * **Date/Time** of the incident.
  * **Who detected it** and **how** it was detected.
  * **Type of incident** (e.g., phishing, system unavailability).
  * **Impacted systems and actions taken.**
  * **Physical location, OS, IP addresses, hostnames, and system state.**
  * **If malware is involved:** List of affected systems, type of malware, forensic data (hashes, files, etc.).
* Understanding the impacted systems helps in making appropriate response decisions.

### **Building an Incident Timeline**

* A chronological event log that organizes information for analysis.
* Example format:

| Date       | Time      | Hostname    | Event Description               | Data Source        |
| ---------- | --------- | ----------- | ------------------------------- | ------------------ |
| 09/09/2021 | 13:31 CET | SQLServer01 | Hacker tool 'Mimikatz' detected | Antivirus Software |

* Helps identify attacker behavior, network connections, downloads, and activity origins.

***

## **Incident Severity & Extent**

Key questions to determine severity:

1. What is the exploitation impact?
2. What are the exploitation requirements?
3. Are business-critical systems affected?
4. Suggested remediation steps?
5. How many systems are impacted?
6. Is the exploit actively being used in the wild?
7. Does the exploit have worm-like capabilities?

* High-impact incidents require immediate escalation.

***

## **Incident Confidentiality & Communication**

* Incident details should remain confidential unless disclosure is legally required.
* Communication should be handled by authorized personnel in coordination with legal teams.
* Initial expectations and goals should be defined:
  * Type of incident.
  * Available evidence sources.
  * Time estimation for investigation.
  * Probability of identifying the adversary.
* Keep stakeholders and management updated as the investigation progresses.

***

## **Quiz: Detection & Analysis Stage (Part 1)**

1. **Which of the following is NOT a source of threat detection?**
   * A) Firewall alerts
   * B) Threat hunting activities
   * C) A customer complaint about service delays
   * D) Employee reporting suspicious activity
2. **What is the purpose of network segmentation in threat detection?**
   * A) It improves network speed.
   * B) It isolates sensitive data and increases visibility.
   * C) It eliminates the need for endpoint security.
   * D) It makes external attacks impossible.
3. **When conducting an initial investigation, which piece of information is the least relevant?**
   * A) The color of the affected device
   * B) The system owner and its purpose
   * C) The IP address and hostname
   * D) The actions taken on the impacted system
4. **What does an incident timeline primarily focus on?**
   * A) System performance benchmarks
   * B) Attacker behavior and event sequence
   * C) Employee activity logs
   * D) Network bandwidth usage
5. **Why should incident information be kept confidential?**
   * A) To protect the adversary's identity
   * B) To prevent leaks that could benefit the attacker
   * C) To avoid informing law enforcement
   * D) To ensure all employees are aware of the breach
6. What is an important factor when determining incident severity?
   * A) Number of impacted systems
   * B) The number of employees in the company
   * C) The software licensing cost
   * D) The length of time since the last security audit
7. What is the role of a firewall in threat detection?
   * A) Prevents phishing attacks
   * B) Blocks unauthorized access at the network perimeter
   * C) Detects anomalies in email attachments
   * D) Tracks user login times
8. If malware is involved in an incident, what information should be collected?
   * A) The number of employees who received an email about it
   * B) The type of malware and forensic details (hashes, files, etc.)
   * C) The software update history of the affected system
   * D) The cost of the affected device
9. Why is context important when analyzing security incidents?
   * A) It helps prioritize responses based on potential impact
   * B) It speeds up hardware replacement
   * C) It guarantees all incidents have the same resolution process
   * D) It eliminates the need for log analysis
10. What should be included in an incident report?

* A) The favorite color of the affected user
* B) The date, time, hostname, and event description
* C) The last software update date
* D) The marketing budget for cybersecurity awareness



### **Answers:**

1. C
2. B
3. A
4. B
5. B
6. A
7. B
8. A
9. A
10. B
