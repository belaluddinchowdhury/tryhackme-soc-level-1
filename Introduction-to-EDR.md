# 🛡️ TryHackMe Lab Writeup: Introduction to EDR (SOC Level 1)

## 📌 1. Module Overview & Repository Index
**Pathway:** SOC Level 1  
**Module 3:** Core SOC Solutions  
**Room 1:** Introduction to EDR  

---

### 📑 Table of Contents
1. [Task 1: Introduction & Learning Objectives](#-task-1-introduction--learning-objectives)
2. [Task 2: What is an EDR? (The Three Pillars)](#-task-2-what-is-an-edr-the-three-pillars)
3. [Task 3: Beyond the Antivirus (AV vs EDR Comparison)](#-task-3-beyond-the-antivirus-av-vs-edr-comparison)
4. [Task 4: How an EDR Works (Architecture & Ecosystem)](#-task-4-how-an-edr-works-architecture--ecosystem)
5. [Task 5: EDR Telemetry (The Endpoint Black Box)](#-task-5-edr-telemetry-the-endpoint-black-box)
6. [Task 6: Detection and Response Capabilities](#-task-6-detection-and-response-capabilities)
7. [Task 7: Practical Alert Investigation & Triage](#-task-7-practical-alert-investigation--triage)
8. [Task 8: Conclusion & Strategic Takeaways](#-task-8-conclusion--strategic-takeaways)

---

## 🎯 Task 1: Introduction & Learning Objectives
Endpoint Detection and Response (EDR) is a core security solution engineered to monitor, detect, and respond to advanced threats at the endpoint level. In modern Security Operations Centers (SOCs), understanding EDR mechanics is non-negotiable due to the rapid decentralization of corporate networks. 

### 📈 Core Learning Objectives:
* Differentiate between signature-based defenses and behavior-based monitoring.
* Master the baseline architecture of endpoint agents and central management consoles.
* Analyze telemetry streams (Process trees, registry hooks, and network sockets).
* Map triggered alerts to the MITRE ATT&CK framework.
* Conduct practical triage on simulated multi-stage intrusions.

---

## 🏛️ Task 2: What is an EDR?
The proliferation of digital assets combined with the fast adoption of **Remote Work** has exposed devices outside the traditional network perimeter protection (Firewalls, IDS/IPS). Organizations require a solution that guards host machines regardless of their geographic location.

### 🔑 The Three Pillars of EDR
1. **Visibility:** Continuous, granular tracking of system activity (registry tweaks, process creations, user actions) rendered into a structured format for analysts.
2. **Detection:** Leveraging machine learning and behavioral baselines to capture advanced threats (e.g., fileless malthreats) that bypass standard rules.
3. **Response:** Providing remote remediation vectors directly from a single centralized dashboard (host isolation, process termination, file quarantine).

**Leading Corporate EDR Solutions Evaluated:**
* CrowdStrike Falcon
* SentinelOne ActiveEDR
* Microsoft Defender for Endpoint
* OpenEDR / Symantec EDR

---

## ⚔️ Task 3: Beyond the Antivirus
While both Antivirus (AV) and EDR aim to secure endpoints, they operate at radically different levels of protection.

* **The Airport Analogy:** * **Antivirus (AV)** is the *Immigration Passport Check*. It inspects incoming entities against a pre-existing list of known bad actors (Signatures). If a threat has no criminal record (Zero-day), it passes cleanly.
  * **EDR** represents the *Internal Security Personnel and CCTV Network*. It ignores the passport and continuously monitors actions: Is an asset roaming restricted areas? Is its behavior anomalous?

### 📊 Multi-Stage Attack Lifecycle: AV vs. EDR Response

| Attack Phase | Antivirus (AV) Action | EDR Action |
| :--- | :--- | :--- |
| **1. Phishing Email Payload Download** | **Does nothing** if the file hash has no match in the local signature DB. | **Logs download activity** and initiates state monitoring on the asset. |
| **2. User Opens Word Document** | **Does nothing**; `winword.exe` is flagged as a completely legitimate utility. | **Records process instantiation** of `winword.exe` and hooks its threads. |
| **3. Malicious Macro Spawns PowerShell** | **Does nothing** if the macro script structure is unique. | **Detects and flags** the highly anomalous Parent-Child process relationship. |
| **4. Obfuscated Script Execution** | **Fails to decode** or flag complex string manipulation. | **Flags execution behavior** of the obfuscated script stream. |
| **5. Memory Injection into `svchost.exe`** | **Bypasses check** due to a lack of live memory integrity monitoring. | **Flags Process Injection** occurring inside the Windows subsystem file. |
| **6. Attacker Gains Remote Shell Access** | **Fails to detect** due to a lack of host network-level telemetry. | **Flags unexpected outbound connection** launched by `svchost.exe`. |
| **🔄 Final Outcome** | Asset marked as **Clean** (False Sense of Security). | **Generates complete Attack Chain** alert for instant triage. |

---

## ⚙️ Task 4: How an EDR Works
EDR functionality relies on a highly efficient client-server framework to transform local system telemetry into centralized actionable alerts.

* **EDR Agents / Sensors:** Lightweight programs running locally on host machines. They serve as the eyes and ears of the SOC, recording real-time operations and shipping data instantly to the cloud or central console.
* **EDR Central Console:** The analytical brain. It ingests telemetry streams from thousands of agents, correlates disparate actions using complex logic, applies machine learning algorithms, and cross-references data with Threat Intelligence feeds to trigger alerts.
* **The SIEM Ecosystem:** EDR functions alongside Firewalls, Email Security Gateways, and DLPs. Analysts commonly ingest EDR alerts into a SIEM (e.g., Splunk) to unify cross-network investigations.

---

## 📦 Task 5: EDR Telemetry
Telemetry is the definitive "black box" of an endpoint. Because advanced threat actors disguise their actions using native, legitimate binaries (**Living off the Land**), comprehensive data ingestion is required to paint an accurate forensic narrative.

### 📋 Crucial Telemetry Channels Captured:
* **Process Executions & Terminations:** Tracks PIDs and PPIDs to instantly catch suspicious child-parent handoffs.
* **Network Connections:** Monitors active sockets to identify active Command and Control (C2) communication, unauthorized lateral movement, or data staging/exfiltration.
* **Command Line Activity:** Captures the explicit string arguments executed inside shells like CMD and PowerShell, bypassing basic execution obfuscation.
* **File/Folder Modifications:** Logs dropping of second-stage payloads, staging files, or widespread encryption changes associated with ransomware.
* **Registry Modifications:** Monitors changes made to critical Windows hives, preserving visibility over configuration manipulation and system modifications.

---

## 🛠️ Task 6: Detection and Response Capabilities

### 🔍 Advanced Detection Frameworks
* **Behavioral & Anomaly Detection:** Establishes an asset baseline. Activities deviating from normal patterns (e.g., an unauthorized registry modification to an autostart key) trigger a behavioral warning regardless of signature status.
* **IOC Matching:** Integrates Threat Intelligence feeds directly. If a file hash matches a known malicious campaign indicator, it is auto-flagged.
* **MITRE ATT&CK Mapping:** Translates complex system activities into known attack stages. For example, creating an unverified scheduled task maps cleanly to:
  * *Tactic:* Persistence
  * *Technique:* Scheduled Task/Job (T1053)

### ⚡ Analytical Response Mechanisms
* **Isolate Host:** Instantly drops all network connections to the endpoint except for the EDR control channel, neutralizing lateral spread across the domain.
* **Terminate Process:** Kills the explicit Process ID (PID) running the threat, mitigating damage to high-availability production servers without taking them offline.
* **Quarantine:** Moves payloads to an isolated, encrypted directory to prevent runtime execution while allowing forensic review.
* **Remote Access (RTR):** Grants a live administrative shell to the host machine, enabling custom script execution, custom file collection, and deep-dive host forensics.

---

## 🕵️‍♂️ Task 7: Practical Alert Investigation & Triage
During live lab scenarios on a simulated environment, triage was performed on multiple high-severity incidents to map the root cause of exploitation.

### 🔍 Case Study 1: Initial Access via Malicious Office Document
* **Target Host:** `DESKTOP-HR01`
* **Analysis Narrative:** User `alice.thomas` executed a macro-enabled document titled `invoice.docm`. This application spawned an unauthorized shell instance (`CMD.exe`).
* **Technical Finding:** The command prompt bypassed signature defenses by calling a native utility, **`cURL`**, to fetch a second-stage payload file from an external server directly into the system environment.

### 🔍 Case Study 2: Data Exfiltration Attempt
* **Target Host:** `WIN-ENG-LAPTOP03`
* **Analysis Narrative:** A high-severity alert signaled an attempt to steal corporate intellectual property out of the localized network environment.
* **Technical Finding:** Reviewing the **`IOC/Indicators`** tab exposed the precise internet destination mapped under the `Domain` type column. The host was establishing connections to external storage infrastructure at **`files-wetransfer.com`** to transmit staged data.

---

## 🏁 Task 8: Conclusion
Completing this lab provides a robust foundation for host-level defensive security. Understanding the inner architecture of EDR platforms—from initial telemetry ingestion via local sensors to automated response vectors—prepares an analyst to rapidly investigate, scope, and contain complex corporate intrusions before they escalate into network-wide breaches.
