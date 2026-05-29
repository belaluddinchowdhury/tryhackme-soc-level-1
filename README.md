# TryHackMe: SOC Level 1 Learning Path Portfolio

## Objective
This repository dedicatedly documents the practical labs, analysis workflows, and technical methodologies completed within the TryHackMe SOC Level 1 Curriculum.

---

## Technical Skills Standardized
* **SIEM Operational Triage:** Splunk
* **Traffic & Packet Analysis:** Wireshark
* **Threat Intelligence Operations:** OSINT Identification
* **Perimeter Containment:** Firewall Rule Configuration

---

## Course Curriculum & Progress Tracking

### Module 1: Blue Team Introduction
* **[Junior Security Analyst - Incident Response](./Junior-Security-Analyst-Intro.md)**
  * *Core Focus:* SIEM Alert triage, infrastructure lookup, incident escalation, and firewall mitigation.
* **[SOC Role in Blue Team](./SOC-Role-In-Blue-Team.md)**
  * *Core Focus:* Security hierarchy, Blue Team specializations, Internal SOC vs MSSP, and CISO incident routing.
* **[Humans as Attack Vectors](./Humans-as-Attack-Vectors.md)**
  * *Core Focus:* Social engineering psychology, technical mitigation vs detection, and workforce security profiling.
* **[Systems as Attack Vectors](./Systems-as-Attack-Vectors.md)**
  * *Core Focus:* Vulnerability patch tracking (CVE metrics), supply chain threats, configuration auditing, and defensive infrastructure remediation.
### Module 2: SOC Team Internals
* **[SOC L1 Alert Triage](./SOC-L1-Alert-Triage.md)**
    * *Core Focus:* SIEM Event vs Alert generation, log pipeline optimization, alert prioritization frameworks, and executing accurate defensive verdicts (True vs False Positives).
* **[SOC L1 Alert Reporting](./SOC-L1-Alert-Reporting.md)**
    * *Core Focus:* Alert reporting constraints, 5Ws triage templates, operational escalation triggers, and crisis communication protocols.
* **[SOC Workbooks and Lookups](./SOC-L1-Workbooks-Lookups.md)**
  * *Core Focus:* Operational context building using Identity and Asset inventories, internal network path reconstruction via topology diagrams, and structured Tier 1 triage playbook design.
* **[SOC Metrics and Objectives](./SOC-L1-Metrics-Objectives.md)**
  * *Core Focus:* Operational volume metrics (AC, FPR, AER, TDR), timeline triage tracking (MTTD, MTTA, MTTR) mapped to formal SLAs, and systematic false positive remediation strategies.
### Module 3: Core SOC Solutions
* **[Introduction to EDR](./Introduction-to-EDR.md)**
    * *Core Focus:* Endpoint telemetry extraction (PIDs, PPIDs, Registry Hooks), differentiating signature vs behavioral heuristic monitoring, and implementing defensive remediation strategies (Network Isolation, Process Termination).
* **[Introduction to SIEM](./Introduction-to-SIEM.md)**
  * *Core Focus:* Fundamental engineering architecture of SIEM data engines, log parsing pipelines, ingestion methodology differentiation (Syslog vs Agents), detection rule logic orchestration, and active triage of malicious execution metrics.
* **[Splunk: The Basics](./Splunk%20-%20The%20Basics.md)**
  * *Core Focus:* Splunk architecture (Forwarder, Indexer, Search Head), structured data ingestion workflow, and fundamental SPL logic for log filtering and anomaly detection.
