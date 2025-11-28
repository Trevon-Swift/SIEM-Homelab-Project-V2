## Troubleshooting Log and Lessons Learned

This log captures the troubleshooting process, errors encountered, solutions implemented, and lessons learned during the SIEM homelab build. This documentation demonstrates problem-solving methodology and technical troubleshooting skills valuable for security operations roles.

---

## Project Information
- **Project:** SIEM Homelab with Wazuh 4.14
- **Start Date:** November 1, 2025
- **Platform:** VirtualBox on Windows Host
- **Target OS:** Ubuntu Server 24.04.1 LTS

---

## Issue Timeline & Resolution

### Issue #1: Initial Installation Failure
**Date:** November 1, 2025  
**Phase:** Initial Wazuh Manager Deployment

#### Problem Description
First Wazuh 4.14 installation appeared to complete but services failed to start properly.

**Symptoms:**
```
wazuh-indexer: not-found, inactive, dead
wazuh-manager: loaded, failed, failed
wazuh-dashboard: status unknown
```

**Error Messages:**
- `wazuh_manager.service not found`
- Services failing to start on system boot
- Dashboard inaccessible via browser (ERR_CONNECTION_REFUSED)

#### Troubleshooting Steps Taken

**Step 1: Service Status Verification**
```bash
sudo systemctl status wazuh-manager
sudo systemctl status wazuh-indexer
sudo systemctl status wazuh-dashboard
```
**Result:** Services either missing or failed state

**Step 2: Network Connectivity Test**
- Verified VM network adapter configuration (Bridged vs NAT)
- Confirmed IP address assignment via `hostname -I`
- Tested connectivity from Windows host to Ubuntu VM
**Result:** Network configuration correct, connectivity confirmed

**Step 3: Installation Verification**
```bash
dpkg -l | grep wazuh
```
**Result:** Packages showed as installed but non-functional

#### Resolution Attempt #1: Reinstallation Over Existing Install
**Action:** Attempted to reinstall Wazuh using same installer script  
**Result:** ❌ Failed - Installer detected existing installation, refused to proceed

#### Resolution Attempt #2: Package Removal and Clean Reinstall
**Action:** Attempted standard package removal
```bash
sudo apt remove --purge wazuh-manager wazuh-indexer wazuh-dashboard
```

**Error Encountered:**
```
error processing package wazuh-manager (--remove):
installed wazuh-manager package pre-removal script subprocess 
returned error exit status 127
dpkg: too many errors, stopping
E: Sub-process /usr/bin/dpkg returned an error code (1)
```

**Analysis:** Package removal scripts were corrupted, preventing clean uninstall

#### Resolution Attempt #3: Force Package Removal
**Action:** Attempted force removal bypassing broken scripts
```bash
sudo dpkg --remove --force-remove-reinstreq wazuh-manager
sudo dpkg --purge wazuh-manager
```
**Result:** ❌ Failed - Same error persisted, package database corrupted

#### Root Cause Analysis
Upon further investigation, discovered underlying storage issue:

**Storage Configuration Problem:**
- Initial VM created with 50GB virtual disk
- VM stored on external HDD via USB connection
- External HDD had limited free space (~50-60GB total free)

**Contributing Factors:**
1. Insufficient disk space allocation (50GB too small for Wazuh)
2. External HDD performance limitations
3. Potential USB connection instability during heavy I/O operations
4. Journal/logging conflicts with external storage

#### Final Resolution: Fresh Installation with Corrected Configuration

**Decision:** Complete VM rebuild with proper specifications

**Action Plan:**
1. Delete corrupted VM entirely
2. Create new VM with corrected specifications
3. Store VM on internal SSD (not external HDD)
4. Increase virtual disk size to 80GB
5. Perform clean Wazuh installation

**New VM Specifications:**
- **Location:** Internal SSD (C:\Users\[username]\VirtualBox VMs\)
- **RAM:** 4GB
- **CPUs:** 2
- **Disk:** 80GB (dynamically allocated)
- **Network:** Bridged Adapter
- **OS:** Ubuntu Server 24.04.1 LTS

**Rationale for Changes:**
- **Internal SSD:** Eliminates USB connection issues, improves I/O performance
- **80GB disk:** Provides adequate space for Wazuh (~40-50GB) plus growth
- **Dynamic allocation:** Conserves host disk space (starts ~5GB, grows as needed)

#### Implementation Status
**Status:** ✅ RESOLVED - Installation successful with corrected configuration

**Final Resolution Steps:**
1. Deleted corrupted VM from external HDD
2. Created fresh VM on internal SSD with 80GB disk
3. Installed Ubuntu Server 25.10 (latest stable release)
4. Installed Wazuh 4.14 successfully
5. **Critical: Configured Bridged Adapter** (initially missed this step)
6. Dashboard accessible from Windows host

**Key Success Factor:** Network adapter configuration (Bridged) combined with internal SSD storage

---

### Issue #2: Dashboard Connectivity (ERR_CONNECTION_REFUSED)
**Date:** November 1, 2025  
**Phase:** Initial Access Attempt

#### Problem Description
Unable to access Wazuh dashboard from Windows host browser despite services appearing to run.

