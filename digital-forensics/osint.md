# OSINT

## Cyber Threat Intelligence

**What is Cyber Threat Intelligence ?** Threat intelligence—also called ‘cyber threat intelligence’ (CTI) or ‘threat intel’—is data containing detailed knowledge about the cybersecurity threats targeting an organization. Threat intelligence helps security teams be more proactive, enabling them to take effective, data-driven actions to prevent cyber attacks before they occur. It can also help an organization better detect and respond to attacks in progress.

**Cyber Threat Intelligence – Types** The four main types of threat intelligence are strategic, tactical, technical, and operational.

<figure><img src="../.gitbook/assets/cyber threat types.png" alt=""><figcaption></figcaption></figure>

* **Strategic** cyberthreat intelligence is a broader term usually reserved for a nontechnical audience. It uses **detailed analyses of trends and emerging risks to create a general picture of the possible consequences of a cyberattack**. Simply put, it asks the question: “Given our technical landscape, what’s the worst that can happen?” This information is often presented to high-level decision makers within an organization, like board members, so it focuses on broader impacts. Some examples include whitepapers, policy documents, and publications distributed within the industry.
* **Tactical** threat intelligence offers **more specific details on threat actors’ tactics, techniques, and procedures, also known as TTPs**. It’s intended for a predominantly technical audience and helps them understand how their network might be attacked based on the latest methods attackers use to achieve their goals. They look for Indicators of Compromise (IOCs) evidence like IP addresses, URLs, and system logs to use to help detect future data breach attempts. Tactical, evidence-based threat intelligence is usually reserved for security teams or the **people in an organization directly involved with protecting the network**.
* **Technical** threat intelligence focuses on the technical clues indicative of a cybersecurity threat, like the subject lines to phishing emails or fraudulent URLs. This type of threat intelligence is important because it gives people an idea of what to look for, making it useful for analyzing social engineering attacks. However, since hackers change up their tactics, techniques, and procedures frequently, technical threat intelligence has a short shelf life.
* **Operational** threat intelligence helps IT defenders understand the nature of specific cyberattacks by detailing relevant factors like nature, intent, timing, and sophistication of the group responsible. Operational threat intelligence is where you get into secret agent stuff like infiltrating hacker chat rooms. Less experienced threat groups might discuss their evil deeds online, but the good ones probably won’t, so operational intelligence can mean playing the long game. Still, all facets of cyberthreat intelligence are necessary for a comprehensive threat assessment.

***

## The threat intelligence lifecycle

The threat intelligence lifecycle is the iterative, ongoing process by which security teams produce, disseminate and continually improve their threat intelligence. While the particulars can vary from organization to organization, most follow some version of the same five-step process.

**Step 1:** **Planning** \
Security analysts work with organizational stakeholders—executive leaders, department heads, IT and security team members, and others involved in cybersecurity decision-making—to set intelligence requirements. These typically include cybersecurity questions that stakeholders want or need to have answered. For example, the CISO may want to know whether a new, headline-making strain of ransomware is likely to affect the organization.

**Step 2: Threat Data Collection** \
The security team collects any raw threat data that may hold—or contribute to—the answers stakeholders are looking for. Continuing the example above, if a security team is investigating a new ransomware strain, the team might gather information on the ransomware gang behind the attacks, the types of organizations they’ve targeted in the past, and the attack vectors they’ve exploited to infect previous victims. This threat data can come from a variety of sources, including:

* **Threat intelligence feeds**—streams of real-time threat information. The name is sometimes misleading: While some feeds include processed or analyzed threat intelligence, others consist of raw threat data. (The latter are sometimes called ‘threat data feeds.’)

Security teams typically subscribe to multiple open-source and commercial feeds. For example, one feed might track IoCs of common attacks, another feed might aggregate cybersecurity news, a another might provide detailed analyses of malware strains, and a fourth might scrape social media and the dark web for conversations surrounding emerging cyber threats. All of it can contribute to deeper understanding of threats.

