# TryHackMe: Systems as Attack Vectors — Writeup & Notes

This repository documents my notes, analytical takeaways, and lab completion criteria for the **Systems as Attack Vectors** room on TryHackMe. This module covers core architectural vulnerabilities, misconfigurations, supply chain compromises, and defensive systems engineering from a SOC Analyst's perspective.

---

## 1. Architectural Definition of a System
In modern enterprise environments, data assets (such as financial records, cardholder information, and corporate emails) do not exist in isolation; they reside on a infrastructure core composed of **physical servers, localized lab machines, or decentralized cloud frameworks (e.g., Microsoft 365).**

While targeting an individual user via social engineering limits an adversary's footprint to a single asset, compromising a foundational **system** allows threat actors to scale their operational impact exponentially (e.g., compromising an email server yields control over thousands of downstream mailboxes).

### Targeted Infrastructure Value Matrix

| Compromised Host System | Primary Threat Vector / Adversary Motivation |
| :--- | :--- |
| **Personal Student Endpoint** | Credential harvesting (e.g., Steam profiles) and hardware absorption into a wider botnet infrastructure. |
| **Senior IT Administrator Workstation** | Lateral movement tracking to capture root access into internal transactional banking systems. |
| **Criminal Law Firm Mail Server** | Full mailbox data extraction (data dumping) to execute targeted extortion and corporate blackmail. |
| **Industrial Control Core (ICS/OT)** | Operational shutdown via network-wide ransomware encryption to extort high-value ransoms. |
| **Government Administration Portal** | Strategic web defacement, operational subversion, and hacktivism campaigns. |

---

## 2. Core Threat Overviews & Initial Access Vectors
Adversaries exploit systems through three primary entry vectors:

### A. Human-Led Vectors
Legitimate internal operators often inadvertently trigger system exposure. This includes physical insertion of unknown, malicious peripherals (e.g., **RubberDucky USB hardware** which acts as a keystroke injection tool running malware instantaneously), downloading pirated components loaded with trojans, or implementing weak credential baselines. 
> 📊 **Metric Standard:** Passwords remain a critical single point of failure, with **81% of documented enterprise breaches** stemming from stolen, leaked, or structurally weak credentials.

### B. Technical Vulnerabilities (CVEs)
Software architectures naturally contain systemic code flaws or bugs. When a software vulnerability is publicly disclosed, it is cataloged with a **Common Vulnerabilities and Exposures (CVE)** tracking designator. This initiates a race condition between threat groups building weaponized exploits and defense teams reinforcing assets.


Key historical and emerging system exploits analyzed include:
* **EternalBlue (CVE-2017-0144):** A critical Microsoft SMB flaw heavily leveraged to scale network-wide ransomware campaigns.
* **PrintNightmare (CVE-2021-34527):** A high-severity flaw residing within the Windows Print Spooler execution loop.
* **Follina (CVE-2022-30190):** A dangerous Remote Code Execution (RCE) execution vector within Microsoft Office frameworks.

### C. Supply Chain Attacks
Modern software relies heavily on layered third-party dependencies and shared code libraries. If an adversary compromises a foundational library used upstream, they can push malicious components disguised as authentic vendor updates directly to thousands of untargeted downstream users simultaneously.
* *Enterprise Examples:* The historic **SolarWinds** and **3CX** infrastructure breaches.
* *Internal Case:* Even **TryHackMe** experienced a supply chain compromise via the **Lottie Player** library utilized for rendering platform web animations.

---

## 3. Configuration Security: Fighting Misconfigurations
A misconfiguration represents an operational oversight in how a secure platform is deployed or configured by internal IT teams, rather than a code-level software flaw.

[Secure SQL Database Implemented]
└── ──► [Sensitive Customer Data Maintained]
└── ──► [CRITICAL ERROR: Password set to default "1111"]
└── ──► [CRITICAL ERROR: Firewall Disabled for Ease-of-Access]
└── ──► [Result: Threat Actors Intercept & Exfiltrate DB]

### Proactive Remediation Measures
1. **Penetration Testing:** Authorized, offensive security simulations designed to aggressively locate and exploit weaknesses before unauthorized actors do.
2. **Vulnerability Scanning:** Automated, recurring infrastructure sweeps to detect default administrative accounts, exposed ports, or unpatched elements.
3. **Configuration Audits:** Rigorous manual reviews of active machine configurations against international security hardening benchmarks, such as the **CIS Benchmarks**.

---

## 4. Practical Application: System Hardening & Controls Enforced
Operating within the TryHackMe Security Dashboard, I performed active triage on high-risk system architectures and established an enterprise remediation strategy.

### Part 1: Systems at Risk Analysis
* **Action:** Investigated active system alarms, intercepted technical attack vectors, and isolated critical host infrastructure to block unauthorized lateral movement.
* **🎯 Captured Flag:** `THM{patch_or_reconfigure?}`

### Part 2: Corporate Remediation Plan Formulation
1. **Security Training for IT:** Deployed recurring technical training modules targeting configuration lifecycles to stop default profile implementation mistakes.
2. **Patch Management:** Established a strict baseline routine for tracking, verifying, and testing security patches to eliminate high-risk CVE windows.
3. **Antivirus Protection:** Multiplied system detection layers by deploying unified endpoint antivirus agents on all primary corporate server assets.
4. **Secure Password Policy:** Enforced strong, randomly autogenerated password rules globally for service accounts and root administrative domains.
* **🎯 Captured Flag:** `THM{best_systems_defender!}`

---

## 5. Lab Verification (Screenshots)

### Systems at Risk Triage Completed
<img width="826" height="459" alt="image" src="https://github.com/user-attachments/assets/df310f7d-ecf6-4464-8f40-f1dbb079d446" />

### Corporate Remediation Policy Standardized
<img width="826" height="459" alt="image" src="https://github.com/user-attachments/assets/64a7a651-8797-4e83-99cd-8d22209cfaa8" />
