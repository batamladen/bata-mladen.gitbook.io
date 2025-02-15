# Preparation Stage (Part 2)

While protection isn’t solely the responsibility of incident handlers, they must be aware of security measures to better analyze incidents and collect evidence.

## **Key Protective Measures**

### **DMARC (Email Security)**

* Prevents phishing by rejecting spoofed emails.
* Requires careful testing to avoid blocking legitimate emails.

### **Endpoint Hardening & EDR**

* Workstations are primary attack targets.
* Key protections:
  * Disable LLMNR/NetBIOS.
  * Implement LAPS & remove admin rights from users.
  * Configure PowerShell in **Constrained Language** mode.
  * Enable **Attack Surface Reduction (ASR)** rules in Microsoft Defender.
  * Implement whitelisting & block execution from user-writable folders.
  * Deploy **EDR** solutions that integrate with **AMSI**.

### **Network Protection**

* Use **network segmentation** to limit lateral movement.
* IDS/IPS systems work best with **SSL/TLS interception**.
* Use **802.1x** or **Conditional Access** to limit network access to approved devices.

### **Privilege Identity Management (PIM), MFA & Password Security**

* **Weak but complex** passwords are still easy to crack (e.g., "Password1!").
* Use **passphrases** instead (e.g., "i LIK3 my coffeE warm").
* Enforce **MFA** for all administrative access.

### **Vulnerability Scanning**

* Regularly scan and remediate **high** & **critical** vulnerabilities.
* If patches aren’t possible, **segment** vulnerable systems.

### **User Awareness Training**

Training users to recognize suspicious behavior and report it when discovered is a big win for us. While it is unlikely to reach 100% success on this task, these trainings are known to significantly reduce the number of successful compromises. Periodic "surprise" testing should also be part of this training, including, for example, monthly phishing emails, dropped USB sticks in the office building, etc.

### **Active Directory Security Assessment**

* Identify misconfigurations & privilege escalation risks.
* Regularly review security settings & test for new vulnerabilities.

### **Purple Team Exercises**

* Red teams simulate attacks, blue teams defend and learn.
* Improves detection, logging, & incident response.

**Bottom Line:** Protection measures reduce risks but must be continuously tested and updated to stay ahead of threats. 🚀🔒

***

## **Quiz: Preparation Stage (Part 2)**

**1. What is the main purpose of DMARC?**\
A) To block all incoming emails from external domains\
B) To prevent email spoofing and phishing attacks\
C) To encrypt emails end-to-end\
D) To blacklist suspicious email addresses

**2. Why is endpoint hardening important?**\
A) It prevents users from browsing non-work-related websites\
B) It ensures that software updates are applied automatically\
C) It protects against threats that target users through websites, emails, or executables\
D) It allows administrative privileges for all users

**3. Which of the following is NOT a recommended endpoint security measure?**\
A) Disabling LLMNR/NetBIOS\
B) Allowing unrestricted execution of scripts (.js, .bat, .vbs)\
C) Enabling Attack Surface Reduction (ASR) rules\
D) Deploying an Endpoint Detection and Response (EDR) solution

**4. What is the main advantage of network segmentation?**\
A) It reduces the load on network bandwidth\
B) It isolates critical systems to prevent lateral movement during an attack\
C) It makes internet access faster for employees\
D) It eliminates the need for antivirus software

**5. What is a common mistake with privileged user credentials?**\
A) Using passphrases instead of passwords\
B) Enforcing multi-factor authentication (MFA)\
C) Using the same password for admin and regular accounts\
D) Regularly updating passwords

**6. Why should vulnerability scanning be performed continuously?**\
A) To keep track of software licenses\
B) To identify and remediate security weaknesses before they can be exploited\
C) To improve internet speed within the organization\
D) To ensure all employees are following security policies

**7. How can user awareness training help improve security?**\
A) By testing users with simulated phishing attacks and other security exercises\
B) By removing the need for antivirus software\
C) By giving all employees administrator privileges\
D) By disabling password protection requirements

**8. What is an Active Directory Security Assessment used for?**\
A) To identify and fix security misconfigurations that attackers could exploit\
B) To improve Active Directory performance speed\
C) To store all user passwords in plaintext for easy access\
D) To allow unlimited access between all systems on the network

**9. What is the goal of a Purple Team exercise?**\
A) To simulate a real attack and improve both offensive (Red Team) and defensive (Blue Team) capabilities\
B) To eliminate the need for an incident response team\
C) To replace vulnerability scanning with manual testing\
D) To improve the speed of security updates

**10. Why is implementing Multi-Factor Authentication (MFA) important for administrative access?**\
A) It allows users to log in without a password\
B) It prevents unauthorized access even if credentials are stolen\
C) It removes the need for security monitoring\
D) It allows multiple users to share one account securely



### **Answers:**

1. B
2. C
3. B
4. B
5. C
6. B
7. A
8. A
9. A
10. B
