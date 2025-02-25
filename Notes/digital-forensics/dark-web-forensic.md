# Dark Web Forensic

## Understand Dark Web

The dark web refers to the last and the bottom-most layer of the web, which is not indexed by search engines; hence, its contents remain hidden from normal browsers and users. Access to the dark web requires individuals to use special browsers such as Tor that provide users with anonymity and keep their data safe. Because of the anonymity allowed to dark web users, criminals use the dark web to perform a wide range of illegal activities. This section outlines the fundamentals of the dark web and discusses the characteristics of the Tor network.

There are **3 different layers of the internet**:

* **Surface Web** - As the topmost layer, the surface web stores content that can be accessed as well as indexed by search engines such as Google, Yahoo, and Bing. Public websites such as Wikipedia, eBay, Facebook, and YouTube can be easily accessed from the surface web. The surface web comprises only 4% of the entire web.
* **Deep Web** - This layer of the web cannot be accessed by normal users because its contents are not indexed by search engines. The contents of the deep web can be accessed only by a user with due authorization. Information contained in the deep web can include military data, confidential data of organizations, legal dossiers, financial records, medical records, records of governmental departments and subscription information.
* **Dark Web** - This is the third and the deepest layer of the web. It is an invisible or a hidden part of the web that requires specific web browsers such as the Tor browser to access; such browsers protect the anonymity of the users. It is used to carry out unlawful and antisocial activities. The dark web is not indexed by search engines and allows complete anonymity to its users through encryption. Cyber criminals use the dark web to perform nefarious activities such as drug trafficking, anti-social campaigns, and the use of cryptocurrency for illegal transactions.

***

### Tor Relays

The Tor network has three relays: an entry/guard relay, a middle relay, and an exit relay. These relays are also called nodes or routers and allow network traffic to pass through them.

1. **Entry/Guard Relay** This relay provides an entry point to the Tor network. When attempting to connect via the entry relay, the IP address of the client can be read. The entry relay/guard node transmits the client’s data to the middle node.
2. **Middle Relay** The middle relay is used for the transmission of data in an encrypted format. It receives the client’s data from the entry relay and passes it to the exit relay.
3. **Exit Relay** As the final relay of the Tor circuit, the exit relay receives the client’s data from the middle relay and sends the data to the destination website’s server. The exit relay’s IP address is directly visible to the destination. Hence, in the event of transmission of malicious traffic, the exit relay is suspected to be the culprit, as it is perceived to be the origin of such malicious traffic. Hence, the exit relay faces the most exposure to legal issues, take-down notices, complaints, etc., even when it is not the origin of malicious traffic.

Note: All the above relays of the Tor network are listed in the public list of Tor relays.

***

### Working on the Tor Browser

The Tor broser is based on Mozilla's Firefox web browser and works on the concept of **onion routing**

In onion routing, the traffic is encrypted and passes through different relays present in the Tor circuit. This multi-layered encrypted connection makes the user identity anonymous.

The Tor browser provides access to .onion websites available on the dark web

Tor's hidden service protocol allows users to host websites anonymous with .BIT domains and these websites can only be accessed by users on the Tor network.



<figure><img src="../.gitbook/assets/Pasted image 20250107153756.png" alt=""><figcaption></figcaption></figure>

***

### Tor Bridge Node

The Tor relay nodes are publicly available in the directory list, but the bridge node is different from relay nodes. Bridge nodes are nodes that are not published or listed in the public directory of Tor nodes.

Several entry and exit nodes of the Tor network are publicly listed and accessible on the Internet; consequently, they can be blocked by organizations/governments, if they wish to prohibit the usage of Tor. In many authoritarian countries, governments, Internet Service Providers (ISPs), and corporate organizations ban the use of the Tor network. In such scenarios, where the usage of the Tor network is restricted, bridge nodes help circumvent the restrictions and allow users to access the Tor network. The usage of bridge nodes makes it difficult for governments, organizations, and ISPs to censor the usage of the Tor network.

**How Bridge Nodes Help Circumvent Restrictions on the Tor Network** Bridge nodes exist as proxies in the Tor network, and not all of them are publicly listed in the Tor directory of nodes; several bridge nodes are concealed/hidden. Hence, ISPs, organizations, and governments cannot detect their IP addresses or block them. Even if ISPs and organizations detect some of the bridge nodes and censor them, users can simply switch over to other bridge nodes.

A Tor user transmits traffic to the bridge node, which transmits it to a guard node as selected by the user. Communication with a remote server occurs normally; however, an extra node of transmission is involved, i.e., the bridge node. The use of concealed bridge nodes as proxies help users circumvent the restrictions placed on the Tor network.

***

## Understand Dark Web Forensics

Although the Tor browser provides anonymity to its users, artifacts pertaining to the activities performed on it reside on the system RAM as long as the system is not powered off. Investigators can acquire a RAM dump of the live suspect machine to identify and analyze the artifacts pertaining to malicious use of the Tor browser. If the Tor browser was used on a Windows system, investigators can also identify and analyze artifacts pertaining to the Tor browser that are recorded in Windows Registry and the prefetch folder.

