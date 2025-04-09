# Introduction to Active Directory

## **What is Active Directory?**

* **Active Directory (AD)** is an authentication, authorization and centralized management service that runs on **Windows Servers** and is used to organize and control **users, computers, and rules** within a network.
* The **main components** of AD include:
  * **Domains** – The **core unit** that groups users, computers, and resources under a common security boundary.
  * **Organizational Units (OUs)** – **Sub-divisions within a domain** used to organize users, computers, and apply policies more efficiently.
  * **Group Policies (GPOs)** – **Rules and settings** applied to users and computers to enforce security and management policies.

AD is based on **LDAP** and **X.500** protocols.

**Key Functions:**

* Manages permissions and access to network resources.
* Provides **single sign-on (SSO)** for users.
* Stores critical data in **NTDS.DIT** (AD database file).

***

## **Why is AD Important?**

* Most widely used **Identity & Access Management (IAM)** solution.
* A compromised AD means **full access** to all systems and data (violating **CIA**—Confidentiality, Integrity, Availability).
* Frequent vulnerabilities (over **3000** in the last 3 years) require **proper patch management**.

***

## **AD Structure – Key Terms**

* **Domain** – Group of objects sharing the same AD database.
* **Tree** – Collection of related domains (e.g., `test.local`, `dev.test.local`).
* **Forest** – Top-level structure containing multiple trees.
* **Organizational Unit (OU)** – Container for users, groups, and computers.
* **Domain Controller (DC)** – Server managing AD authentication and policies.

***

## **Authentication in AD**

* **Username/Password** (stored as **NTLM/LM hashes**).

**Kerberos Tickets** (TGT for identity proof, TGS for service access).

* Key Distribution Center `(`KDC)\*\*: a Kerberos service installed on a DC that creates tickets. Components of the KDC are the authentication server (AS) and the ticket-granting server (TGS).
* `Kerberos Tickets` are tokens that serve as proof of identity (created by the KDC):
* `TGT` is proof that the client submitted valid user information to the KDC.
* `TGS` is created for each service the client (with a valid TGT) wants to access.
* **LDAP** (a protocol that systems in the network environment use to communicate with Active Directory.).

***

## **Security Risks & Best Practices**

* **Default groups (like Domain Admins, Enterprise Admins) are highly privileged** – misuse leads to major breaches.
* **Account Operators group** can be abused for privilege escalation.
* **Logon types** leave credential traces (except **Network Logon, Type 3**).

***

## **Key Network Ports**

* 53 (DNS)
* 88 (Kerberos),
* 389/636 (LDAP)
* 445 (SMB)
* 3389 (RDP)
* 5985/5986 (WinRM).

***

## **AD Weaknesses & Attack Vectors**

1. **Complexity** – Nested group memberships can lead to unintended admin access.
2. **Design Flaws** – SMB protocol allows remote code execution on DCs.
3. **Legacy Issues** – **NetBIOS/LLMNR** broadcast credentials, making them easy to steal.

***

## **Real-World Considerations**

* **Classify critical systems** (ERP, backups, etc.) to prioritize security.
* **Services like DNS, PKI, or SCCM can be exploited for AD takeover.**

**Next Steps:**

* Study **AD Enumeration & Attacks** for common exploitation techniques.
* Ensure **patch management** and **least privilege principles** are enforced.
