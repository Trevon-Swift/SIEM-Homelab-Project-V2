# Configuration Files & Commands

This directory contains Wazuh configuration files and a reference of commands used during the SIEM homelab project.

---

## 📁 Directory Contents

```
configurations/
├── README.md              # This file
├── commands-used.md       # Commands executed during project
├── kali-agent.conf        # Kali Linux agent configuration
└── ossec.conf             # Wazuh Manager configuration
```

---

## 📄 Configuration Files

### `ossec.conf`
**Purpose:** Main Wazuh Manager configuration file

**Location on Manager:** `/var/ossec/etc/ossec.conf`

**Key Configuration Highlights:**
- **Alert Level:** 3 (medium and above logged)
- **Remote Connection:** Port 1514, TCP protocol
- **File Integrity Monitoring (FIM):**
  - Scan frequency: 1800 seconds (30 minutes)
  - Real-time monitoring enabled
  - Monitored directories: `/etc`, `/usr/bin`, `/usr/sbin`, `/bin`, `/sbin`, `/boot`
- **Log Sources:** Authentication logs, system logs, kernel logs
- **Active Response:** Disabled for homelab testing

**Note:** This is a simplified version highlighting the key settings used in the homelab. Production deployments would include additional security hardening and custom rules.

---

### `kali-agent.conf`
**Purpose:** Wazuh agent configuration for Kali Linux endpoint

**Location on Kali:** `/var/ossec/etc/ossec.conf`

**Key Configuration Highlights:**
- **Manager Connection:** 192.168.1.102:1514 (TCP)
- **FIM Monitoring:**
  - Critical system directories: `/etc`, `/home`
  - Real-time monitoring on `/etc`
  - 30-minute scan cycle
- **Log Forwarding:** Authentication logs, system logs
- **System Inventory:** Enabled (hardware, OS, network, processes)

**Purpose in Lab:** Monitors the Kali attack platform to demonstrate SIEM detection of suspicious activities performed on the agent itself.

---

## 💻 Manual Command Execution

### `commands-used.md`
**Purpose:** Documentation of actual commands executed during the project

**Why Manual Commands Instead of Scripts:**

This project demonstrates **hands-on command-line proficiency** rather than just automation scripting. All attack simulations, agent deployments, and system modifications were performed using manual command execution and industry-standard tools.

**This approach demonstrates:**
- ✅ Deep understanding of command syntax and parameters
- ✅ Ability to troubleshoot and adapt in real-time
- ✅ Practical security tool usage (Hydra, Nmap)
- ✅ Real-world SOC analyst workflow (manual investigation/execution)
- ✅ Command-line expertise across Linux environments

**Tools Used:**
- **Hydra** - Password attack simulation
- **Nmap** - Network reconnaissance
- **Standard Linux utilities** - File manipulation, system modification
- **Manual SSH attempts** - Authentication attack testing

---

## 🔧 Implementation Notes

### Deployment Process

**Manager Deployment:**
1. Downloaded Wazuh installer script
2. Executed all-in-one installation: `sudo bash ./wazuh-install.sh -a`
3. Configured services and verified functionality
4. Accessed web dashboard for management

**Agent Deployment:**
1. Downloaded agent package via HTTP file server (network troubleshooting solution)
2. Installed with environment variables specifying manager IP and agent name
3. Started and verified agent service
4. Confirmed enrollment in dashboard

**Attack Simulations:**
- Executed manually using Hydra for brute force testing
- Used standard Linux commands for FIM testing
- Performed privilege escalation attempts with sudo
- Created persistence mechanisms via cron and systemd

### Configuration Management

**Manager Configuration:**
- Default Wazuh 4.14 configuration with minor adjustments
- FIM scan frequency reduced to 30 minutes for faster testing
- Active response disabled to allow full attack testing without automatic blocking

**Agent Configuration:**
- Standard agent setup connecting to local manager
- Enhanced FIM monitoring for attack detection
- Log forwarding for authentication and system events

---

## 🎯 Detection Capabilities Validated

Through these configurations, the following detections were validated:

**Authentication Attacks:**
- ✅ Failed login attempts (Rules 5710, 5712)
- ✅ Brute force pattern detection (Rule 5720)
- ✅ SSH authentication failures (Rule 5760)
- ✅ Failed sudo attempts (Rules 5401, 5402)

**File Integrity Violations:**
- ✅ File modifications (Rule 550)
- ✅ New file creation in monitored directories (Rule 554)
- ✅ Permission changes on system files (Rule 82)

**Persistence Mechanisms:**
- ✅ Crontab modifications detected
- ✅ Systemd service creation in monitored paths
- ✅ Startup configuration changes

**System Activity:**
- ✅ Sudo command execution monitoring
- ✅ Network connection tracking
- ✅ Process execution logging
- ✅ System inventory changes

---

## 📊 MITRE ATT&CK Coverage

Configurations enabled detection of:
- **T1110** - Brute Force (Credential Access)
- **T1053.003** - Scheduled Task/Job: Cron (Persistence)
- **T1543.002** - Create or Modify System Process: Systemd Service (Persistence)
- **T1548** - Abuse Elevation Control Mechanism (Privilege Escalation)
- **T1565.001** - Data Manipulation: Stored Data Manipulation (Impact)

---

## 💡 Key Takeaways

**Configuration Approach:**
- Started with default Wazuh settings (industry best practice)
- Made minimal, targeted adjustments for homelab testing
- Prioritized detection coverage over complex customization
- Validated detection capabilities through real attack simulation

**Learning Outcomes:**
- Understanding of SIEM configuration parameters
- Knowledge of FIM monitoring best practices
- Experience with agent enrollment and management
- Practical application of detection rules and alert correlation

**Professional Value:**
- Demonstrates ability to configure enterprise SIEM platforms
- Shows understanding of detection engineering concepts
- Proves hands-on security monitoring experience
- Validates real-world attack detection capabilities

---

## 📝 Usage Notes

**For Replication:**
1. These configs reflect a homelab environment (not production-hardened)
2. Update IP addresses to match your environment (192.168.1.102 → your IP)
3. Adjust FIM scan frequency based on performance needs
4. Enable active response in production environments
5. Add custom rules based on specific detection requirements

**For Learning:**
- Study the FIM directory selections (what/why monitored)
- Review alert level thresholds and their implications
- Understand agent-to-manager communication flow
- Examine log sources and their detection value

---

**Project:** SIEM Homelab  
**Author:** Trevon Swift  
**SIEM Platform:** Wazuh 4.14  
**Configuration Date:** November 2025  
**Approach:** Manual execution, industry-standard tools, hands-on learning
