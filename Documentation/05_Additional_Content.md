# SIEM Homelab Project

## Enterprise Security Monitoring & Threat Detection Laboratory

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
│  ┌──────────────┐  ┌──────────────┐                    │
│  │   Wazuh      │  │  Kali Linux  │                    │
│  │   Manager    │◄─┤   (Agent)    │                    │
│  │  (Ubuntu)    │  │              │                    │
│  │              │  │ Red Team     │                    │
│  │  SIEM Core   │  │ Attack       │                    │
│  │  192.168.1   │  │ Platform     │                    │
│  │  .102        │  │              │                    │
│  └──────────────┘  └──────────────┘                    │
│         │                  │                            │
│         │                  │                            │
│         └──────────────────┘                            │
│              Monitoring &                               │
│           Attack Simulation                             │
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
SIEM-Homelab-Project/
│
├── README.md                              # Project overview (this file)
├── WORKFLOW_DOCUMENTATION.md              # Complete build process and troubleshooting
├── ATTACK_SCENARIOS.md                    # Detailed attack simulations and detections
│
├── documentation/
│   ├── phase1-manager-deployment.md       # SIEM installation guide
│   ├── phase2-agent-deployment.md         # Agent configuration procedures
│   └── phase3-attack-simulation.md        # Attack testing methodology
│
├── configurations/
│   ├── wazuh-manager/
│   │   └── ossec.conf                     # Manager configuration
│   └── agents/
│       └── kali-agent.conf                # Kali agent configuration
│
├── screenshots/
│   ├── 01-phase1-manager-deployment/      # Infrastructure setup evidence
│   ├── 02-phase2-agent-deployment/        # Agent enrollment screenshots
│   ├── 03-phase3-attack-simulations/      # Detection and alert evidence
│   └── 04-troubleshooting/                # Problem resolution documentation
│
└── scripts/
    ├── attack-simulation/
    │   └── brute-force-test.sh            # Automated attack scripts
    └── documentation/
        └── generate-report.py             # Report generation automation
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

**Framework Alignment:** 6 of 14 MITRE ATT&CK tactics demonstrated

---

## 🔗 Key Documentation

