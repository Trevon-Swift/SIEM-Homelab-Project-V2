# Code and Configuration Files

This directory contains all configuration files, scripts, and code snippets used in the SIEM Homelab Project.

---

## 🔧 Configuration Files

### Wazuh Manager (`wazuh-manager/`)

**`ossec.conf`** - Main Wazuh manager configuration
- Global settings and alert levels
- Remote connection configuration (port 1514)
- File Integrity Monitoring (FIM) settings
- Rootcheck system audit configuration
- Log analysis sources
- Active response rules (disabled for testing)
- Vulnerability detection settings

**Key Configuration Highlights:**
- **Alert Level:** 3 (medium and above)
- **FIM Frequency:** 1800 seconds (30 minutes)
- **Monitored Directories:** `/etc`, `/usr/bin`, `/usr/sbin`, `/bin`, `/sbin`, `/boot`
- **Active Response:** Disabled for homelab testing

### Agents (`agents/`)

**`kali-agent.conf`** - Kali Linux agent configuration
- Manager connection settings (192.168.1.102:1514)
- File Integrity Monitoring for attack platform
- Log collection from auth.log, syslog, kern.log
- Command monitoring (netstat, df, last)
- System inventory collection (syscollector)

**Monitored on Kali:**
- Critical system directories
- Home directories
- Systemd services (persistence detection)
- Cron jobs (persistence detection)
- Authentication logs
- Sudo usage logs

---

## 📜 Scripts

### Attack Simulation (`scripts/attack-simulation/`)

Scripts for testing SIEM detection capabilities through controlled attack simulations.

#### `brute-force-ssh.sh`
**Purpose:** Simulate SSH brute force attack

**Usage:**
```bash
sudo ./brute-force-ssh.sh [target] [attempts] [delay]

# Examples:
sudo ./brute-force-ssh.sh 127.0.0.1 20 1
sudo ./brute-force-ssh.sh localhost 50 2
```

**Parameters:**
- `target`: IP or hostname (default: 127.0.0.1)
- `attempts`: Number of login attempts (default: 20)
- `delay`: Seconds between attempts (default: 1)

**Detection Rules Triggered:**
- Rule 5710: Attempt to login using non-existent user
- Rule 5712: Multiple authentication failures
- Rule 5720: Multiple failed login attempts (correlation)

**MITRE ATT&CK:** T1110 - Brute Force

---

#### `fim-test.sh`
**Purpose:** Test File Integrity Monitoring detection

**Usage:**
```bash
sudo ./fim-test.sh
```

**Tests Performed:**
1. Modify `/etc/hosts` (Rule 550: Integrity checksum changed)
2. Create suspicious file `/etc/backdoor_test.sh` (Rule 554: File added)
3. Modify `/etc/crontab` (Rule 550: Persistence attempt)
4. Change `/etc/passwd` permissions (Rule 82: Permission change)
5. Create systemd service (Rule 554: Persistence mechanism)

**Cleanup:** Script offers automatic cleanup of test artifacts

**Detection Rules Triggered:**
- Rule 550: Integrity checksum changed
- Rule 554: File added to the system
- Rule 82: System file permission change

**MITRE ATT&CK:** 
- T1053.003 - Scheduled Task/Job: Cron
- T1543.002 - Create or Modify System Process: Systemd Service
- T1565.001 - Data Manipulation: Stored Data Manipulation

---

### Setup Scripts (`scripts/setup/`)

#### `deploy-agent.sh`
**Purpose:** Automate Wazuh agent installation

**Usage:**
```bash
sudo ./deploy-agent.sh
```

**Interactive prompts for:**
- Wazuh Manager IP address
- Agent name (defaults to hostname)
- Installation confirmation

**Features:**
- Checks for existing agent installation
- Downloads agent package (tries multiple sources)
- Configures manager connection
- Starts and verifies agent service
- Displays connection status and logs

**Requirements:**
- Root/sudo access
- Network connectivity to Wazuh manager or internet
- Debian/Ubuntu-based system (Kali Linux, Ubuntu)

---

## 🚀 Quick Start

### 1. Set Up Wazuh Manager

```bash
# On Ubuntu Server VM
curl -sO https://packages.wazuh.com/4.14/wazuh-install.sh
sudo bash ./wazuh-install.sh -a

# Save admin credentials displayed
# Note Manager IP address
```

### 2. Deploy Agent on Kali

```bash
# Copy deploy-agent.sh to Kali
# Make executable
chmod +x deploy-agent.sh

# Run installer
sudo ./deploy-agent.sh

# Enter Manager IP when prompted
# Verify agent shows as "Active" in dashboard
```

### 3. Run Attack Simulations

```bash
# Test brute force detection
sudo ./brute-force-ssh.sh 127.0.0.1 20 1

# Wait 1-2 minutes
# Check dashboard: Threat Hunting → Events → Search "authentication"

# Test FIM detection
sudo ./fim-test.sh

# Wait 30 minutes for FIM scan cycle
# Check dashboard: Search "integrity" or "syscheck"
```

---

## ⚠️ Important Notes

### Security Warnings

1. **Lab Environment Only:** All attack simulation scripts are for authorized testing in controlled homelab environments only
2. **No Production Use:** Never run these scripts against production systems
3. **Legal Compliance:** Ensure you have explicit authorization before running any attack simulations
4. **Cleanup:** Always clean up test artifacts (FIM script includes cleanup option)

### Configuration Notes

1. **Active Response Disabled:** Active response rules are disabled in the homelab configuration to allow attack testing without automatic blocking
2. **FIM Scan Frequency:** Set to 30 minutes for faster testing (production typically uses 12-24 hours)
3. **Alert Levels:** Configured to capture medium-level and above for comprehensive testing
4. **IP Addresses:** Configuration files reference 192.168.1.102 - update for your environment

### Customization

**To adapt for your environment:**

1. **Update Manager IP:**
   - In `agents/kali-agent.conf`: Change `<address>192.168.1.102</address>`
   - In deployment scripts: Update `WAZUH_MANAGER_IP` variable

2. **Adjust FIM Directories:**
   - Add/remove monitored directories in `ossec.conf` and agent configs
   - Adjust `realtime` monitoring based on system performance

3. **Enable Active Response:**
   - In `wazuh-manager/ossec.conf`: Change `<disabled>yes</disabled>` to `no`
   - Test carefully to avoid blocking legitimate traffic

---

## 📚 Additional Resources

### Official Documentation
- [Wazuh Documentation](https://documentation.wazuh.com/)
- [Wazuh Agent Deployment](https://documentation.wazuh.com/current/installation-guide/wazuh-agent/index.html)

### MITRE ATT&CK
- [MITRE ATT&CK Framework](https://attack.mitre.org/)
- [Technique T1110: Brute Force](https://attack.mitre.org/techniques/T1110/)
- [Technique T1053: Scheduled Task/Job](https://attack.mitre.org/techniques/T1053/)

### Related Project Documentation
- [Main README](../../README.md) - Project overview
- [Workflow Documentation](../../WORKFLOW_DOCUMENTATION.md) - Build process and troubleshooting
- [Attack Scenarios](../../ATTACK_SCENARIOS.md) - Detailed attack analysis

---

## 🤝 Contributing

This is a personal portfolio project, but suggestions for improvements are welcome via GitHub issues.

---

## 📄 License

MIT License - See [LICENSE](../../LICENSE) file for details

---

**Project:** SIEM Homelab  
**Author:** Trevon Swift  
**Last Updated:** November 2025
