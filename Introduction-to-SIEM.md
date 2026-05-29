# 🛡️ TryHackMe Lab Writeup: Introduction to SIEM

## 📌 Module Overview
* **Pathway:** SOC Level 1  
* **Module:** Core SOC Solutions  
* **Room:** Introduction to SIEM  
* **Core Objective:** Understand the foundational engineering principles of Security Information and Event Management (SIEM) systems. Analyze log source differentiation, deconstruct data normalization pipelines, and evaluate correlation models to detect and analyze network threats.

---

## 📋 Table of Contents
1. [Task 1: Introduction & High-Level Core Concepts](#-task-1-introduction--high-level-core-concepts)
2. [Task 2: Logs Everywhere, Answers Nowhere](#-task-2-logs-everywhere-answers-nowhere)
3. [Task 3: Why SIEM (Core Architecture)](#-task-3-why-siem-core-architecture)
4. [Task 4: Log Sources and Ingestion Pathways](#-task-4-log-sources-and-ingestion-pathways)
5. [Task 5: Alerting Process and Analysis](#-task-5-alerting-process-and-analysis)
6. [Task 6: Practical Lab Work & Incident Investigation](#-task-6-practical-lab-work--incident-investigation)

---

## 🔍 Task 1: Introduction & High-Level Core Concepts
In an enterprise network infrastructure, every deployed asset (endpoints, data servers, and network perimeters) continuously records state changes and user interactions via log files. Auditing these logs individually scales poorly and introduces vast visibility blind spots. A SIEM system provides a unified telemetry plane that collects, normalizes, and correlates these logs to detect malicious activities in a timely manner.

---

## 🛑 Task 2: Logs Everywhere, Answers Nowhere
Relying on decentralized local data nodes introduces severe engineering and operational challenges during a network breach.

### 1. Categorization of Log Sources
* **Host-Centric Log Sources:** Capture events occurring strictly within or related to a specific endpoint execution context (e.g., Windows Event Logs, Linux system logs, authentication requests, process execution activity, registry updates, and local PowerShell executions).
* **Network-Centric Log Sources:** Generated when hosts communicate with each other across local network segments or external gateways (e.g., firewall deny logs, IDS/IPS capture logs, active SSH/FTP connections, corporate VPN access, and shared network file directories).

### 2. Core Operational Challenges
* **Numerous Log Sources:** A typical network has countless log sources generating hundreds of events per second, resulting in scattered and unmanageable data pools.
* **No Centralization:** Because logs reside locally on the machines that generated them, analysts must establish individual remote sessions (SSH, RDP) to gather evidence, which wastes critical time during a breach.
* **Limited Context:** Individual logs cannot tell the whole story of an activity. Isolated events on different log sources may seem completely harmless, but when correlated, they reveal a targeted attack pipeline (e.g., catching a lateral movement vector following a boundary account compromise).
* **Limited Analysis Capability:** The sheer volume of incoming events makes manual review statistically impossible for humans, leading to critical indicators being completely missed.
* **Format Inconsistency:** Different log sources generate telemetry in varying syntax structures and formats, complicating cross-platform analysis.

---

## 🧠 Task 3: Why SIEM (Core Architecture)
A SIEM system efficiently mitigates data fragmentation challenges by running five core architectural pipelines:

* **Centralized Log Collection:** Collects logs from all heterogeneous sources (endpoints, firewalls, servers) into a single, centralized location using lightweight agents or native APIs.
* **Normalization of Logs:** Converts raw, unstructured text arrays into a consistent format. This involves breaking logs into specific fields (**Parsing**) and mapping different vendor naming conventions to a universal schema (**Normalization**, e.g., standardizing `src`, `source`, and `Src_IP` fields into a single token: `source_ip`).
* **Correlation of Logs:** Analyzes sequential conditions across entirely different systems within specific time windows to find relationships and uncover structured attack behaviors.
* **Real-time Alerting:** Continuously matches inbound telemetry streams against predefined detection rules to automatically flag high-severity threats for analysts to triage.
* **Dashboards and Reporting:** Aggregates processed metrics into highly summarized, near real-time graphical views—tracking actionable insights like active system health alerts, failed login attempt numbers, top domains visited, and triggered rules.

---

## 📂 Task 4: Log Sources and Ingestion Pathways

### 1. Structural Breakdown of Platform Logs
* **Windows Machine Telemetry:** Windows records events within a unified system known as the `Event Viewer`, assigning a unique and standardized **Event ID** to each type of log activity (e.g., successful login is `4624`, failed login is `4625`, and process creation is `4688`).
* **Linux System Auditing:** Linux architectures drop low-level, flat text telemetry logs directly into specific system paths:
  * `/var/log/auth.log` or `/var/log/secure`: Stores authentication and system access states.
  * `/var/log/cron`: Logs events related to automated scheduled root engine tasks.
  * `/var/log/httpd` or `/var/log/apache`: Stores inbound web server request/response errors and events.

### 2. Log Ingestion Mechanisms
* **Agent / Forwarder:** A lightweight tool (such as the Splunk Universal Forwarder) installed locally on endpoints to capture and ship live data logs directly to the central SIEM instance.
* **Syslog Protocol:** A widely used networking protocol that collects real-time streaming data from diversos systems like databases or web gateways and routes it to a centralized destination.
* **Manual Bulk Upload:** Directly ingesting offline log archives (`.csv`, `.json`, `.log`) into the system interface to conduct retrospective or forensically scoped analysis.
* **Port-Forwarding Listener:** Configuring specific network listening ports directly on the SIEM server to receive forwarded data streams pushed from perimeter devices.

---

## 🚨 Task 5: Alerting Process and Analysis
SIEM detection rules act as logical expressions configured to flag specific activities based on field-value pairs. 

### Use-Case Engineering Examples
* **Log Invalidation Detection:** Threat actors often attempt to clear tracks post-exploitation. Windows captures this action under a specific event code.  
  $$\text{If Log Source is WinEventLog AND EventID is 104} \rightarrow \text{Trigger Alert: Event Log Cleared}$$
* **Privilege Escalation Commands:** Attackers frequently execute commands like `whoami` to verify active execution context.  
  $$\text{If EventCode is 4688 AND NewProcessName contains 'whoami'} \rightarrow \text{Trigger Alert: WHOAMI Command Execution Detected}$$

When an alert lands on the monitoring dashboard, the analyst conducts an event triage pipeline to determine its classification:
* **False Positive (FP):** A benign activity or normal system noise incorrectly flagged as a threat. This classification requires rule tuning to suppress future noise.
* **True Positive (TP):** A validated threat execution. Analysts must initiate active incident containment playbooks—such as isolating infected host nodes from the internal network, stopping malicious processes, and updating edge firewalls to block the attacker's source IP addresses.

---

## 🕵️‍♂️ Task 6: Practical Lab Work & Incident Investigation
An incident triage investigation was executed on a simulated Splunk SIEM architecture to isolate and analyze an active network compromise.

### 1. Key Metric Indicators (Pre-Exploitation)
* **High-Volume Malicious Traffic:** Windows Event ID `4625` (Failed Login Attempt) accounted for **38.3%** of all inbound log entries, confirming an active brute-force campaign.
* **Primary Indicator of Compromise (IOC):** The external host IP address **`102.152.62.75`** generated **40%** of all network RDP connection attempts.

### 2. Forensic Resolution: Unauthorized Binary Isolation
* **Target Node & Compromised Identity:** An active alert was flagged on host workstation **`HR_02`** running under the active session profile assigned to user **`Chris`**.
* **Suspicious Payload Execution:** Forensic analysis of the active process events isolated an unauthorized executable running out of a user temp path:  
  `C:\Users\Chris\temp\cudominer.exe`
* **Detection Rule Evaluation:** The alert was triggered because the payload string matched a specific five-letter keyword configured in the background detection rule.
* **Rule Match Parameter:** `miner`
* **Final Verdict:** **True Positive (Cryptomining Attack).** A threat actor successfully established entry and deployed an unauthorized mining payload to steal enterprise CPU clock cycles.
