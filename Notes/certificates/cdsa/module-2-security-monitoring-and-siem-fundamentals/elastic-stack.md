# Elastic Stack

### What is the Elastic Stack?

The **Elastic Stack** (formerly ELK Stack) is an open-source collection of tools developed by Elastic for searching, analyzing, and visualizing log data in real time. It consists of:

* **Elasticsearch** – A distributed search and analytics engine.
* **Logstash** – A data processing pipeline that ingests, transforms, and forwards data.
* **Kibana** – A visualization tool for exploring and interacting with Elasticsearch data.
* **Beats** – Lightweight data shippers that send logs and metrics to Logstash or Elasticsearch.

#### High-Level Architecture

In high-demand environments, Elastic Stack can be enhanced with:

* **Kafka, RabbitMQ, and Redis** for buffering and resilience.
* **nginx** for added security.

### Components of the Elastic Stack

#### 1. Elasticsearch

* Distributed, JSON-based search and analytics engine.
* Uses RESTful APIs.
* Stores, indexes, and queries log data.

#### 2. Logstash

* Collects, processes, and transports log records.
* **Three main functions:**
  * **Input processing:** Collects logs from sources like flat files, TCP sockets, or syslog.
  * **Transformation and enrichment:** Modifies and normalizes log data using filter plugins.
  * **Output transmission:** Sends processed logs to Elasticsearch.

#### 3. Kibana

* Visualization tool for Elasticsearch data.
* Provides tables, charts, and dashboards for analysis.
* Used extensively by **SOC analysts** for security monitoring.

#### 4. Beats

* Lightweight shippers installed on remote machines.
* Forwards logs and metrics directly to **Logstash** or **Elasticsearch**.

**Data Flow in the Elastic Stack:**

1. **Beats → Logstash → Elasticsearch → Kibana**
2. **Beats → Elasticsearch → Kibana**
3. **Beats → Logstash**
4. **Beats → Elasticsearch**

***

### The Elastic Stack as a SIEM Solution

The Elastic Stack can function as a **Security Information and Event Management (SIEM)** system to:

* Collect logs from **firewalls, IDS/IPS, endpoints**, etc.
* Store and index security data in **Elasticsearch**.
* Visualize and analyze threats using **Kibana**.
* Detect security incidents via search and correlation.

**Key Tools for SOC Analysts:**

* **Kibana** for dashboards and reports.
* **Elasticsearch** for performing complex queries on security data.

***

### Kibana Query Language (KQL)

KQL is a user-friendly query language for searching and analyzing Elasticsearch data in Kibana.

#### 1. Basic Structure

Queries use **field**\*\*:value\*\* pairs:

```
 event.code:4625
```

* Filters logs with Windows Event ID **4625** (failed login attempts).
* Helps SOC analysts detect brute-force attacks or password guessing.

#### 2. Free Text Search

```
 "svc-sql1"
```

* Searches for the string **svc-sql1** across all fields.

#### 3. Logical Operators

We can use logical operators for our searches, such as: **AND**, **OR**, **NOT**.

Example:

```
 event.code:4625 AND winlog.event_data.SubStatus:0xC0000072
```

* Filters failed login attempts where **account is disabled (SubStatus: 0xC0000072).**
* Helps identify attackers using compromised credentials.

#### 4. Comparison Operators

We can use comparision operators for our searches, such as: **:** , **>** , **>=** , **<** , **<=** , **!.**

Example:

```
 event.code:4625 AND @timestamp >= "2023-03-03T00:00:00.000Z" AND @timestamp <= "2023-03-06T23:59:59.999Z"
```

* Finds failed logins within a **specific date range**.

#### 5. Wildcards & Regular Expressions

```
 event.code:4625 AND user.name: admin*
```

* Identifies failed logins for accounts starting with **"admin"**.

***

### Identifying Available Data in Kibana

#### 1. **Using Free Text Search**

* Use the **Discover** feature to explore indexed logs.
* Example: Searching for **"4625"** reveals fields such as:
  * `event.code:4625` (Elastic Common Schema field)
  * `winlog.event_id:4625` (Winlogbeat field)

#### 2. **Checking Elastic Documentation**

* Elastic provides extensive documentation on field names and log structures.
* Example: **Windows Event ID 4625** details available at security log resources.

#### 3. **Understanding Field Naming Conventions**

* **ECS (Elastic Common Schema)**: Standardized field names for interoperability.
* **Winlogbeat Fields**: Specific to Windows event logs.
* **@timestamp**: Stores the extracted event time (different from `event.created`).

***

### Summary

* **Elastic Stack** is a powerful tool for log collection, search, and visualization.
* **Elasticsearch** indexes and analyzes data, **Logstash** processes logs, and **Kibana** visualizes security threats.
* **KQL** simplifies querying in Kibana for security monitoring.
* **SOC analysts** must be proficient in KQL and Kibana’s discovery tools to investigate incidents efficiently.
* **Elastic Stack as a SIEM** helps detect and mitigate security threats through log analysis and real-time visualization.