**Symptoms:**
- Browser error: `ERR_CONNECTION_REFUSED`
- Unable to reach `https://[VM-IP]` from host machine

#### Troubleshooting Steps Taken

**Step 1: Service Status Verification**
```bash
sudo systemctl status wazuh-dashboard
```
**Result:** Service status unclear due to underlying installation issues

**Step 2: Network Configuration Check**
- Verified VM network adapter set to Bridged (not NAT)
- Confirmed IP address assignment
- Tested ping from host to VM
**Result:** Network connectivity functional

**Step 3: Port Verification**
```bash
sudo netstat -tlnp | grep 443
```
**Result:** Could not verify due to service failures

**Step 4: Firewall Check**
```bash
sudo ufw status
```
**Result:** Firewall not active (not the issue)

#### Root Cause
Connection refused error was symptom of underlying installation failure (Issue #1). Services were not actually running properly despite initial appearance.

#### Resolution
Will be resolved by Issue #1 resolution (clean installation on proper storage)

---

## Key Lessons Learned

### Technical Lessons

#### 1. Storage Location Matters
**Learning:** VirtualBox VMs perform significantly better on internal SSDs vs external HDDs
- External HDD via USB introduces latency and potential connection issues
- Heavy I/O operations (like Wazuh installation) can overwhelm USB connections
- Journal/logging systems may fail on external storage during intense writes

**Best Practice:** Always store active VMs on internal storage when possible

#### 2. Disk Space Planning
**Learning:** Initial 50GB allocation was insufficient for enterprise SIEM deployment
- Wazuh requires ~15GB for installation
- Indexing and log storage require additional 20-30GB
- OS overhead and updates need buffer space

**Best Practice:** Allocate 80GB+ for SIEM VMs, use dynamic allocation to conserve host space

#### 3. Package Management Recovery
**Learning:** Corrupted package installations can create cascading failures
- Standard removal procedures may fail if installation scripts are corrupted
- Force removal options (`--force-remove-reinstreq`) may not always work
- Sometimes starting fresh is faster than repairing broken installations

**Best Practice:** When package database is corrupted and force removal fails, rebuild VM

#### 4. Network Configuration
**Learning:** NAT vs Bridged adapter distinction is critical for multi-VM communication
- NAT: VM can access internet but other VMs cannot reach it
- Bridged: VM appears as separate machine on network, enables VM-to-VM communication

**Best Practice:** Use Bridged adapter for security lab environments requiring inter-VM traffic

### Problem-Solving Methodology Lessons

#### 1. Systematic Troubleshooting Approach
- ✅ Start with service verification
- ✅ Check network connectivity
- ✅ Verify configurations
- ✅ Examine logs for errors
- ✅ Test individual components
- ✅ Identify root cause before attempting fixes

#### 2. When to Rebuild vs Repair
**Rebuild When:**
- Installation is completely corrupted
- Multiple cascading failures present
- Time to rebuild < time to troubleshoot
- No critical data/configuration to preserve
- VM is newly created (< 48 hours old)

**Repair When:**
- Single isolated issue
- Clear path to resolution
- Significant configuration exists
- Data preservation required

**Decision:** In this case, rebuild was correct choice (VM was 1 day old, no custom configs, multiple failures)

#### 3. Documentation During Troubleshooting
- Real-time logging captures decision-making process
- Error messages should be copied exactly as displayed
- Each troubleshooting step should be documented before moving to next
- Failed attempts are as valuable as successful ones for learning

---

## Screenshot & Documentation Strategy

### What to Screenshot (Avoid Redundancy)

#### One-Time Screenshots (Take Once)
✅ **VirtualBox VM Settings**
- System tab (RAM/CPU allocation) - ONE screenshot
- Network tab (Bridged adapter configuration) - ONE screenshot
- Storage tab (disk allocation) - ONE screenshot

✅ **Ubuntu Installation**
- Initial boot screen - OPTIONAL (not critical)
- Installation summary screen - ONCE
- First successful login - ONCE

✅ **Wazuh Installation**
- Installation command execution start - ONCE
- Admin credentials display (CRITICAL - redact password in shared docs) - ONCE
- "Installation finished" message - ONCE

✅ **Dashboard Access**
- First successful dashboard login - ONCE
- Dashboard overview/home screen - ONCE

#### Repeated Process Screenshots (Take ONCE, Reference Multiple Times)
❌ **Do NOT Screenshot Every Time:**
- Running `sudo systemctl status` commands (document command in text instead)
- Every package installation step
- Every `apt update` command
- Every configuration file edit (show final config only)
- Multiple failed login attempts to dashboard

✅ **Do Screenshot Each Unique Error:**
- Different error messages (each unique error once)
- Critical failures (corrupted packages, disk space issues)

#### What to Document in Text Only (No Screenshots Needed)
📝 **Commands and Outputs:**
- Service status checks → Copy/paste text output
- Network configuration commands → Text output sufficient
- Package queries → Text output sufficient
- Log file excerpts → Copy relevant lines

📝 **Troubleshooting Steps:**
- Decision rationale → Written explanation
- Commands attempted → Code blocks with results
- Why specific approach chosen → Written analysis

---

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
