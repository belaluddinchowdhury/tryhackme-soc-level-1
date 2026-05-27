# Module 2: SOC L1 Alert Triage — Room 2: SOC L1 Alert Reporting

This repository contains my structured technical documentation, analytical breakdowns, and lab deliverables for **Room 2: SOC L1 Alert Reporting** under **Module 2: SOC L1 Alert Triage** on TryHackMe. This section covers the optimization of the alert lifecycle through context-rich reporting, escalation procedures, and operational crisis communication playbooks.

---

## 📌 Table of Contents
1. [The Operational Funnel & Tiered Architecture](#1-the-operational-funnel--tiered-architecture)
2. [Incident Reporting Framework (The 5Ws Approach)](#2-incident-reporting-framework-the-5ws-approach)
3. [Enterprise Crisis Communication Matrix](#3-enterprise-crisis-communication-matrix)
4. [Practical Lab Walkthroughs & Artifact Verification](#4-practical-lab-walkthroughs--artifact-verification)
   * [Case Study 1: Phishing Email Delivered](#case-study-1-phishing-email-delivered)
   * [Case Study 2: Spike of Domain Discovery Commands](#case-study-2-spike-of-domain-discovery-commands)
5. [Summary of Key Takeaways](#5-summary-of-key-takeaways)

---

## 1. The Operational Funnel & Tiered Architecture

Data reduction is a primary metric within enterprise Security Operations Centers. Tier 1 analysts establish the initial perimeter by parsing raw streams to eliminate noise and structural false positives.

[Telemetry / Incoming Alarms] ──► (L1 Triage & Noise Suppression) ──► [True Positives] ──► (Escalation Actions) ──► [L2 Containment]

* **Tier 1 Analyst (Triage):** Triages real-time alarms within SIEM, EDR, or ticket management queues. Responsible for verifying artifact validity, checking internal asset parameters, closing False Positives, and escalating critical telemetry.
* **Tier 2 Analyst (Remediation):** Validates L1 triage findings and initiates localized mitigation protocols, including host network isolation, credential modification, and target malicious process termination.
* **DFIR Specialist (Incident Response):** Deploys advanced disk/memory forensics, reverse-engineers targeted binaries, establishes root-cause timelines, and coordinates legal compliance workflows.

---

## 2. Incident Reporting Framework (The 5Ws Approach)

Comprehensive incident documentation minimizes the Mean Time to Respond (MTTR). Rather than submitting ambiguous notes, every True Positive escalation entry must clearly define the following technical context:

* **Who:** The specific local/domain user account, process privilege context, or source actor identity.
* **What:** The exact malicious payload string, command sequence, or suspicious binary configuration executed.
* **When:** Precise Universal Time Coordinated (UTC) boundary timestamps mapping the start and conclusion of the event.
* **Where:** The affected network environment, host infrastructure IDs, source/destination IP addresses, or targeted application URIs.
* **Why:** The precise analytical reasoning, signature match, or corporate policy violation justifying a **True Positive** verdict.

---

## 3. Enterprise Crisis Communication Matrix

Operational environments introduce unexpected workflow disruptions. The following matrix details the standardized playbooks for maintaining communication continuity during major service or personnel exceptions:

| Unexpected Scenario | Standard SOC Playbook Procedure / Action |
| :--- | :--- |
| **L2 Analyst Unresponsive (30+ Mins)** | Maintain emergency chain-of-command. Attempt phone verification to L2 ──► Reassign to L3 ──► Escalate directly to SOC Manager. |
| **Communication Platform Breach (Slack/Teams)** | **Never** use the compromised application to verify a login or breach with a user. Implement out-of-band contact (Phone call/In-person). |
| **Massive Queue Backlog (Alert Fatigue)** | Strictly prioritize based on asset criticality matrices; immediately notify the on-shift L2 Lead regarding queue flooding. |
| **Discovered Historical Misclassification** | Do not cover up mistakes. Notify Tier 2 immediately with the Ticket ID. Attackers often maintain silent persistence for weeks prior to impact. |
| **Malformed/Unparsed SIEM Telemetry** | Do not skip the alert. Manually extract raw artifact strings to document what is visible, then log an infrastructure bug with a SIEM Engineer. |

---

## 4. Practical Lab Walkthroughs & Artifact Verification

### Case Study 1: Phishing Email Delivered
* **Alert Taxonomy:** Phishing Email Delivered
* **Timestamp:** Mar 21st, 2025 at 19:25 UTC
* **Telemetry Analysis:** Inbound mail spoofing corporate identity from an unauthorized external sender. Cryptographic validation audits confirmed explicit failure: **SPF: Fail; DKIM: Fail**. The social engineering vector utilized a "Microsoft Teams Pricing Increase" theme targeting the IT Manager's mailbox.

#### Analyst 5Ws Report:
```text
A phishing email spoofing 'Microsoft Support' was successfully delivered to IT Manager Eddie Huffman's mailbox (e.huffman@tryhackme.thm) on Mar 21st, 2025 at 19:25. The email contains a highly suspicious attachment named 'REPORT.rar' under the subject of a Microsoft Teams pricing increase. Security audits confirmed identity spoofing with SPF and DKIM failures. Marked as True Positive due to verified credential/malware delivery indicators. Escalating to L2 for endpoint isolation and attachment analysis.

Captured Flag: THM{nice_attempt_faking_microsoft_support}

Case Study 2: Spike of Domain Discovery Commands
Alert Taxonomy: Spike of Domain Discovery Commands

Target Host: DMZ-MSEXCHANGE-2013 (Windows Server 2012 R2)

Telemetry Analysis: Audit log expansion revealed that the local IIS Web Server worker process (w3wp.exe) abnormally spawned an interactive reverse shell (revshell.exe). Operating under the highest privileges (NT AUTHORITY\SYSTEM), the shell initiated an aggressive domain enumeration sequence.

Executed Command Vectors: dir, hostname, whoami /priv, net group "Domain Admins" /domain, nltest /dclist:tryhackme.thm

At 19:56 UTC, an active internal reconnaissance attempt was detected on host DMZ-MSEXCHANGE-2013 running Windows Server 2012 R2. The compromised user account NT AUTHORITY\SYSTEM executed a spike of domain discovery commands, including net group "Domain Admins" and nltest /dclist, generated via a malicious parent process revshell.exe spawned by w3wp.exe. Marked as True Positive due to confirmed interactive reverse shell activity and unauthorized network mapping. Escalating to L2 (E.Fleming) for immediate endpoint isolation, credential containment, and incident response deployment.