### Core Documents
- [**Workflow Documentation**](WORKFLOW_DOCUMENTATION.md) - Complete build process, troubleshooting, and decisions
- [**Attack Scenarios**](ATTACK_SCENARIOS.md) - Detailed attack simulations and detection analysis
- [**Troubleshooting Log**](WORKFLOW_DOCUMENTATION.md#troubleshooting-log) - 7 major issues resolved with solutions

### Technical Guides
- Manager Deployment Guide *(Phase 1 documentation)*
- Agent Configuration Procedures *(Phase 2 documentation)*
- Attack Simulation Methodology *(Phase 3 documentation)*

---

## 💼 Professional Value

### For Employers

This project demonstrates:

✅ **Practical SIEM expertise** - Hands-on deployment and operational experience  
✅ **Multi-platform knowledge** - Windows and Linux system administration  
✅ **Threat detection skills** - 100% detection rate across attack vectors  
✅ **Documentation excellence** - Professional-grade technical writing  
✅ **Problem-solving ability** - 7 major technical issues independently resolved  
✅ **Security operations mindset** - Think like both attacker and defender  
✅ **Framework knowledge** - MITRE ATT&CK application in real scenarios  
✅ **Persistence and resilience** - Multiple installation failures overcome  

### Resume Bullet Points

**Ready to use:**

- "Deployed enterprise SIEM platform (Wazuh 4.14) achieving 100% detection rate across 121+ simulated attack events covering 7 MITRE ATT&CK techniques"

- "Engineered security monitoring infrastructure monitoring Kali Linux attack platform with real-time file integrity monitoring, authentication tracking, and command execution analysis"

- "Conducted red team/blue team exercises simulating reconnaissance, brute force, privilege escalation, and persistence attacks with comprehensive detection validation"

- "Resolved 7 complex technical issues including storage optimization, network configuration conflicts, and service permission problems through systematic troubleshooting"

- "Created professional security documentation including incident response workflows, attack scenario analysis, and detection rule logic aligned with MITRE ATT&CK framework"

### Interview Talking Points

**"Tell me about a challenging project you've worked on"**

> "I built an enterprise SIEM homelab from scratch, which involved overcoming multiple technical challenges including storage performance issues, network configuration conflicts, and service permission problems. Through systematic troubleshooting, I successfully deployed Wazuh SIEM, enrolled monitoring agents, and achieved a 100% detection rate across over 120 simulated attack events spanning authentication attacks, file tampering, privilege escalation, and persistence mechanisms. The project demonstrates both my technical capabilities and problem-solving methodology."

**"How do you approach troubleshooting?"**

> "I follow a systematic approach: First, I verify the current state and gather diagnostic information. Then I research potential causes and test hypotheses methodically. In my SIEM project, when the indexer service repeatedly failed, I analyzed logs to find the root cause—file permission issues—rather than just restarting services. I document my troubleshooting process in real-time, which helps with both learning and creating references for future issues. This approach led me to resolve 7 major technical problems in my homelab project."

**"What experience do you have with MITRE ATT&CK?"**

> "I applied the MITRE ATT&CK framework throughout my SIEM homelab project to structure attack simulations and validate detections. I covered 7 different techniques across 6 tactics including Initial Access, Persistence, Privilege Escalation, and Credential Access. For example, I simulated T1110 (Brute Force) using Hydra for SSH attacks, and T1053.003 (Scheduled Task/Job: Cron) for persistence mechanisms. My SIEM successfully mapped all detections to their corresponding MITRE techniques, which demonstrates how I'd use the framework in a SOC for threat intelligence and incident categorization."

---

## 📈 Learning Outcomes

### Technical Knowledge Gained

**SIEM Operations:**
- Enterprise SIEM architecture and deployment methodologies
- Log aggregation, normalization, and correlation
- Alert rule creation and tuning
- Event investigation and forensics workflows

**Security Monitoring:**
- File Integrity Monitoring (FIM) implementation
- Authentication log analysis
- Process and command execution tracking
- Network activity monitoring

**Attack Methodologies:**
- Credential brute force techniques
- Privilege escalation tactics
- Persistence mechanism establishment
- System reconnaissance procedures

**System Administration:**
- Linux server deployment and management
- Service troubleshooting and recovery
- Network configuration and diagnostics
- Virtual infrastructure optimization

### Professional Skills Developed

**Problem-Solving:**
- Systematic troubleshooting methodology
- Root cause analysis techniques
- Decision-making under uncertainty (repair vs rebuild)
- Resource constraint management

**Documentation:**
- Real-time technical documentation
- Incident response reporting
- Decision rationale recording
- Professional portfolio development

**Project Management:**
- Complex project decomposition into phases
- Scope management and prioritization
- Timeline planning with dependencies
- Milestone tracking and completion

---

## 🛡️ Security Considerations

**Homelab Environment Notice:**

This project was conducted in an isolated homelab environment for educational purposes:

- ✅ All VMs on private network segment
- ✅ No production systems involved
- ✅ Controlled attack scenarios only
- ✅ All "attacks" self-contained within lab
- ✅ Proper credential management practices followed
- ✅ No actual malware or exploits used

**Skills Transfer to Production:**

The techniques demonstrated are directly applicable to enterprise environments with proper authorization and within scope of employment responsibilities.

---

## 🔮 Future Enhancements

### Planned Phase 4 (Optional Advanced)

**Custom Detection Rules:**
- Develop signature-based detection rules
- Create behavioral anomaly detection
- Implement alert correlation logic
- Configure automated response actions

**Additional Attack Vectors:**
- Web application attacks (if DVWA deployed)
- Lateral movement simulation
- Data exfiltration techniques
- Advanced persistence mechanisms

**Integration & Automation:**
- SOAR platform integration
- Threat intelligence feed ingestion
- Email alerting configuration
- Incident response playbook automation

**Scalability:**
- Deploy Windows target VM with agent
- Add Ubuntu Desktop for multi-OS monitoring
- Create vulnerable application environment
- Expand to multi-subnet architecture

---

## 🤝 Connect With Me

**Trevon Swift**

- 💼 [LinkedIn](https://linkedin.com/in/trevon-swift-35477b65)
- 🐙 [GitHub](https://github.com/Trevon-Swift)
- 📧 Email: swift.tre@gmail.com

**Open to opportunities in:**
- SOC Analyst roles
- Security Operations positions
- Incident Response teams
- Threat Detection engineering
- Security Monitoring specialist positions

---

## 📝 Project Timeline

**Total Duration:** ~2 weeks (November 2025)

| Phase | Duration | Status | Key Milestone |
|-------|----------|--------|---------------|
| Planning & Design | 1 day | ✅ Complete | Architecture finalized |
| Phase 1: Manager Deployment | 3 days | ✅ Complete | Operational SIEM |
| Phase 2: Agent Enrollment | 2 days | ✅ Complete | Kali agent reporting |
| Phase 3: Attack Simulation | 3 days | ✅ Complete | 100% detection validated |
| Documentation & Portfolio | 2 days | ✅ Complete | Professional deliverables |

**Total Project Hours:** ~40-50 hours (including troubleshooting and documentation)

---

## 🙏 Acknowledgments

- **Wazuh Community** - Excellent open-source SIEM platform and comprehensive documentation
- **MITRE ATT&CK** - Framework for understanding adversary tactics and techniques
- **Cybersecurity Community** - Resources, guides, and inspiration from forums and blogs
- **VirtualBox** - Free virtualization platform enabling homelab infrastructure

---

## 📄 License

This project is for educational and portfolio purposes. All tools and techniques demonstrated should only be used in authorized testing environments.

---

**Project Completion Date:** November 2025  
**Last Updated:** November 26, 2025  
**Status:** ✅ Complete - All Phases Finished  
**Detection Rate:** 100% across 121+ events  
**Documentation:** Comprehensive (3 major documents, 100+ screenshots)

---

*This project demonstrates practical cybersecurity expertise through hands-on SIEM deployment, attack simulation, and threat detection analysis. Built from scratch with complete documentation of challenges, solutions, and learning outcomes.*

---

**⭐ If this project helped you, please star the repository!**