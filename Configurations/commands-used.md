# Commands Used in SIEM Homelab

This document contains the commands executed during the project.

## Agent Deployment (Manual)
```bash
# Downloaded agent from manager
wget http://192.168.1.102:8000/wazuh-agent_4.14.1-1_amd64.deb

# Installed agent
sudo WAZUH_MANAGER='192.168.1.102' WAZUH_AGENT_NAME='kali' dpkg -i ./wazuh-agent_4.14.1-1_amd64.deb

# Started agent
sudo systemctl start wazuh-agent
```

## Attack Simulations

### SSH Brute Force
```bash
# Manual failed login attempts
for i in {1..10}; do ssh fakeuser@localhost 2>/dev/null; done

# Using Hydra
hydra -l root -P /usr/share/wordlists/rockyou.txt ssh://127.0.0.1
```

### File Integrity Monitoring
```bash
# Modified /etc/hosts
echo "# Test modification" | sudo tee -a /etc/hosts

# Created suspicious file
sudo touch /etc/backdoor_test.sh
```
