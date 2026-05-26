# TryHackMe: SOC Role in Blue Team — Writeup & Notes

This room covers how security teams are structured, the roles within a Blue Team, the differences between an internal SOC and an MSSP, and how a CISO handles incidents.

---

## 1. Corporate Security Hierarchy and Departments
Every company sets its security goals based on its business type (e.g., law firms focus on document privacy, factories on uptime, and hospitals on patient safety). Executives hire a **Chief Information Security Officer (CISO)** to manage the technical security teams. 

The main security departments include:
* **Red Team:** Offensive security experts and penetration testers who actively look for weaknesses and vulnerabilities in the system.
* **GRC Team (Governance, Risk, and Compliance):** Specialists who manage company policies and ensure everything complies with regulations like PCI DSS.
* **Blue Team:** Defensive security teams (including SOC analysts and engineers) who monitor the network and respond to active threats.

---

## 2. Blue Team Roles and Sub-departments
The Blue Team is responsible for defending the network. Depending on the company size, it includes several specific roles:

### Security Operations Center (SOC)
The main hub for monitoring security alerts.
* **L1 Analyst:** Junior tier. They do the initial triage of alerts and pass complex incidents to L2.
* **L2 Analyst:** Experienced tier. They investigate advanced attacks and breaches deeply.
* **SOC Engineer:** Technical experts who configure and maintain security tools like SIEM and EDR.
* **SOC Manager:** Manages the overall team and operational workflows.

### Cyber Incident Response Team (CIRT / CERT)
The "firefighters" of the security world. They are called in when a breach goes out of control and requires immediate, hands-on remediation.

### Other Specialized Roles
* **Digital Forensics Analyst:** Analyzes hard drives and memory to find out exactly how a breach happened.
* **Threat Intelligence Analyst:** Tracks threat groups, malware trends, and modern hacker tactics.
* **AppSec Engineer:** Ensures the software and apps developed by the company are secure.

---

## 3. Internal SOC vs. MSSP

| Topic | Internal SOC | MSSP (Managed Security Service Provider) |
| :--- | :--- | :--- |
| **Scope** |only protect own company's network (e.g., a bank's internal systems). | work for an outsourced agency protecting dozens of different clients. |
| **Work Pace** | Usually calmer shifts with less time pressure. | Fast-paced shift, usually starting with a long queue of urgent alerts. |
| **Tools** | work with fewer tools but must know them deeply. | handle many different security platforms and environments. |
| **Experience** | see fewer major attacks per year but understand them thoroughly. | deal with different live breaches weekly, gaining quick experience. |

---

## 4. Final Challenge: TrySecureMe CISO Simulation
In the final lab challenge, acted as the CISO of *TrySecureMe* to map seven simultaneous incidents to the correct security professionals. 

Here is how the threat vectors were successfully resolved:
1. **Firewall Brute-Force Triage:** Assigned to the **SOC L1 Analyst** to handle initial analysis.
2. **Phishing Malware Analysis:** Escalated to the **SOC L2 Analyst** for deep inspection.
3. **Ransomware Outbreak:** Dispatched to the **CERT Lead** for immediate containment and response.
4. **Credit Card Storage Audit:** Assigned to the **GRC Auditor** to check compliance with PCI DSS standards.
5. **Web Vulnerability Assessment:** Routed to the **Penetration Tester** to test the new site version.
6. **SIEM Storage Limit Issue:** Handled by the **SOC Engineer** to fix the unavailable system logs.
7. **FIN7 APT Group Analysis:** Sent to the **Threat Researcher** to break down the hacker group's tactics.

### Lab Flag/Token:
`THM{trysecureme_is_secured!}`
<img width="975" height="896" alt="image" src="https://github.com/user-attachments/assets/bfd3bdf3-bd59-4170-8d99-35b85e532faa" />

