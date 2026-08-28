# Cloud Service Types

The cloud service types are:

* IaaS – infrastructure as a service
* Paas – platform as a service
* Saas – software as a service

&#x20;

## Iaas

&#x20;

IaaS (Infrastructure as a Service): A cloud computing model where a provider delivers virtualized computing resources—like servers, storage, and networking—over the internet, so you don’t have to manage physical hardware.

The cloud vendor is responsible for the physical 3 (datacenter, physical servers, networking/cabling)

IaaS is managed infrastructure and this "infrastructure" always refers to VMs.

Basic example of an IaaS service provider is Contaboo, they rent you VMs where you are responsible for everything except the above mentioned physical 3.

&#x20;

## Paas

Fully managed cloud solution.

Managed and prepacked services for developing apps/solutions.

* No managin of infra
* Instead, focus on code

Specialized services for different use cases (managed db, managed storage...)

Serverless = Extreme Paas

* Just run code with no infra management at all



<figure><img src="../../../.gitbook/assets/Screenshot 2026-04-06 114721.png" alt=""><figcaption></figcaption></figure>



## Saas

Is a cloud based application

Pros:

* Redy-to-go software, just download it or open in browser
* Subscription based

&#x20;

***

## Defense in Depth

### What is Def in Depth?

A multi-layered approach to security.

Objective is to slow or stop unauthorized data access

If one layer is breached other layers reman



<figure><img src="../../../.gitbook/assets/Screenshot 2026-04-06 120326.png" alt=""><figcaption></figcaption></figure>



<figure><img src="../../../.gitbook/assets/Screenshot 2026-04-06 120254.png" alt=""><figcaption></figcaption></figure>



### Understanding Def in Depth Layers

&#x20;

&#x20;

Physical security

* First line of def
* &#x20;building/hardware access

Identity & Access

* Securing identities
* Giving only the bare minimum so job can be done

Perimeter

* Protect agains network based attacks
* Perimeter firewall/ddos protection

Network

* Secure connectivity btw resources
* Perimeter and network are related but network is more focues on limiting network conn inside our network

Compute

* Harden VMs
* Endpoint prtection/updates

Appliciation

* Resolve vulns in apps
* Secure secrets

Data layer

* Primart target
* databases, disks, SaaS apps





