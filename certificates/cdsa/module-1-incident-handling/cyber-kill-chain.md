# Cyber Kill Chain

## **What Is The Cyber Kill Chain?**

The Cyber Kill Chain describes the lifecycle of a cyber attack, outlining how attacks manifest and progress through different stages. Understanding this lifecycle is crucial for detecting and stopping attacks at various points.

## **Stages of the Cyber Kill Chain**

1. <mark style="color:yellow;">Reconnaissance (Recon)</mark>
   * The attacker selects a target and gathers information about it.
   * Passive information gathering: Social media (LinkedIn, Instagram), company web pages, job ads.
   * Active information gathering: Scanning web applications, probing IP addresses.
2. <mark style="color:yellow;">Weaponization</mark>
   * The attacker creates malware, embedding it into an exploit or payload.
   * The goal is to achieve initial access with undetectable, persistent malware.
   * Attackers craft the malware to bypass antivirus and EDR tools.
3. <mark style="color:yellow;">Delivery</mark>
   * The attacker delivers the payload to the victim.
   * Common methods: Phishing emails, malicious websites, social engineering phone calls.
   * Attackers may use USB drives for physical delivery.
4. <mark style="color:yellow;">Exploitation</mark>
   * The payload is triggered, executing malicious code on the target system.
   * Exploits vulnerabilities to gain control or escalate privileges.
5. <mark style="color:yellow;">Installation</mark>
   * Malware is installed on the compromised system.
   * Common techniques:
     * **Droppers:** Small code snippets that install malware.
     * **Backdoors:** Maintain persistent access for attackers.
     * **Rootkits:** Hide malware presence from security tools.
6. <mark style="color:yellow;">Command & Control (C2)</mark>
   * The attacker establishes remote access to the compromised system.
   * Often modular, allowing attackers to load additional scripts and tools.
   * Advanced attackers ensure multiple malware variants exist in the network for resilience.
7. <mark style="color:yellow;">Action on Objectives</mark>
   * The attacker achieves their ultimate goal, which may include:
     * Data exfiltration (stealing confidential data).
     * Privilege escalation (gaining highest network access).
     * Deploying ransomware (encrypting data for ransom).

## **Key Takeaways**

* Attackers do not always follow the cyber kill chain in a linear fashion.
* They may repeat earlier stages (e.g., new reconnaissance after installation).
* Security teams should aim to stop attacks as early as possible in the kill chain.

***

## **Cyber Kill Chain - Quiz**

1. **What is the main purpose of understanding the Cyber Kill Chain?**
   * a) To identify vulnerabilities in an organization
   * b) To understand how an attack progresses and detect it early
   * c) To create new attack techniques
   * d) To ensure malware is undetectable
2. **Which of the following is NOT an example of passive reconnaissance?**
   * a) Searching LinkedIn for employee details
   * b) Checking job postings for technology details
   * c) Actively scanning a company's IP addresses
   * d) Reading the company's publicly available documentation
3. **What is the primary goal of the weaponization stage?**
   * a) To send phishing emails
   * b) To create malware that bypasses security tools
   * c) To deploy a ransomware attack
   * d) To scan the network for vulnerabilities
4. **Which attack vector is commonly used in the delivery stage?**
   * a) Rootkits
   * b) Social engineering phone calls
   * c) Lateral movement in a network
   * d) Exploiting software vulnerabilities
5. **During which stage does the attacker attempt to execute malicious code on the target system?**
   * a) Reconnaissance
   * b) Exploitation
   * c) Installation
   * d) Command & Control
6. **What type of malware is designed to provide an attacker with ongoing access to a system?**
   * a) Dropper
   * b) Backdoor
   * c) Rootkit
   * d) Worm
7. **What is the final stage of the Cyber Kill Chain?**
   * a) Exploitation
   * b) Command & Control
   * c) Action on Objectives
   * d) Installation
8. **Why do attackers repeat earlier stages of the Cyber Kill Chain?**
   * a) To ensure they maintain control over the network
   * b) To exfiltrate data before getting caught
   * c) To test new malware on the system
   * d) To move deeper into the network and identify new targets
9. **What is an example of an action on objectives stage?**
   * a) Sending a phishing email
   * b) Gaining administrative privileges
   * c) Encrypting company files with ransomware
   * d) Scanning an organization's external network
10. **Which is the best stage to stop an attack in the Cyber Kill Chain?**
    * a) Reconnaissance
    * b) Weaponization
    * c) Action on Objectives
    * d) Command & Control

**Answers**

1. **b)** To understand how an attack progresses and detect it early.
2. **c)** Actively scanning a company's IP addresses.
3. **b)** To create malware that bypasses security tools.
4. **d)** Exploiting software vulnerabilities.
5. **b)** Exploitation.
6. **b)** Backdoor.
7. **c)** Action on Objectives.
8. **a)** To ensure they maintain control over the network.
9. **c)** Encrypting company files with ransomware.
10. **a)** Reconnaissance.
