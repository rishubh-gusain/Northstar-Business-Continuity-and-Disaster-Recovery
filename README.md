# North Star Analytics: Business Continuity & Disaster Recovery Framework  

This repository serves as a Technical Case Study of a resilient BCDR (Business Continuity and Disaster Recovery) architecture designed for high-availability analytics services.  The project simulates a production-grade enterprise environment focused on maintaining service continuity through site-level disruptions and hardware failures.

### Project Objectives & KPIs  

The primary goal was to transition from manual, localized backups to an automated, resilient framework with measurable recovery performance.   Recovery Time Objective (RTO): $\leq 10$ minutes   Recovery Point Objective (RPO): $\leq 5$ minutes   Availability Target: $\geq 99.9\%$ Uptime  


### System Architecture
The environment was architected within a VMware Workstation lab using a distributed multi-node approach to eliminate single points of failure.  

#### Tech Stack
- Operating System: Ubuntu Server (Web & Database nodes)   

- Web Tier: Apache 2 + PHP   

- Database Tier: MariaDB (Primary and Standby nodes)   

- HA Orchestration: Keepalived (VRRP-based failover)

#### Network Configuration
| Role | Physical IP | Virtual IP (VIP) |
| :--- | :---: | :---: |
| Primary Database (DB-1) | 192.168.1.128 | 192.168.1.100 |
| Standby Database (DB-2) | 192.168.1.129 | 192.168.1.100 |
| Web Server | 192.168.1.130 | N/A |
| Windows Client | 192.168.1.131 | N/A |

### High-Availability Logic
The architecture utilizes a Virtual IP (VIP) managed by Keepalived to ensure the web application remains reachable during node failures.   

#### Code snippet

    Client --> VIP{Virtual IP: 192.168.1.100}
    VIP -- Active Path --> DB1
    VIP -. Standby Path.-> DB2
    DB1 -- Sync/Backup --> DB2
    Web --> VIP

- Keepalived Failover: Keepalived monitors the health of the Master node. If a failure is detected, the VIP automatically migrates to the Standby node, allowing the web application to fetch data without connection string updates.   

- Data Integrity: Secure database synchronization is achieved through mysqldump and scp transfers, ensuring the standby node is prepared for restoration.

### Compliance & Standards
The design and operational procedures are aligned with international frameworks to ensure audit readiness:   

- ISO 22301: Business Continuity Management Systems   

- NIST SP 800-34 Rev 1: IT Contingency Planning   

- ITIL: Service Continuity Guidelines   

- Security: AES-256 at rest and TLS 1.2+ for data in transit.

### Repository Contents
Since the original VM files are proprietary, this repository contains the Architectural Blueprints and Operational Procedures:

- /docs/Architecture_Design.pdf: Detailed HLD (High-Level Design) and business requirements.   

- /docs/BCDR_Runbook.pdf: Step-by-step operational workflows for failover and recovery.   

- /configs/: Recreated configuration snippets (e.g., keepalived.conf) based on the project Runbook.   

- Demo_Video.md: A link to the live demonstration showing a successful failover test.

### Failover Proof of Concept (PoC)
To validate the architecture, we performed a simulated service crash:

1. Trigger: Manual stop of Keepalived on DB-1.

2. Detection: DB-2 detected the absence of VRRP heartbeats.

3. Action: DB-2 promoted itself to MASTER state and assumed the VIP.   

4. Verification: The Windows client dashboard successfully fetched data via the VIP from the new active node within the defined RTO.

### Authors
- Rozita Akbari

- Rishubh Gusain

- Hardeepsingh Saini

- Harshdeep Puri
