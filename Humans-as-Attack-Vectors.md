# Humans as Attack Vectors — Writeup & Notes

This repository contains lab configuration analysis for the **Humans as Attack Vectors** This room focuses on the psychology behind social engineering, why human targets are highly valued by threat actors, and how a SOC team combines mitigation and detection strategies to shield an organization.

---

## 1. The Human Element
In security architectures, a corporate network can be heavily protected with firewalls and technical barriers. However, human users often act as the "gatekeepers." Attackers frequently target people rather than trying to crack complex software defenses because manipulating a human into granting access is significantly faster and easier.

Threat actors target different organizational roles based on the data or infrastructure they control:
* **HR Manager:** Targeted to breach credentials and extract entire employee databases for resale.
* **High-Net-Worth Individuals:** Manipulated into executing code to allow session hijacking of web banking platforms.
* **IT/Network Administrator:** Compromising their VPN or system accounts grants attackers deep access to the core corporate network.
* **Government Personnel:** Targeted to extract classified intelligence, simplifying subsequent operations.

---

## 2. Social Engineering Core Tactics
Social engineering maneuvers around technical perimeters by exploiting human psychology. To bypass logical skepticism, these schemes rely on two primary elements:
* **Trustworthiness:** The adversary creates a realistic pretext or impersonates a legitimate authority (e.g., IT Support, C-Suite executives) so the victim feels secure.
* **Emotional Triggers:** Manipulating intense responses such as fear, extreme urgency, or intense curiosity to disrupt logical critical thinking.

### Dominant Attack Vectors
1. **Phishing:** Fake urgent emails (e.g., "Account Compromised!") leading to spoofed authentication pages designed to harvest credentials.
2. **Malicious Downloads:** Disguising malware as benign utilities. Attackers amplify this via SEO Poisoning (ranking fake sites at the top of search results), malicious QR codes, and fake CAPTCHAs.
3. **Deepfakes:** Utilizing AI-generated audio and video to accurately impersonate corporate leaders or vendors, occasionally convincing financial departments to authorize fraudulent high-value wire transfers.
4. **Impersonation:** Directly calling helpdesks or employees while pretending to be corporate IT personnel to gain immediate control over an account under the guise of an "emergency repair."

---

## 3. Defense Framework: Mitigation vs. Detection
Defending human assets requires a dual-layered approach consisting of technical controls and defensive behavior:

* **Mitigation (Prevention):** Measures deployed to limit the attack surface before an incident occurs. This includes setting up automated email filtering or mandatory company-wide training.
* **Detection (Analysis):** Identifying and investigating advanced anomalies or malicious execution paths that successfully slip past prevention boundaries.

### Key Control Matrix

| Mitigation Strategy | Operational Impact |
| :--- | :--- |
| **Anti-Phishing Solutions** | Filters out malicious mail vectors before they reach user inboxes, removing the human error component. |
| **Antivirus & EDR Deployments** | Serves as a host-level safety net to catch and block execution when a user accidentally runs malicious payloads. |
| **"Trust but Verify" Protocols** | Training personnel to independently check and verify unexpected high-priority requests through separate official channels. |
| **Security Awareness Training** | Educating teams on identifying indicators of trickery and running phishing simulations to test organizational alertness. |

---

## 4. Practical Application: Security Policy Optimization
During the practical lab simulation, evaluated an insecure organizational layout, mitigated live threat scenarios for targeted employees, and enforced a standardized security baseline to secure the workforce.

### Part 1: Employees at Risk Mitigation
* **Action:** Analysed compromised threat templates and protected active workplace targets from ongoing infrastructure exposure.
* **🎯 Captured Flag:** `THM{anyone_else_at_risk?}`

### Part 2: Corporate Security Policy Selection
1. **Antivirus Solution:** Positioned host-level telemetry sensors across corporate endpoints to immediately isolate active host threats.
2. **Anti-Phishing Solution:** Deployed automated gateway filtering to actively scan, identify, and drop inbound malicious attachments and phishing links.
3. **Security Awareness Program:** Established structured, rolling security training and internal simulations to keep employees aware of evolving social engineering techniques.
4. **Access Management Policy:** Mandated that all internal IT support desks rigorously verify identities before executing password resets or account modifications.
* **🎯 Captured Flag:** `THM{human_protection_expert!}`

---

## 5. Proof of Work (Lab Completion Screenshots)

### Employees at Risk Scenario Solved
![Employees at Risk Flag](image_bed41f.png)

### Security Policy Baseline Enforced
![Security Policy Flag](image_bed6e5.png)