* **Information-sharing communities**—forums, professional associations, and other communities where analysts from share firsthand experiences, insights, and their own threat data.

In the U.S., many critical infrastructure sectors—such as the healthcare, financial services, and oil and gas industries—operate industry-specific Information Sharing and Analysis Centers (ISACs). These ISACs coordinate with one another via the National Council of ISACs (NSI). Internationally, the open-source MISP Threat Sharing intelligence platform supports a number of information-sharing communities organized around different locations, industries, and topics. MISP has received financial backing from both NATO and the European Union.

Internal security logs—internal security data from security and compliance systems such as SIEM, SOAR (security orchestration, automation and response), EDR (endpoint detection and response), XDR (extended detection and response), and attack surface management (ASM) systems. This data provides a record of the threats and cyberattacks the organization has faced, and can help uncover previously unrecognized evidence of internal or external threats. Information from these disparate sources is typically aggregated in a centralized dashboard, such as a SIEM or a threat intelligence platform, for easier management.

**Step 3: Processing** \
At this stage, security analysts aggregate, standardize, and correlate the raw data they’ve gathered to make it easier to analyze the data for insights. This might include filtering out false positives, or applying a threat intelligence framework, such as MITRE ATT\&CK, to data surrounding a previous security incident, to better Many threat intelligence tools automate this processing, using artificial intelligence (AI) and machine learning to correlate threat information from multiple sources and identify initial trends or patterns in the data.

**Step 4: Analysis** \
Analysis is the point at which raw threat data becomes true threat intelligence. At this stage, security analysts test and verify trends, patterns, and other insights they can use to answer stakeholders’ security requirements and make recommendations. For example, if security analysts find that the gang connected with a new ransomware strain has targeted other businesses in the organizations industry, the team may identify specific vulnerabilities in the organization’s IT infrastructure that the gang is likely to exploit, as well as security controls or patches that might mitigate or eliminate those vulnerabilities.

**Step 5: Dissemination** \
The security team shares its insights and recommendations with the appropriate stakeholders. Action may be taken based on these recommendations, such as establishing new SIEM detection rules to target newly identified IoCs or updating firewall blacklists to block traffic from newly identified suspicious IP addresses. Many threat intelligence tools integrate and share data with security tools such as SOARs or XDRs, to automatically generate alerts for active attacks, assign risk scores for threat prioritization, or trigger other actions.

***

## OSINT

**What is Open-Source Intelligence?** Open-Source Intelligence (OSINT) is defined as intelligence produced by collecting, evaluating and analyzing publicly available information with the purpose of answering a specific intelligence question.

**Who Uses OSINT?**

* Government
* Law Enforcement
* Military
* Investigative journalists
* Human rights investigators
* Private Investigators
* Law firms
* Information Security
* Cyber Threat Intelligence
* Pen Testers

***

## Types of OSINT

There are 2 types of OSINT: **Pasive and Active**

**Passive** means you do not engage with a target.Passive open-source collection is defined as gathering information about a target using publicly available information. Passive means there will be no communicating or engaging with individuals online, which includes commenting, messaging, friending, and/or following.

**Active** means you are engaging with a target in some fashion, i.e. adding the target as a friend on social profiles, liking, commenting on the target’s social media posts, messaging the target, etc. Active open-source research is considered engagement and can be looked upon as an undercover operation for some organizations. Please be aware of the differences and request clarification from your agency prior to engaging.

***

## OSINT Tools List

* OSINT Framewor
* Prowl.lupovis.io
* Censys
* Wappalyzer
* CheckUserNames
* HaveIBeenPwned
* TheHarvester
* Shodan
* Nmap
* WebShag
* Jigsaw
* Fierce
* Uniornscan
* Google Dorks
* Maltego
* Spidergoot
* Creepy
* Recon-Ng
* Grepp App
* Leakix
* OpenVas
* Foca
* IVRE
* Exiftool
* Zoomeye
* OWASP Amass
* AllInOne Search Engine
* Carrot2
* Waybackmachine
* searchftps
