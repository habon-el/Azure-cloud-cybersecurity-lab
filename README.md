# Azure Cloud Cybersecurity SIEM Lab

![Azure](https://img.shields.io/badge/Microsoft_Azure-0089D6?style=for-the-badge&logo=microsoft-azure&logoColor=white)
![Elastic](https://img.shields.io/badge/Elastic_Stack-005571?style=for-the-badge&logo=elastic&logoColor=white)
![Kali](https://img.shields.io/badge/Kali_Linux-557C94?style=for-the-badge&logo=kali-linux&logoColor=white)
![Sentinel](https://img.shields.io/badge/Microsoft_Sentinel-0078D4?style=for-the-badge&logo=microsoft&logoColor=white)

## Project Overview

A fully functional enterprise-grade Cloud Cybersecurity lab built on Microsoft Azure. This project demonstrates end-to-end security operations including infrastructure deployment, SIEM configuration, attack simulation, and incident investigation — skills directly applicable to SOC Analyst, Cloud Security Engineer, and DevSecOps roles.

---
# Architecture
Internet

│

▼

[JumpBox VM] ──── SSH Key Only (Port 22)

│

├──► [Web-1 VM] ── DVWA (Docker) ── Filebeat ──►

│                                                  │

├──► [Web-2 VM] ── DVWA (Docker) ── Filebeat ──► [ELK Server]

│                                                  │

└──► [ELK-Server VM] ◄──────────────────────────►  Kibana :5601
Azure VNet: 10.0.0.0/16

Management Subnet: 10.0.0.0/24 (JumpBox)

Private Subnet: 10.0.1.0/24 (Web-1, Web-2, ELK)


---

## Technologies Used

| Category | Technology |
|----------|-----------|
| Cloud Platform | Microsoft Azure |
| SIEM (Self-hosted) | ELK Stack (Elasticsearch 8.x, Kibana, Filebeat) |
| SIEM (Cloud) | Microsoft Sentinel + Defender XDR |
| Vulnerable App | DVWA (Damn Vulnerable Web Application) |
| Containerization | Docker |
| Automation | Ansible |
| Attack Tools | Nmap, Nikto, Hydra |
| OS | Ubuntu 24.04, Kali Linux |
| Language | Bash, YAML, KQL |

---

## Lab Steps Completed

### Step 1 — Azure Infrastructure
- Created Resource Group, Virtual Network, and 2 Subnets
- Deployed 4 Ubuntu VMs: JumpBox, Web-1, Web-2, ELK-Server
- Configured Network Security Groups (NSGs) with deny-by-default rules
- Restricted SSH access to JumpBox only from trusted IP

### Step 2 — JumpBox Configuration
- Configured SSH key-based authentication (no password auth)
- Set up JumpBox as sole entry point to private subnet
- Configured Ansible on JumpBox for automated VM management

### Step 3 — DVWA Deployment (Web-1 & Web-2)
- Deployed Docker containers running DVWA on both web servers
- Used Ansible playbook to automate Docker + DVWA installation
- Verified DVWA accessible on port 80 on both servers

### Step 4 — ELK Stack SIEM
- Installed and configured Elasticsearch 8.x with TLS security
- Installed and configured Kibana with enrollment token authentication
- Deployed Filebeat agents on Web-1 and Web-2
- Enabled system, auth, apache, and container log modules
- Configured SSH tunnel for secure Kibana access

### Step 5 — Microsoft Sentinel
- Created Log Analytics Workspace (CyberLab-Workspace)
- Enabled Microsoft Sentinel on workspace
- Connected workspace to Microsoft Defender XDR portal
- Configured Azure Activity data connector

### Step 6 — Attack Simulations
- Nmap port scan on Web-1 and Web-2
- Nikto web vulnerability scan (11 vulnerabilities found per server)
- Hydra HTTP brute force against DVWA login page
- Directory traversal attempts captured in ELK logs

### Step 7 — Incident Investigation
- Detected coordinated attack spike (1,100 events in 5 seconds)
- Identified attacker IP: 10.0.0.4 using Kibana container logs
- Confirmed attack tool: Nikto/2.1.5 via User-Agent in logs
- Verified no breach: all sensitive file requests returned 404/AH00126
- Blocked attacker IP in NSG (web-1-nsg and web-2-nsg)
- Wrote professional incident report (INC-2026-001)

### Step 8 — Kibana Dashboards
- Built SOC Overview Dashboard (bar chart, pie chart, metric counter, event table)
- Mapped web-1 vs web-2 traffic split (52.99% vs 47.01%)
- Identified attack spikes visually on timeline

---

## Key Security Concepts Demonstrated

- **Zero Trust Architecture** — deny by default, verify explicitly
- **Defence in Depth** — JumpBox + NSG + SIEM + Sentinel layered protection
- **SIEM Monitoring** — dual SIEM approach (ELK + Sentinel)
- **Incident Response** — PICERL framework applied
- **MITRE ATT&CK Mapping** — T1595, T1190, T1110, T1083
- **Log Analysis** — KQL queries in Kibana for threat hunting
- **Attack Detection** — identifying Nikto, Hydra, Nmap in logs

---

## Incident Report

A professional SOC incident report was written for the simulated attack:

- **Incident ID:** INC-2026-001
- **Severity:** MEDIUM
- **Status:** RESOLVED
- **Attacker IP:** 10.0.0.4
- **Tools Used:** Nikto 2.1.5, Hydra 9.5, Nmap
- **Breach:** NO — all sensitive file access blocked
- **Action:** Attacker IP blocked via Azure NSG rules

---

## Skills Demonstrated

- Azure cloud infrastructure deployment
- Network security (VNet, NSG, subnets, JumpBox pattern)
- ELK Stack SIEM deployment and configuration
- Microsoft Sentinel and Defender XDR setup
- Log analysis and threat hunting with KQL
- Attack simulation (ethical hacking tools)
- SOC incident investigation workflow
- Professional incident report writing
- Dashboard creation for security monitoring

---

## Author

**Axcel Habon**  
Cybersecurity | Cloud Security | SOC Operations  
GitHub: [@habon-el](https://github.com/habon-el)
