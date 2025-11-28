## Troubleshooting Log and Lessons Learned

This log captures the troubleshooting process, errors encountered, solutions implemented, and lessons learned during the SIEM homelab build. This documentation demonstrates problem-solving methodology and technical troubleshooting skills valuable for security operations roles.

## Issue #3: Dashboard Accessibility - Network Adapter Configuration
**Date:** November 1, 2025  
**Phase:** Dashboard Access After Successful Installation

#### Problem Description
After successful Wazuh installation on fresh VM (internal SSD, Ubuntu 25.10), initial dashboard access attempt failed from Windows host browser.

**Symptoms:**
- Wazuh services running correctly (all green/active)
- VM showing IP address in 10.x.x.x range
- Unable to access dashboard from Windows host initially
- Uncertainty about which IP address to use (IPv4 vs IPv6)

#### Troubleshooting Steps Taken

**Step 1: IP Address Identification**
```bash
hostname -I
# or
ip a
```
**Result:** VM showing 10.x.x.x IP address (valid private IP range)

**Question:** Should inet6 (IPv6) address be used instead?  
**Answer:** No - IPv4 (10.x.x.x) is correct for this lab environment

**Step 2: Network Adapter Verification**
Checked VirtualBox network settings for VM:
- Settings → Network → Adapter 1
- **Discovery:** Adapter was NOT set to Bridged
- **Configuration:** Set to NAT or Host-Only (incorrect for multi-VM communication)

#### Root Cause
Network adapter misconfiguration prevented Windows host from reaching VM on the network.

**NAT Mode Limitation:**
- VM can access internet
- VM cannot be reached by host or other VMs
- Creates isolated network environment

**Bridged Adapter Requirement:**
- VM appears as separate device on network
- Accessible by host machine
- Enables VM-to-VM communication (needed for agent deployment)

#### Resolution
**Action:** Changed network adapter to Bridged mode

**Steps:**
1. Shut down VM (or changed while running if VirtualBox allows)
2. VirtualBox Manager → Right-click VM → Settings
3. Network → Adapter 1 → Attached to: **Bridged Adapter**
4. Selected host's network adapter (WiFi or Ethernet)
5. Clicked OK
6. Started/restarted VM
7. Accessed dashboard: `https://10.x.x.x` from Windows host browser

**Result:** ✅ SUCCESS - Dashboard accessible, admin login successful

#### Key Learnings

**Network Adapter Modes:**
- **NAT:** VM isolated, can access internet, cannot be accessed
- **Bridged:** VM on same network as host, fully accessible
- **Host-Only:** VM-to-host only, no internet access

**For SIEM Lab Requirements:**
- Must use Bridged Adapter
- Enables agent communication from monitored VMs
- Allows host machine to access dashboard
- Critical for multi-VM security monitoring scenarios

**IP Address Ranges:**
All private IP ranges are valid:
- 10.x.x.x ✅
- 172.16.x.x - 172.31.x.x ✅
- 192.168.x.x ✅
IPv6 (inet6) not needed for this lab

## Issue #4: Lost Admin Credentials
**Date:** November 12, 2025  
**Phase:** Pre-Phase 2 (Before Agent Deployment)

#### Problem Description
Admin credentials for Wazuh dashboard were not saved/accessible after initial successful installation.

**Symptoms:**
- Wazuh Manager operational and accessible
- Dashboard login page reachable at `https://10.x.x.x`
- Admin password unknown/unavailable
- Unable to proceed with agent deployment without dashboard access

#### Troubleshooting Steps Considered

**Step 1: Credential Recovery Investigation**
Researched available options for recovering/resetting Wazuh admin credentials.

**Potential Solutions Identified:**

**Option A: Password Reset via Command Line**
```bash
# Wazuh includes password management tools
sudo /usr/share/wazuh-indexer/plugins/opensearch-security/tools/wazuh-passwords-tool.sh
```
- Would require understanding Wazuh security architecture
- Risk of breaking existing configuration
- Time investment for unfamiliar process

**Option B: Extract Credentials from Installation Logs**
```bash
# Check if original credentials stored in logs
sudo cat /var/log/wazuh-install.log | grep -i password
sudo cat ~/wazuh-install-files.tar
```
- Credentials may have been displayed during install but not captured
- Installation logs might not retain cleartext passwords
- Low probability of success

**Option C: Reinstall Wazuh**
- Clean reinstallation would generate new credentials
- Would display admin password during install process
- Guaranteed to resolve access issue
- Risk: losing current configuration (minimal at this stage)

#### Decision Analysis

**Factors Considered:**

**Current System State:**
- ✅ Fresh installation (< 24 hours old)
- ✅ No agents deployed yet
- ✅ No custom rules created
- ✅ No configuration changes made
- ✅ No data to preserve
- ✅ Only at baseline state

**Time Investment Comparison:**
- Password reset/recovery: Unknown duration, uncertain success
- Reinstallation: 30 minutes, guaranteed success

**Risk Assessment:**
- Password reset: Risk of system misconfiguration
- Reinstallation: No risk (nothing to lose at this stage)

**Learning Value:**
- Password reset: Limited learning (one-time issue)
- Reinstallation: Reinforces installation process, better documentation

#### Resolution Decision: Reinstall

**Rationale:**
Given that:
1. No meaningful configuration existed yet
2. No agents were deployed
3. Reinstallation would take less time than troubleshooting password reset
4. Guaranteed to provide fresh, known credentials
5. Opportunity to document installation process more thoroughly

**Decision:** Perform clean Wazuh reinstallation

#### Implementation Steps

**Step 1: Uninstall Existing Wazuh**
```bash
# Stop all services
sudo systemctl stop wazuh-manager wazuh-indexer wazuh-dashboard

# Remove packages
sudo apt remove --purge wazuh-manager wazuh-indexer wazuh-dashboard -y

# Remove data directories
sudo rm -rf /var/ossec
sudo rm -rf /etc/wazuh-indexer
sudo rm -rf /usr/share/wazuh-indexer
sudo rm -rf /var/lib/wazuh-indexer
sudo rm -rf /usr/share/wazuh-dashboard

# Clean package cache
sudo apt autoremove -y
```
(Content truncated due to size limit. This is the end of the content from the uploaded file.)