***

### Identifying Tor Browser Artifacts: Command Prompt

When Tor browser is installed on a Windows machine, it uses port 9150/9151 for establishing connection via Tor nodes. When investigators test for the active network connections on the machine by using the command netstat -ano, they will be able to identify whether Tor was used on the machine.



<figure><img src="../.gitbook/assets/tor cmd.png" alt=""><figcaption></figcaption></figure>

***

### Identifying Tor Browser Artifacts: Windows Registry

When the Tor browser is installed on a Windows machine, the user activity is recorded in Windows Registry. Forensic investigators can obtain the path from where the TOR browser is executed in the following Registry key: HKEY\_USERS\<SID>\SOFTWARE\Mozilla\Firefox\Launcher



<figure><img src="../.gitbook/assets/Tor wind registry.png" alt=""><figcaption></figcaption></figure>

***

### Extracting Last Execution Date and Time of the Tor Browser

On a suspect machine, the investigator analyzes the ‘State’ file located in the path where the Tor browser was executed. The directory of the State file in the Tor browser folder is \*_\Tor Browser\Browser\TorBrowser\Data\Tor\*_



<figure><img src="../.gitbook/assets/tor data.png" alt=""><figcaption></figcaption></figure>

***

### Identifying Tor Browser Artifacts: Prefetch Files

When the Tor browser is uninstalled from a machine, or if it is installed in a location other than the desktop (in Windows), it will be difficult for investigators to know whether it was used or the location where it is installed. Examining the prefetch files will help investigators in obtaining this information. The prefetch files are located in the directory **C:\WINDOWS\Prefetch** on a Windows machine.

Using tools such as **WinPrefetchView**, investigators can obtain metadata related to the browser, which includes browser created timestamps, browser last run timestamps, number of times the browser was executed, Tor browser execution directory, Filename and File Size.



<figure><img src="../.gitbook/assets/Win Prefetch.png" alt=""><figcaption></figcaption></figure>

***

### Dark Web Forensics Challenges

The following are the challenges involved in Dark Web Forensics: • High level of anonymity: The dark web allows perpetrators to carry out illegal activities by hiding their identity • Encrypted networks: Tracing the physical location of the perpetrators is difficult. • Limited number of traces: When the Tor browser is uninstalled on a suspect machine, the investigator is left with a limited number of artifacts, which makes the investigation process difficult. • Legal jurisdiction issues: The criminal activities on the dark web occur irrespective of the jurisdiction, posing legal jurisdiction issues to investigators and law enforcement agencies.

***

## Perform Tor Browser Forensics

The method and outcome of performing Tor browser forensics differ based on the status of the Tor browser on the suspect machine. This section discusses Tor browser forensics concepts including memory acquisition, collecting, and analyzing memory dumps.

***

### Tor Browser Forensics: Memory Acquisition

RAM contains volatile information pertaining to various processes and applications running on a system. Examining RAM dumps can provide deep insights regarding the actions that occurred on the system. Forensic investigators can examine these dumps in an attempt to extract various Tor browser artifacts that help in reconstructing the incident.

The results obtained by examining Tor browser artifacts differ based on the following conditions: • Tor Browser opened • Tor Browser closed • Tor Browser uninstalled

A memory dump taken while the browser is opened collects the most number of artifacts, while a dump taken post browser uninstallation collects the least. Memory dumps taken while the browser is closed contain most of the information that is found in memory dumps collected when the browser is left opened.

***

### Collecting Memory Dumps

Investigators need to acquire a memory dump of the suspect machine to begin the forensic examination. Tools such as Belkasoft LIVE RAM Capturer and AccessData FTK Imager can help capture RAM. The memory dump collected from the suspect machine not only contains artifacts related to the browser, but also related to all the activities that occurred on it.

<figure><img src="../.gitbook/assets/Pasted image 20250107160334.png" alt=""><figcaption></figcaption></figure>

***

### Memory Dump Analysis: Bulk Extractor

The memory dump acquired from the machine must be examined on the forensic workstation to discover artifacts that may potentially be helpful during investigation. Tools such as Bulk Extractor help in processing these dumps and providing useful information such as the URLs browsed, email IDs used, and personally identifiable information entered in the websites.

**Bulk Extractor** Source: https://digitalcorpora.org Bulk Extractor is a program that extracts features such as email addresses, credit card numbers, URLs, and other types of information from digital evidence files. Bulk Extractor Viewer is a UI for browsing features that have been extracted via the Bulk Extractor feature extraction tool. BEViewer supports browsing multiple images and bookmarking and exporting features. BEViewer also provides a User Interface for launching Bulk Extractor scans.



<figure><img src="../.gitbook/assets/tor bulk (1).png" alt=""><figcaption></figcaption></figure>
