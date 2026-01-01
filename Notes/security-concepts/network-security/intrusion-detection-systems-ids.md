# Intrusion Detection Systems (IDS)

What is an IDS?

A system called an intrusion detection system (IDS) observes network traffic for malicious transactions and sends immediate alerts when it is observed. It is software that checks a network or system for malicious activities or policy violations. Each illegal activity or violation is often recorded either centrally using an SIEM system or notified to an administration.&#x20;

IDS monitors a network or system for malicious activity and protects a computer network from unauthorized access from users. The intrusion detector learning task is to build a predictive model (i.e. a classifier) capable of distinguishing between ‘bad connections’ (intrusion/attacks) and ‘good (normal) connections.



### Working of Intrusion Detection System(IDS)

* An IDS (Intrusion Detection System) monitors the traffic on a computer network to detect any suspicious activity.
* It analyzes the data flowing through the network to look for patterns and signs of abnormal behavior.
* The IDS compares the network activity to a set of predefined rules and patterns to identify any activity that might indicate an attack or intrusion.
* If the IDS detects something that matches one of these rules or patterns, it sends an alert to the system administrator.
* The system administrator can then investigate the alert and take action to prevent any damage or further intrusion.

### &#x20;Types **of Intrusion Detection System(IDS)**

1. Protocol-based (PIDS)\
   A protocol-based intrusion detection system is usually installed on a web server. It monitors and analyzes the protocol between a user/device and the server. A PIDS normally sits at the front end of a server and monitors the behavior and state of the protocol.
2. Application protocol-based (APIDS)\
   An APIDS is a system or agent that usually sits inside the server party. It tracks and interprets correspondence on application-specific protocols. For example, this would monitor the SQL protocol to the middleware while transacting with the web server.
3. Hybrid intrusion detection system\
   A hybrid intrusion detection system combines two or more intrusion detection approaches. Using this system, system or host agent data combined with network information for a comprehensive view of the system. The hybrid intrusion detection system is more powerful compared to other systems. One example of Hybrid IDS is Prelude.
4. Network-based intrusion detection system (NIDS)\
   A network IDS monitors a complete protected network. It is deployed across the infrastructure at strategic points, such as the most vulnerable subnets. The NIDS monitors all traffic flowing to and from devices on the network, making determinations based on packet contents and metadata.
5. Host-based intrusion detection system (HIDS)\
   A host-based IDS monitors the computer infrastructure on which it is installed. In other words, it is deployed on a specific endpoint to protect it against internal and external threats. The IDS accomplishes this by analyzing traffic, logging malicious activity and notifying designated authorities.



## IDS evasion techniques

* **Fragmentation:** Dividing the packet into smaller packet called fragment and the process is known as fragmentation[^1]. This makes it impossible to identify an intrusion because there can’t be a malware signature.
* **Packet Encoding:** Encoding packets using methods like Base64 or hexadecimal can hide malicious content from signature-based IDS.
* **Traffic Obfuscation:** By making message more complicated to interpret, obfuscation can be utilised to hide an attack and avoid detection.
* **Encryption:** Several security features, such as data integrity, confidentiality, and data privacy, are provided by [encryption](../../cryptography/encryption-algorithm/). Unfortunately, security features are used by malware developers to hide attacks and avoid detection.



## Detection Methods of IDS

* **Signature-based Method:** Signature-based IDS detects the attacks on the basis of the specific patterns such as the number of bytes or a number of 1s or the number of 0s in the network traffic. It also detects on the basis of the already known malicious instruction sequence that is used by the malware. The detected patterns in the IDS are known as signatures. Signature-based IDS can easily detect the attacks whose pattern (signature) already exists in the system but it is quite difficult to detect new malware attacks as their pattern (signature) is not known.
* **Anomaly-based Method:** Anomaly-based IDS was introduced to detect unknown malware attacks as new malware is developed rapidly. In anomaly-based IDS there is the use of machine learning to create a trustful activity model and anything coming is compared with that model and it is declared suspicious if it is not found in the model. The machine learning-based method has a better-generalized property in comparison to signature-based IDS as these models can be trained according to the applications and hardware configurations.



## IDS vs Firewall

a firewall is a proactive security measure that prevents unauthorized access at the network perimeter, while an IDS is a reactive tool that detects and alerts about suspicious activity within the network. Both are crucial components of a comprehensive network security strategy, working together to safeguard against various threats.

<br>



[^1]: The process of dividing a data file or an executable program file, into fragments that are stored in different parts of a computer’s storage medium, such as its hard disc or RAM, is known as **fragmentation** in computing.
