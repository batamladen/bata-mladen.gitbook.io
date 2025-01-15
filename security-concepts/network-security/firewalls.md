# Firewalls

## What is a firewall?

Firewall is a network security system designed to monitor and control incoming and outgoing network traffic based on predetermined security rules. It acts as a barrier between a trusted internal network and untrusted external networks, such as the internet, to prevent unauthorized access and protect against various threats such as cyber attacks, malware, and intrusion attempts.

***

## How do firewalls work?

A firewall decides which network traffic is allowed to pass through and which traffic is deemed dangerous. Essentially, it works by filtering out the good from the bad, or the trusted from the untrusted.&#x20;

**Filtering traffic via a firewall** makes use of pre-set or dynamically learned rules for allowing and denying attempted connections. These rules are how a firewall regulates the flow of web traffic through your private network and private computer devices. Regardless of type, all firewalls may filter by some combination of the following:

* **Source:** Where an attempted connection is being made from.
* **Destination:** Where an attempted connection is intended to go.
* **Contents:** What an attempted connection is trying to send.
* **Packet protocols:** What ‘language’ an attempted connection is speaking to carry its message. Among the networking protocols that hosts use to ‘talk’ with each other, TCP/IP protocols are primarily used to communicate across the internet and within intranet/sub-networks.
* **Application protocols:** Common protocols include HTTP, Telnet, FTP, DNS, and SSH.

Source and Destination comunicate by Internet Protocol (IP) addresses and ports. Ports are typically assigned specific purposes, so certain protocols and IP addresses using uncommon ports or disabled ports can be a concern.\
By using these identifiers, a firewall can decide if a data packet attempting a connection is to be discarded—silently or with an error reply to the sender—or forwarded.

***

## Types of Firewalls

Firewall types are distinguished by their approach to:

1. Connection tracking
2. Filtering rules
3. Audit logs

Each type operates at a different level of the standardized communications model, the **Open Systems Interconnection model (OSI)**.



### Static Packet-Filtering Firewall

A static packet-filtering firewall operates at the network layer (layer 3) of the OSI model. It filters data packets based on their source and destination IP addresses, ports, and packet protocols. These firewalls don't track previously approved connections, so each connection must be re-approved with every data packet sent. They use manually created access control lists to set filtering rules, which can be rigid and require ongoing manual updates. They cannot inspect the contents of messages within packets, limiting their ability to provide comprehensive protection.



### Circuit-Level Gateway Firewall

Circuit-level gateways operate on the session level (layer 5). These firewalls check for functional packets in an attempted connection, and—if operating well—will permit a persistent open connection between the two networks. The firewall stops supervising the connection after this occurs.

Aside from its approach to connections, the circuit-level gateway can be similar to proxy firewalls.

The ongoing unmonitored connection is dangerous, as legitimate means could open the connection and later permit a malicious actor to enter uninterrupted.



### Stateful Inspection Firewall

A stateful inspection firewall operates at multiple layers of the OSI model, including the transport and application layers. Unlike static filtering, it can monitor ongoing connections and remember past ones using a state table. This allows it to make filtering decisions based on the state of connections, improving security and flexibility. It uses both administrator-defined rules and learned behaviors from past interactions to filter traffic effectively. Stateful inspection firewalls are widely used due to their ability to adapt and provide robust protection.



### Proxy Firewall

A proxy firewall, operating at the application layer (layer 7), reads and filters application protocols like FTP, HTTP, and DNS. It acts as an intermediary between external networks and internal hosts, analyzing data deeply for potential threats. Unlike other firewalls, it can understand and filter based on application-level content, providing enhanced security. However, this thorough inspection can sometimes cause delays in data delivery.



### Next-Generation Firewall (NGFW)

Evolving threats continue to demand more intense solutions, and next-generation firewalls stay on top of this issue by combining the features of a traditional firewall with network intrusion prevention systems.

Threat-specific next-generation firewalls are designed to examine and identify specific threats, such as advanced malware, at a more granular level. More frequently used by businesses and sophisticated networks, they provide a holistic solution to filtering out threats.



### Hybrid Firewall

As obvious by the name, hybrid firewalls use two or more firewall types in a single private network.
