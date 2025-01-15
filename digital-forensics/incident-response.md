# Incident Response

## Events vs Incident Response

An event is any noticeable occurrence in a system or network, such as a user connecting to a file share, a server accepting a connection request, in other words an event is something plannced and expected.

An incident is unplanned and unexpected event intended to harm the system. A security incident is an event that affects the confidentiality, integrity, or availability of information and assets in the organization.

<mark style="background-color:yellow;">All incidents are events but not all events are incidents</mark>

***

## Incident Information Sources

We can get information about an incident from: IPS & IDS SIEM Antivirus OS

***

## Cost of an Incident

Cost of an incident include:

* Valuable information and human resources
* Business reputation
* Physical loss
* Cost to restore the damage

***

## What is Incident Response (IR)?

Incident response, sometimes called incident handling, is how a company handles a data breach or cyberattack. Eventually, the objective is to manage the incident and limit the damage. A typical incident response process pursues a six-step action plan. NIST (<mark style="color:yellow;">https://csrc.nist.gov/glossary/term/incident\_response</mark>) defined it as "the mitigation of violations of security policies and recommended practices".

1. Preparation - Includes documentation, testing, training and other preparatory activities.
2. Identification - Includes the confirmation that an incident has occurred and the initial severity level. It identifies what data, devices, or systems were damaged, accessed, or exposed as part of the breach. Additionally, it includes the collection of logs, system images, and other artifacts. Activation of the CSIRT happens at this time if required.
3. Containment - Initial short-term containment of the incident will typically entail the disconnection of affected services, devices or networks to limit additional damage or malicious activity.
4. Eradication - Identify the root cause of the incident. Remove malware, malicious code and vulnerabilities from all affected systems using the identification step’s collected information.
5. Recovery - Return systems carefully back to production status, ensuring mitigation of the root cause occurs first.
6. Lessons Learned - Review the root cause of the incident and identify opportunities to improve detection and defense.

***

## NIST Incident Response (NISTIR) Lifecycle

NISTIR framework breaks the process into four phases:

1. **Preparation:** This phase involves preparing the necessary skills, tools, and resources to respond efficiently.
2. **Detection and analysis:** This is the most difficult phase. It is a challenging step for every incident response team. This phase includes networks and systems profiling, defining log retention policy, signs of incident recognition, and prioritizing security incidents.
3. **Containment, eradication, and recovery:** in this phase, digital evidence are collected for further analysis, and the operation is restored to its default state.
4. **Post-incident activity phase:** discussions are held during this phase to evaluate the team's performance and explore possible improvement options.

***

## Incident Tracking

**Incident tracking is the exercise of recording incident details in a centralized location.** Information is collected during incidents and shared across the IR team and stakeholders. There is a huge possibility you will lose some key information if you do not have a tracking mechanism in place.

Almost every step, from the incident's detection to its final resolution, should be **logged and timestamped.** Tracking incident details help with proper handling

**But what information should be logged and tracked?**

NIST suggests keeping track of the following information for each incident: • Incident status (new, in progress, forwarded for investigation, resolved, etc.) • Incident summary. • Other incidents related to this incident • Actions performed by all incident handlers on this incident. • Impact assessment. • Contact information for other involved parties (e.g., system owners, system administrators) • A list of evidence gathered during the incident investigation

***

## Incident Classification and Priorization

One key aspect of successful IR is resource utilization. No matter how big the IR team is, they have a capacity limit and an intense need to prioritize incidents based on impact severity. A backdoor on the email server incident would never have the same severity as a phishing attack.

Sevierity levels:

* critical
* high
* medium
* low
* informative

<figure><img src="../.gitbook/assets/Pasted image 20241209111937 (1).png" alt=""><figcaption></figcaption></figure>

**High-level incident:** A high-level incident is an incident that is expected to cause significant damage, corruption, or loss of critical and/or strategic company or customer information.

**Moderate-level incident:** A moderate-level incident is an incident that may cause damage, corruption, or loss of replaceable information

**Low-level incident:** A low-level incident is an incident that causes inconvenience and/or unintentional damage or loss of recoverable information.

***

## Phase 1: Preparation

The preparation phase involves anything we can do before the incident to make the response process efficient. It's also an indicator of how well the IR team will perform. The more you prepare, the better you will do. **By failing to prepare, you are preparing to fail.**

This phase countians: • Preventive strategies and controls • Effective incident communication planning in IR • IR architecture: • IR-friendly network • Defense in Dept • Zero trust • Never trust, always verify • Standardized logging • IR Policy

***

## Phase 2: Detection & Analysis

**Detection** After good preparation, comes the detection and analysis phase. Most responders consider this the most challenging phase in the IR lifecycle because it requires a solid combination of technical skills, infrastructure and tools, and processes.

**Analysis** Once an incident has been detected, personnel from the organization or a trusted third party will begin the analysis phase. In this phase, personnel begin the task of collecting evidence from systems such as running memory, log files, network connections, and running software processes. Depending on the type of incident, this collection can take as little as a few hours to several days.

***

## Phase 3: Containment, Eradication and Recovery

So far, we have spent some time preparing and were able to detect the attack. The remaining part of the IR exercise can be summarized into:

• Stop further damage (Containment). • Fix present damage and prevent it from happening again (Eradication). • Restore clean-state (Recovery).

The term **"contain"** means keeping something within a limit. In incident response, containment is a set of action the IR team perform to limit the incident and stop the threat from causing further damage.

Responders should ask the following question for every containment action they perform: will that action impact the attack evidence? Evidence is traces that get left behind based on the attacker's activities, i.e., the attacker's footprints. If yes, then it should be performed after taking forensic images. Otherwise, responders may risk 15losing vital attack details not found elsewhere.

• Pre evidence collection containment • Evidence collection • Post evidence collection containment

***

## Phase 4: Eradication

EWe have seen that the containments phase objective is to stop further damage. The objective of this phase its to prevent the present damage through the following steps:

1. **Root cause analysis** The first step is to conduct a root cause analysis (RCA) to identify the underlying cause of the incident and determine appropriate solutions. The word root means going down to the deepest possible level to understand how the attack was executed and figure out the attacker's entry point.
2. Remediation and improving defenses Based on the root cause analysis, we figured out how the attacker executed the attack, the attack scope, what happened from the attacker's first hit to the moment of detection, and in which order. The next step is to remediate the root cause of the attack and improve defenses. In other words, removing the vulnerability!
3. Identifying other vulnerable systems Identifying other vulnerable systems often translates into a vulnerability analysis exercise
4. Removing attacker's tools and artifacts Upon eliminating the vulnerability and improving defenses, we should clean up the mess the attacker caused by removing the tools the attacker downloaded, the user accounts he created, and other persistence mechanisms he may have deployed

***

## Phase 5: Recovery

1. Restore The first step is recovering the system data from a recent backup and restoring operations
2. Validate Putting the system back into production is just the beginning. The next step is verifying the system's operational and security aspects.
3. Monitor Finally comes the monitoring part. After validating that the system is in a functional state, it's time to monitor the system closely to alert for any abnormal activity in the few days or weeks after restoring it.

***

## Phase 6: Post-Incident review

Here we come to the end of the IR journey! The final phase in the process is postincident activities. It focuses on two primary things: documenting everything from start to end and extracting lessons learned to improve the Incident Response process, tech, and team performance.

* Lessons Learned meeting
* Prepare the meeting Agenda
* Invite all parties
* Explore improvement areas
* Incident Rport
* Tehnical Investigatin (who, when?)

***

## Incident Response Team



<figure><img src="../.gitbook/assets/IR Team.png" alt=""><figcaption></figcaption></figure>
