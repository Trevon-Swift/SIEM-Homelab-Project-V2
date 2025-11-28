# Enterprise Security Monitoring & Threat Detection Laboratory

![Project Status](https://img.shields.io/badge/Status-Complete-success)
![Wazuh Version](https://img.shields.io/badge/Wazuh-4.14-blue)
![Detection Rate](https://img.shields.io/badge/Detection%20Rate-100%25-brightgreen)
![MITRE Techniques](https://img.shields.io/badge/MITRE%20Techniques-7+-orange)

---

## 📋 Project Overview

This project demonstrates enterprise-grade Security Information and Event Management (SIEM) capabilities through a comprehensive homelab environment. Built from scratch using Wazuh SIEM, this lab showcases threat detection, security monitoring, log analysis, and incident response skills essential for SOC analyst and security engineering roles.

**Achievement: 100% detection rate across 121+ simulated attack events**

### Key Objectives

- ✅ Deploy and configure enterprise SIEM platform (Wazuh 4.14)
- ✅ Implement multi-platform security monitoring
- ✅ Simulate real-world attack scenarios with penetration testing tools
- ✅ Validate detection capabilities across MITRE ATT&CK framework
- ✅ Develop custom detection rules and correlation logic
- ✅ Document incident response procedures and threat hunting methodologies
- ✅ Create professional security analysis reports

---

## 🏗️ Architecture

### Infrastructure Components

```
┌─────────────────────────────────────────────────────────┐
│                    Host Machine                         │
│                  Windows + VirtualBox                   │
│                                                         │
│          ┌──────────────┐  ┌──────────────┐             │
│          │   Wazuh      │  │  Kali Linux  │             │
│          │   Manager    │◄─┤   (Agent)    │             │
│          │  (Ubuntu)    │  │              │             │
│          │  SIEM Core   │  │ Red Team     │             │ 
│          │  192.168.1   │  │ Attack       │             │ 
│          │  .102        │  │ Platform     │             │
│          └──────────────┘  └──────────────┘             │
│                 │                  │                    │
│                 │                  │                    │
│                 └──────────────────┘                    │
│                 Monitoring &                            │
│              Attack Simulation                          │
└─────────────────────────────────────────────────────────┘
```

### Virtual Machine Specifications

| VM Name | Operating System | RAM | CPUs | Disk | Role | IP Address |
|---------|-----------------|-----|------|------|------|------------|
| Wazuh-Manager | Ubuntu Server 25.10 | 4 GB | 2 | 80 GB | SIEM Manager | 192.168.1.102 |
| Kali-Linux | Kali Linux 2024.x | 4 GB | 2 | 80 GB | Attack Platform & Monitored Agent | Dynamic DHCP |

**Network Configuration:** Bridged Adapter (VMs on host network segment)

---

## 🔧 Technologies & Tools

### Core Platform
- **SIEM Solution:** Wazuh 4.14 (Open-source SIEM and XDR platform)
- **Virtualization:** Oracle VirtualBox 7.x
- **Operating Systems:** Ubuntu Server 25.10, Kali Linux 2024.x

### Security Tools & Attack Simulation
- **Penetration Testing:** Kali Linux (Nmap, Hydra, Netcat)
- **Monitoring:** Wazuh agents, File Integrity Monitoring (FIM), Authentication tracking
- **Analysis:** Wazuh dashboard, Custom detection rules, MITRE ATT&CK mapping

### Languages & Configuration
- **Scripting:** Python, Bash
- **Configuration:** XML (Wazuh rules), YAML (agent configs)
- **Documentation:** Markdown, Technical writing

---

## 🎯 Skills Demonstrated

### Technical Competencies

**SIEM Operations**
- Enterprise SIEM deployment and configuration
- Multi-platform agent management and monitoring
- Real-time log analysis and event correlation
- Alert triage and investigation workflows
- 100% detection rate across tested attack vectors

**Threat Detection Engineering**
- File Integrity Monitoring (FIM) configuration
- Authentication monitoring and brute force detection
- Privilege escalation attempt identification
- Persistence mechanism detection
- Command execution analysis

**Security Analysis**
- Attack pattern recognition across MITRE ATT&CK framework
- Log analysis and forensics
- Incident timeline reconstruction
- IOC (Indicators of Compromise) identification
- Threat intelligence application

**Red Team/Penetration Testing**
- Attack simulation using industry-standard tools
- Brute force credential attacks (Hydra)
- Reconnaissance and enumeration techniques
- Privilege escalation attempts
- Persistence mechanism establishment

**System Administration**
- Linux server deployment and hardening
- Service management and troubleshooting
- Network configuration and diagnostics
- Virtual infrastructure management

### Professional Skills

- Systematic problem-solving and troubleshooting (resolved 7 major technical issues)
- Comprehensive technical documentation (3 major documents, 121+ screenshots)
- Security operations workflows and procedures
- Incident response methodologies
- Professional reporting and communication
- MITRE ATT&CK framework application

---

## 📂 Repository Structure

```
SIEM-Homelab-Project-V2/
│
├── README.md                              # Project overview (this file)
├── Documentation/
│   ├── 04_Attack_Scenarios_and_Detection.md  # Detailed attack simulations and detections
│   ├── 05_Additional_Content.md               # Additional project content
│   ├── 06_Troubleshooting_Log.md              # Complete troubleshooting documentation
│   └── README.md                              # Documentation index
│
├── Assets/
│   ├── Screenshots/                       # All visual evidence (27 organized screenshots)
│   │   ├── 01_01_Ubuntu_VM_RAM_CPU_Allocation.png
│   │   ├── 02_01_Wazuh_Install_Script_Output.png
│   │   ├── 03_01_Agent_Deployment_Wazuh_UI_Command.png
│   │   ├── 04_01_Kali_Agent_Attack_Simulation.png
│   │   └── ... (and 23 more organized screenshots)
│   └── README.md                          # Assets documentation
│
└── Code/
    └── README.md                          # Code and configuration documentation
```

---

## 🚀 Project Phases & Achievements

### ✅ Phase 1: Foundation (Complete)
**Duration:** Days 1-3 | **Status:** Complete

**Achievements:**
- [x] Ubuntu Server 25.10 VM deployment on internal SSD
- [x] Wazuh Manager 4.14 installation and configuration
- [x] All services operational (manager, indexer, dashboard)
- [x] Web dashboard access verified from Windows host
- [x] Network connectivity validated (bridged adapter)
- [x] Comprehensive troubleshooting documentation

**Major Challenges Overcome:**
1. Storage configuration issues (external HDD → internal SSD migration)
2. Disk space allocation (50GB → 80GB expansion)
3. Network adapter conflicts (dual adapter resolution)
4. Wazuh indexer permission issues
5. Admin credential management

**Deliverable:** Operational SIEM manager ready for agent enrollment

---

### ✅ Phase 2: Agent Deployment (Complete)
**Duration:** Days 4-5 | **Status:** Complete

**Achievements:**
- [x] Kali Linux agent installation via HTTP file transfer
- [x] Agent enrolled and reporting as "Active"
- [x] Security events flowing to SIEM in real-time
- [x] Dashboard showing agent details and system information
- [x] Network troubleshooting (wget connectivity resolved)

**Technical Solutions:**
- Resolved dual network adapter configuration conflict
- Implemented alternative agent deployment via HTTP server
- Fixed wazuh-indexer service permission issues
- Established reliable VM-to-VM communication

**Deliverable:** Multi-platform monitoring infrastructure with Kali Linux reporting

---

### ✅ Phase 3: Attack Simulation & Detection (Complete)
**Duration:** Days 6-8 | **Status:** Complete

**Attack Scenarios Executed:**

**1. Authentication Brute Force Attacks**
- 100+ failed SSH login attempts via Hydra
- Non-existent user enumeration
- Rapid connection attempts detected
- **Detection Rate:** 100% (Rules 5710, 5712, 5720)

**2. File Integrity Violations**
- System file modifications (/etc/hosts, /etc/crontab)
- Backdoor script creation
- Permission change attempts on /etc/passwd
- **Detection Rate:** 100% (Rules 550, 554, 82)

**3. Privilege Escalation Attempts**
- /etc/shadow access attempts
- Failed sudo with wrong passwords
- SUID binary enumeration
- Sudoers file reconnaissance
- **Detection Rate:** 100% (Rules 5401, 5402, 5403)

**4. Persistence Mechanism Establishment**
- Malicious cron job creation
- Systemd service installation
- Startup configuration modifications
- **Detection Rate:** 100% (FIM alerts, Rules 550, 554)

**5. Command Execution & Process Monitoring**
- Reverse shell syntax attempts
- Netcat listener establishment
- Suspicious file downloads
- Bash obfuscation techniques
- **Detection Rate:** 100% (Command logging, process monitoring)

**Deliverable:** Validated SIEM detection across MITRE ATT&CK framework

---

## 📊 Detection Performance

### Overall Statistics

| Metric | Value |
|--------|-------|
| **Total Attack Events** | 121+ |
| **Events Detected** | 121+ |
| **Detection Rate** | 100% |
| **Average Detection Time** | < 30 seconds |
| **False Positives** | 0 (in controlled testing) |
| **MITRE Techniques Covered** | 7+ |
| **Alert Rules Triggered** | 15+ unique rules |

### MITRE ATT&CK Coverage

**Tactics & Techniques Detected:**

| Tactic | Technique ID | Technique Name | Detection Status |
|--------|--------------|----------------|------------------|
| Initial Access | T1078 | Valid Accounts (brute force) | ✅ Detected |
| Persistence | T1053.003 | Scheduled Task: Cron | ✅ Detected |
| Persistence | T1543.002 | Systemd Service | ✅ Detected |
| Privilege Escalation | T1548 | Abuse Elevation Control | ✅ Detected |
| Defense Evasion | T1565.001 | Data Manipulation | ✅ Detected |
| Credential Access | T1110 | Brute Force | ✅ Detected |
| Discovery | T1087 | Account Discovery | ✅ Detected |

---

## 🔗 Documentation & Evidence

All detailed documentation, troubleshooting logs, and visual evidence (screenshots) are organized in the following directories:

- **Documentation:** Detailed guides for each project phase and comprehensive attack scenario documentation.
- **Assets/Screenshots:** All 27 visual evidence files, consistently named and categorized by project phase.

---

## 🤝 Contact

**Author:** Trevon Swift  
**GitHub:** [Trevon-Swift](https://github.com/Trevon-Swift)  
**Repository:** [SIEM-Homelab-Project-V2](https://github.com/Trevon-Swift/SIEM-Homelab-Project-V2)
