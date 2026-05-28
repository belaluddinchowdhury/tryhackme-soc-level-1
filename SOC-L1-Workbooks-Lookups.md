# Module 2: SOC Team Internals — Room 3: SOC Workbooks and Lookups

This repository documents my comprehensive technical notes, methodology breakdowns, and lab solutions for **Room 3: SOC Workbooks and Lookups** under **Module 2: SOC Team Internals** on TryHackMe. This room explores the practical implementation of enterprise identity catalogs, asset lookup databases, network mapping telemetry, and the structural engineering of investigation workbooks to streamline Tier 1 alert triage.

---

## 📌 Table of Contents
1. [Context Building: Assets & Identities](#1-context-building-assets--identities)
2. [Network Topology & Incident Path Reconstruction](#2-network-topology--incident-path-reconstruction)
3. [The Engineering of SOC Workbooks](#3-the-engineering-of-soc-workbooks)
4. [Practical Lab Walkthroughs & Flag Verification](#4-practical-lab-walkthroughs--flag-verification)
   * [Workbook 1: Email Analysis (External Script/Binary)](#workbook-1-email-analysis-external-scriptbinary)
   * [Workbook 2: PowerShell Analysis (Executable Download)](#workbook-2-powershell-analysis-executable-download)
   * [Workbook 3: Network Analysis (Internal Port Scanning)](#workbook-3-network-analysis-internal-port-scanning)
5. [Summary of Key Takeaways](#5-summary-of-key-takeaways)

---

## 1. Context Building: Assets & Identities

Raw logs lack operational meaning without immediate identity and asset cross-referencing. When an alert triggers, an analyst must look past the string matching and resolve the contextual environment of the activity.

### A. Identity Inventory
An Identity Inventory functions as the central catalogue of all corporate human entities (User Accounts) and non-human processes (Machine/Service Accounts). It contains critical parameters including privileges, active communication paths, and corporate roles.

*   **Corporate Solution Vectors:**
    *   **Active Directory / Entra ID:** The primary operational database used by SOC teams to validate account groups and current status.
    *   **SSO Providers (Okta / Google Workspace):** Cloud-native directory integrations that allow quick user searching and session auditing.
    *   **HR Systems (BambooHR / SAP):** The authoritative source for validating structural data like an employee's exact role, division, and location.

### B. Asset Inventory
An Asset Inventory (Asset Lookup) tracks every computing resource (primarily servers and workstations) within the enterprise IT perimeter.

*   **Corporate Solution Vectors:**
    *   **SIEM or EDR Agents (Elastic / CrowdStrike):** Collect continuous configuration telemetry, operating system versions, and active host bindings.
    *   **MDM Solutions (Microsoft Intune / Jamf MDM):** Dedicated infrastructure built to track corporate hardware lifecycle and enforce asset compliance.

---

## 2. Network Topology & Incident Path Reconstruction

In complex enterprise environments, resolving alerts requires evaluating raw telemetry from a structural network standpoint. 

### A. Network Diagrams
A network diagram is a visual layout detailing existing enterprise locations, allocated subnets, and the specific routing conduits that link them. For an analyst, a network diagram explains why a specific IP segment is traversing the firewall and reveals whether cross-zone traffic is explicitly unauthorized.

### B. Analytical Path Reconstruction
Consider a scenario tracking firewall translation logs over a specific timeline:
1.  **08:00 UTC:** External IP `103.61.240.174` aggressively hits the perimeter firewall on `TCP/10443`. Network mapping identifies this port as the exposed interface for `vpn.tryhackme.thm`.
2.  **08:23 UTC:** The firewall records a successful login session. NAT logs show the external IP has been translated into an internal lease address: `10.10.0.53`.
3.  **08:25 UTC:** The newly assigned IP `10.10.0.53` initiates a host discovery sweep against the Database Subnet (`172.16.15.0/24`), resulting in zero open responses due to restrictive firewall drops.
4.  **08:32 UTC:** The adversary pivots laterally, launching a scanning sequence against the internal Office Subnet (`172.16.23.0/24`) to identify a secondary endpoint.

---

## 3. The Engineering of SOC Workbooks

### A. Core Definition
A SOC Workbook (also denoted as a Playbook, Runbook, or Workflow) is a highly structured document establishing the exact modular steps an analyst must perform to investigate and remediate specific threat classes consistently.

[Incoming Alert] ──► (1. Enrichment Stage) ──► (2. Investigation Stage) ──► (3. Escalation/Closure)

Workbooks act as quality assurance buffers for Tier 1 analysts. They remove analytical guesswork, ensure strict adherence to forensic requirements, and prevent critical artifacts from being overlooked during multi-stage incident processing.

### B. Standard Three-Tier Architecture
1.  **Enrichment:** Running indicators against Threat Intelligence frameworks and cross-referencing Identity/Asset logs to pull baseline metadata.
2.  **Investigation:** Auditing log timelines and SIEM data to determine if the activity aligns with expected organizational behaviors.
3.  **Escalation / Closure:** Formally reporting validated True Positives to Tier 2 engineering teams or executing False Positive case closure.

---

## 4. Practical Lab Walkthroughs & Flag Verification

### Workbook 1: Email Analysis (External Script/Binary)
*   **Objective:** Structure an analytical runbook to safely evaluate an inbound external email carrying potentially malicious code or binary attachments.
*   **Core Triage Flow:**
    1.  **Ownership:** Take ownership of the alert, compile a list of email recipients, and identify their internal corporate roles.
    2.  **Telemetry Analysis:** Investigate the email using an EML analyzer to audit SPF/DKIM verification, raw text content, and sender domain reputation.
    3.  **Malware Deconstruction:** Execute final attachment analysis by processing binary payloads inside a secure sandbox environment or running manual code reviews for scripts.
    4.  **Evidence Collection:** Gather all atomic threat indicators, parse sandbox execution artifacts, and append EML analysis summaries to the master alert.
    5.  **Reporting:** Draft a formal investigation report detailing technical findings, targeted recipients, and core evidence.
*   **Captured Flag:** `THM{the_most_common_soc_workbook}`

---

### Workbook 2: PowerShell Analysis (Executable Download)
*   **Objective:** Formulate a structured verification playbook to handle endpoint alerts signaling a PowerShell instance downloading executable objects from external endpoints.
*   **Core Triage Flow:**
    1.  **Asset Context:** Assign the alert to yourself and pull the system context of the affected host using the Asset Inventory database.
    2.  **Indicator Enrichment:** Utilize Threat Intelligence engines to parse the external source URL and conduct static analysis on the downloaded executable payload.
    3.  **Process Tracing:** Query SIEM repositories to generate a comprehensive Win32 Process Tree and audit local interactive login timelines.
    4.  **Documentation:** Write a detailed escalation report for Tier 2 containing the compiled technical evidence, host assumptions, and SIEM search strings.
*   **Captured Flag:** `THM{be_vigilant_with_powershell}`

---

### Workbook 3: Network Analysis (Internal Port Scanning)
*   **Objective:** Model an investigation matrix to handle anomalous lateral port scanning originating from an internal host subnet.
*   **Core Triage Flow:**
    1.  **Topology Mapping:** Claim the incoming alert and pull host network parameters using the active corporate network map and asset tracking logs.
    2.  **Target Verification:** Identify the precise ports scanned by the origin host and determine the explicit business services bound to those target interfaces.
    3.  **Forensic Consolidation:** Gather the operational timeline, aggregate active attack durations, catalog targeted ports, and document the origin IP subnets.
    4.  **Escalation Execution:** Write the final alert summary report, attach all indexed network evidence, and escalate the case directly to Tier 2 for endpoint containment.
*   **Captured Flag:** `THM{asset_inventory_is_essential}`

---

## 5. Summary of Key Takeaways

*   **Context Over Logs:** A raw IP string or username means nothing without real-time enrichment via Identity and Asset lookups.
*   **Enforce Consistency:** Standardizing workflows via modular workbooks prevents operational fatigue and ensures high-quality triage execution.
*   **Drive Automation:** As triage models mature, workbooks serve as the logical structural blueprints needed to build automated SOAR playbooks.
