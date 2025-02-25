# The Triaging Process

## **What Is Alert Triaging?**

* Performed by SOC analysts to evaluate and prioritize security alerts.
* Determines threat level and potential impact on systems and data.
* Helps allocate resources effectively and respond to incidents.

## **Escalation in Alert Triaging**

* Escalation involves notifying decision-makers (supervisors, incident response teams).
* Analysts provide details such as severity, impact, and findings.
* Ensures critical alerts receive timely attention and coordinated response.

## **The Ideal Triaging Process**

1. **Initial Alert Review**
   * Analyze metadata (timestamp, source/destination IP, affected systems).
   * Review logs (network, system, application) to understand context.
2. **Alert Classification**
   * Categorize based on severity, impact, and urgency.
3. **Alert Correlation**
   * Cross-check with related alerts to find patterns and IOCs.
   * Use SIEM, logs, and threat intelligence for validation.
4. **Enrichment of Alert Data**
   * Gather additional information (network captures, file samples, threat intel).
   * Perform system reconnaissance for anomalies.
5. **Risk Assessment**
   * Assess risk based on system value, data sensitivity, and attack likelihood.
6. **Contextual Analysis**
   * Evaluate affected assets, security controls, and compliance requirements.
7. **Incident Response Planning**
   * Document alert details, assign roles, and coordinate response teams.
8. **Consultation with IT Operations**
   * Gather insights from IT teams about system status or false positives.
9. **Response Execution**
   * Decide on response actions based on investigation findings.
10. **Escalation**
    * Trigger escalation if alert severity is high or organization policy requires it.
    * Provide a detailed summary to higher-level teams.
11. **Continuous Monitoring**
    * Track response progress and provide updates.
12. **De-escalation**
    * Lower alert priority when the threat is mitigated and under control.
    * Document actions taken and lessons learned.

***

## **Quiz: The Triaging Process**

### **Multiple Choice**

1. What is the primary goal of alert triaging?\
   a) Ignoring false positives\
   b) Prioritizing security alerts based on risk and impact\
   c) Blocking all alerts immediately\
   d) Responding to every alert equally
2. Which of the following is NOT part of the ideal triaging process?\
   a) Initial alert review\
   b) Risk assessment\
   c) Hardware installation\
   d) Escalation
3. Why is alert correlation important?\
   a) It helps identify patterns and potential indicators of compromise\
   b) It replaces the need for manual analysis\
   c) It guarantees the alert is a false positive\
   d) It immediately escalates alerts to higher authorities
4. When should an alert be escalated? (Choose two)\
   a) When it involves a critical system compromise\
   b) When an insider threat is suspected\
   c) When the alert is resolved and marked as non-malicious\
   d) When no information is available for investigation
5. Which of the following helps enrich alert data?\
   a) Collecting network packet captures\
   b) Ignoring external intelligence sources\
   c) Avoiding system reconnaissance\
   d) Closing the alert without investigation

### **True/False**

1. Alert triaging only involves responding to alerts without prioritization. (False)
2. The escalation process ensures that critical alerts receive timely attention. (True)
3. Contextual analysis helps determine if an alert is due to security control failures. (True)
4. Continuous monitoring is unnecessary once an alert is escalated. (False)
5. De-escalation occurs when the risk is mitigated and further escalation is unnecessary. (True)
