# TryHackMe: SOC Level 1 — Junior Security Analyst Intro Writeup

This repository documents the foundational methodologies, daily operations, and practical lab application completed in the **Junior Security Analyst Intro** room under the TryHackMe SOC Level 1 learning path.

---

## 1. Executive Summary & Core Responsibilities
A Junior Security Analyst (SOC Level 1 Analyst) serves as the primary line of defense within a 24/7 Security Operations Center (SOC). Responsibilities include managing the continuous influx of telemetry and incoming security events to prevent organizational infrastructure from appearing in cyber threat intelligence feeds.

### Daily Operational Mandates:
* **Continuous Monitoring:** Reviewing and investigating diverse security alerts.
* **Collaboration & Alignment:** Participating in SOC brainstorming sessions and cooperating cross-functionally with internal infrastructure teams.
* **Skill Acquisition:** Constantly analyzing modern attack vectors and defensive mitigation strategies.
* **Incident Lifecycle Management:** Handling operational challenges ranging from data stealer infections and phishing campaigns to large-scale ransomware containment.

---

## 2. Theoretical Breakdown: The SOC Team Ecosystem
Security monitoring is a cooperative workflow supported by defined roles within the Security Operations Center:
* **SOC Engineers:** Manage, configure, and maintain the deployment of security tooling and log ingestion pipelines.
* **Tier 1 Analysts (Junior):** Perform initial triage, log enrichment, and alert validation.
* **Tier 2/3 Analysts (Senior):** Direct advanced forensics, hunt for hidden threats, and manage complex system breaches.
* **SOC Managers:** Maintain administrative oversight, regulatory compliance, and incident command.

---

## 3. Practical Lab Application: Incident Response Walkthrough

The following section details the objective execution of an alert triage and incident response lifecycle simulated within the integrated lab dashboard.

### Phase 1: Alert Monitoring and Triage
* **Observation:** Evaluated the centralized SIEM dashboard to categorize incoming telemetry based on risk thresholds (Critical, Medium, Low).
* **Detection:** Identified a high-severity critical alert sequences: an **Unauthorized login attempt** immediately followed by a **Successful SSH login** originating from an unverified external network. 
* **Triage Analysis:** This specific indicator confirms unauthorized infrastructure access, moving the event from a standard alert to an active security incident.

### Phase 2: Threat Intelligence & IP Investigation
* **Operational Action:** Conducted infrastructure lookup on the suspicious source IP address (`221.181.185.159`) via open-source intelligence (OSINT) and reputation platforms.
* **Findings:** Verified the adversary profile:
  * **Network Provider:** China Mobile Communications Corporation
  * **Geographic Origin:** China
  * **Classification:** Associated with active network reconnaissance (Port Scanning) and **PlugX Remote Access Trojan (RAT)** Command & Control (C2) infrastructure.

### Phase 3: Statistical Analysis and Anomaly Detection
* **Baseline Comparison:** Reviewed contextual visualization metrics including *Logins by Country* and *Alerts by Category*.
* **Analysis:** Noted a statistical anomaly in geographical traffic. Sudden spikes in authentication attempts from outside standard operating perimeters validated a distributed reconnaissance or brute-force profile.

### Phase 4: Incident Escalation and Ticketing
* **Operational Action:** Compiled all collected forensic evidence—including system timestamps, affected ports, and attacker attribution metrics—into a formal security incident ticket.
* **Escalation:** Transferred the verified breach from Tier 1 triage to the Tier 2/Tier 3 Incident Response team lead under the operational identifier **Will Griffin** for deep forensic tracking.

### Phase 5: Threat Containment & Mitigation
* **Operational Action:** Initiated active containment procedures to mitigate lateral threat movement and prevent potential data exfiltration.
* **Enforcement:** Updated the perimeter firewall configuration access control list (ACL) to block and drop all incoming and outgoing traffic tied to the malicious IP address (`221.181.185.159`).
* **Documentation:** Logged the administrative rule change with the classification: `Successful SSH login from suspicious IP`, successfully isolating the threat actor.

