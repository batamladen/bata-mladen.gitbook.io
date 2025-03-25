# Threat Intelligence

## **Cyber Threat Intelligence (CTI) Definition**

Cyber Threat Intelligence (CTI) is the process of collecting, analyzing, and sharing information about **potential or existing cyber threats** to help organizations understand and defend against cyberattacks. The goal of CTI is to move from a **reactive** to a **proactive** cybersecurity approach by identifying attacker tactics, techniques, and procedures (TTPs) before they can cause harm.

Cyber Threat Intelligence (CTI) is a crucial component of cybersecurity, offering valuable insights to strengthen our defenses against cyber threats. The primary objective of CTI is to shift from reactive security measures to a proactive defense strategy. CTI teams provide essential intelligence to Security Operations Centers (SOC).

***

## **Fundamental Principles of CTI**

CTI relies on four key principles to be effective:

* **Relevance:** Information must be applicable to our organization. Intelligence about vulnerabilities in software we do not use has limited value.
* **Timeliness:** Rapid dissemination of intelligence ensures mitigation measures are implemented before threats materialize.
* **Actionability:** Intelligence must provide clear directives for defensive actions. Unactionable intelligence wastes resources.
* **Accuracy:** Intelligence must be verified to avoid misattributions or incorrect indicators, ensuring effective resource allocation.

By integrating these principles, CTI helps organizations:

* Identify potential adversary operations targeting them.
* Improve data analysis through CTI and network defense collaboration.
* Discover adversary tactics, techniques, and procedures (TTPs) to enhance mitigation strategies.
* Provide decision-makers with relevant intelligence to inform strategic security decisions.

***

## **Threat Intelligence vs. Threat Hunting**

While interconnected, Threat Intelligence and Threat Hunting serve distinct roles:

* **Threat Intelligence (Predictive):** Focuses on anticipating adversary actions, identifying attack locations, timing, and methodologies.
* **Threat Hunting (Reactive & Proactive):** Investigates potential threats based on internal or external incidents, seeking signs of adversary presence or previous undetected activity.

The two are complementary to each other, but not the same.

* **Threat Intelligence (CTI)** is **predictive** and focuses on collecting, analyzing, and sharing intelligence about threats **before they happen**. It provides insights into attacker tactics, techniques, and procedures (TTPs), helping organizations prepare and defend against future attacks.
* **Threat Hunting** is **both proactive and reactive**. It involves actively searching for threats **inside the network** that may have bypassed traditional security measures. Threat hunters use indicators of compromise (IOCs) and TTPs from CTI to investigate suspicious activity.

***

## **Criteria for Effective CTI**

CTI must be:

* **Actionable** – Provide practical defense strategies.
* **Timely** – Be available when needed.
* **Relevant** – Apply to the organization’s specific risks.
* **Accurate** – Contain verified, precise information.

Quality CTI enhances:

* Threat awareness for the organization and partners.
* Visibility into the organization’s network vulnerabilities.
* Leadership's ability to mitigate risks effectively.

***

## **Categories of Cyber Threat Intelligence**

CTI is classified into three categories:

**Strategic Intelligence**

* Consumed by executives and leadership.
* Aligns intelligence with organizational risks.
* Focuses on adversary motives and long-term strategies.
* Answers: _Who? Why?_

**Example:** A report on APT28 (Fancy Bear) covering its past campaigns, political espionage motives, and long-term goals.

**Operational Intelligence**

* Provides TTPs of adversaries.
* Details ongoing adversary campaigns.
* Used by mid-level security teams.
* Answers: _How? Where?_

**Example:** A report analyzing REvil ransomware campaigns, covering initial access vectors, lateral movement techniques, and payload execution methods.

**Tactical Intelligence**

* Delivers immediate, actionable information.
* Supports network defenders in incident response.
* Includes specific technical details such as IP addresses, file hashes, and malware signatures.

**Example:** A list of IOCs linked to REvil ransomware, including command-and-control server IPs and registry modifications.

***

## **Interpreting a Tactical Threat Intelligence Report**

A structured approach ensures optimal response to intelligence reports:

1. **Understanding Context:** Assess the threat’s relevance to our organization.
2. **Identifying IOCs:** Categorize indicators (e.g., network-based, host-based, email-based).
3. **Analyzing Attack Lifecycle:** Map adversary tactics using the MITRE ATT\&CK framework.
4. **Validating IOCs:** Cross-reference with external sources (e.g., VirusTotal) to ensure accuracy.

Effective CTI empowers organizations to anticipate, detect, and mitigate cyber threats, strengthening overall cybersecurity resilience.

***

## **Quiz**

1. What is the primary goal of Cyber Threat Intelligence (CTI)? a) To react to cyber threats after they occur\
   b) To provide proactive defense against cyber threats\
   c) To replace security operations teams\
   d) To develop new malware
2. Which of the following is NOT a fundamental principle of CTI?\
   a) Relevance\
   b) Timeliness\
   c) Profitability\
   d) Accuracy
3. What type of intelligence is consumed by executives and focuses on long-term strategies?\
   a) Tactical Intelligence\
   b) Operational Intelligence\
   c) Strategic Intelligence\
   d) None of the above
4. How does Threat Intelligence support Threat Hunting?\
   a) By providing intelligence on adversary actions and tactics\
   b) By replacing manual investigations\
   c) By automating all cybersecurity defenses\
   d) By eliminating the need for security analysts
5. Which category of CTI includes IP addresses and malware signatures?\
   a) Strategic Intelligence\
   b) Operational Intelligence\
   c) Tactical Intelligence\
   d) None of the above

***

#### **Answers**

1. **b)** To provide proactive defense against cyber threats
2. **c)** Profitability
3. **c)** Strategic Intelligence
4. **a)** By providing intelligence on adversary actions and tactics
5. **c)** Tactical Intelligence
