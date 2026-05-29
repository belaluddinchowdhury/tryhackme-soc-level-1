TryHackMe: Splunk - The Basics 
Room Overview
Room Name: Splunk: The Basics

Objective: Understand the core architecture of Splunk SIEM, navigate its interface, and learn how SOC analysts ingest and analyze data during log investigations.

Technical Overview & Core Architecture
Splunk is a leading Security Information and Event Management (SIEM) solution used to collect, analyze, and correlate network and machine logs in real time. It provides centralized visibility into infrastructure activities to accelerate threat detection and incident response.

Core Components
Splunk operations rely on three tier-based components working seamlessly together:

Splunk Forwarder: A lightweight agent installed on monitored endpoints (Web servers, Windows endpoints, Linux hosts, Database servers). Its sole responsibility is to collect logs using minimal resource overhead and stream them directly to the Indexer.

Splunk Indexer: The processing engine. It parses raw incoming log data, normalizes it into searchable field-value pairs, categorizes it, and saves the final output as standardized Events inside specified indices.

Search Head: The user-facing frontend interface (primarily within the Search & Reporting App). It allows security analysts to query the indexed data using SPL (Search Processing Language) and transform raw statistics into actionable tables, pie charts, and visualization dashboards.

Interface Navigation
1. Splunk Bar
Located at the very top panel of the screen, providing critical administrative options:

Messages: View system-level health notifications and alerts.

Settings: Configure Splunk instance settings such as data inputs, indexes, and user permissions.

Activity: Track the execution progress and history of current or background search jobs.

Help: Access product documentation, tutorials, and developer reference guides.

Find: A global utility bar to search across different internal objects and app modules.

The Splunk Bar allows users to switch between different installed applications instantly without returning to the main menu.

2. Apps Panel & Explore Splunk
Search & Reporting: The primary, out-of-the-box application used by SOC analysts for core log analysis and threat-hunting operations.

Explore Splunk Quicklinks: Direct interface shortcuts used to add data inputs, discover new Splunk applications, or jump straight into external reference guides.

Log Ingestion Phase (Data Sources)
According to Splunk documentation, incoming data streams are parsed, tagged with timestamps, and structured into distinctive records called Events. These inputs are categorized across major deployment boundaries:

Files and Directories: Handles the ingestion of flat log files being continuously updated by system processes or applications.

Network Events: Captures direct telemetry sent via specific network transport ports (TCP/UDP) or structured SNMP traps from routers, switches, and firewalls.

IT Operations: Collects state metrics from infrastructure orchestration and performance engines such as Nagios.

Cloud / Database Services: Supports cloud platform monitoring streams (AWS CloudTrail, Kinesis) and relational database transactional audits (SQL Server, Oracle, MySQL).

Security Services: Specialized telemetry channels from security perimeters and endpoints, including endpoint detection logs, identity providers (Active Directory), and local antivirus agents.

Windows Sources: Windows-specific architectures including classic Event Logs (Security, System, Application), PowerShell script execution blocks, and advanced system auditing tools like Sysmon.

Practical Implementation: Data Upload Workflow
To perform a manual log investigation on a standalone instance, data ingestion follows a structured 5-step workflow:

Select Source -> Select Source Type -> Input Settings -> Review -> Done

Ingestion Steps:
Select Source: Locate the targeted log file locally or via the deployment server path (e.g., VPN_logs.json located inside the TryHackMe /root/Rooms/SplunkBasic/ directory on the AttackBox).

Select Source Type: Define the structural format of the incoming logs (e.g., explicitly choosing _json for JSON data arrays or syslog for standard Linux logs) to ensure accurate parsing.

Input Settings:

Host: Define the unique host identity mapping to the events (e.g., local machine IP or target server hostname).

Index: Assign a dedicated logical database partition for the logs. Creating a unique index like vpn_logs guarantees isolation from default system indexes, accelerating query speeds.

Review: Sanity check on configuration summary matrix before final commit.

Done: Execution confirmation. The file is successfully indexed and pushed to the Search Head for real-time analysis.

Key Learning Takeaways
Everything is an Event: Regardless of the type of raw data uploaded (Firewall, Web, or VPN logs), Splunk standardizes data as individual lines of records known as Events.

Logical Index Isolation: Best security practices dictate isolating separate log structures into descriptive, custom-named indices (index="vpn_logs") rather than dumping everything into the main bucket. This directly reduces query latency and scope.

SPL Readiness: Successfully setting up clean indices lays down the immediate foundation required to build search pipelines, map fields, filter out noise, and detect adversarial behaviors on the network.
