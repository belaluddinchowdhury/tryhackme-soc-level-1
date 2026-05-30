TryHackMe: Elastic Stack: The Basics – Walkthrough & Lab Report
Executive Summary
This repository contains the technical documentation and analytical walkthrough for the Elastic Stack: The Basics room on TryHackMe. The primary objective of this lab is to investigate an organization's VPN logs using the Elastic Stack (ELK) to identify operational anomalies, filter unauthorized access patterns, and establish a centralized Security Operations Center (SOC) dashboard.

Technical Overview: The ELK Stack
While the Elastic Stack is traditionally a log management and analytics suite, its rapid querying engine makes it a formidable solution for modern SOC deployments acting as a SIEM.

Core Components Archetype
Elasticsearch: A distributed, JSON-based full-text search and analytics engine used to index and correlate log data.

Logstash: A server-side data processing pipeline that ingests data from multiple concurrent sources, parses/normalizes the payloads, and ships them to the designated storage cluster.

Beats: Lightweight, single-purpose data shippers installed directly on endpoints to forward specific telemetry (e.g., Filebeat, Winlogbeat) to Elasticsearch or Logstash.

Kibana: The frontend visualization interface utilized by security analysts to query data streams, conduct threat hunting, and design operational dashboards.

Phase 1: Operational Ingestion & Investigation (Discover Tab)
The ingestion profile focuses on the vpn_connections index pattern containing enterprise VPN access logs.

Analysis & Timeline Anomalies
Upon normalizing the dataset, an initial analysis of the timeline volume revealed a significant anomaly:

Observation: A massive vertical spike in connection logs was isolated on January 11, 2022, indicating a potential brute-force or unauthorized scripted access campaign.

Phase 2: Threat Hunting with Kibana Query Language (KQL)
To drill down into the anomalies, specific KQL syntax structures were leveraged to filter noise from raw event streams.

KQL Reference Patterns Implemented
Free Text Filtering:

"United States" AND "Virginia"
Isolates all connection logs originating specifically from corporate infrastructures or vectors within Virginia, USA.

Exclusion Logic:

"United States" AND NOT ("Florida")
Filters the dataset to focus on nationwide activity while suppressing known noise from the Florida branch.

Targeted Field Inquiries:

Source_ip : 238.163.231.224 AND UserName : Suleman
Directly correlates a specific suspicious IP address with the targeted corporate username to check for account takeover attempts.

Phase 3: Visualizations & SOC Dashboard Architecture
Metric Isolation: Failed VPN Attempts
To capture brute-force patterns, an index-wide filter was introduced to isolate failed authentication sequences:

action : "failed"
Temporal Boundaries: Setting the absolute time anchor between Jan 1, 2022 and Feb 1, 2022 isolated a total volume of 274 failed connection attempts occurring entirely within January.

Attacker Profiling: Sorting the aggregated data by record counts in descending order identified user Simon as the primary target of the authentication failures, originating systematically from the malicious node 172.201.60.191.

Unified Dashboard Composition (VPN_Activity)
To transition from tactical queries to persistent monitoring, a centralized dashboard named VPN_Activity was compiled by adding individual lenses from the Kibana library:

Failed Attempts Table: Displays compromised account names alongside their corresponding hostile Source IPs and attempt weights.

Top 5 VPN Users Pie Chart: Breaks down corporate bandwidth consumption, highlighting statistical outliers.

IP Address Volume Metric: Monitors baseline thresholds for incoming connections.

Raw Stream Table View: Maintains a filtered, live-updating ledger of the vpn_connections index pattern.
