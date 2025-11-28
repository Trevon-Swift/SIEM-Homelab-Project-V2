# SIEM Homelab Project - Comprehensive Troubleshooting Log & Lessons Learned

**Document Purpose:** This log captures the troubleshooting process, errors encountered, solutions implemented, and lessons learned during the SIEM homelab build. This documentation demonstrates problem-solving methodology and technical troubleshooting skills valuable for security operations roles.

---

## 📋 Project Information
- **Project:** SIEM Homelab with Wazuh 4.14
- **Start Date:** November 1, 2025
- **Platform:** VirtualBox on Windows Host
- **Target OS:** Ubuntu Server 24.04.1 LTS / 25.10
- **Status:** ✅ Active & Operational

---

## 🔧 Issue Timeline & Resolution

### Issue #1: Initial Installation Failure
**Date:** November 1, 2025  
**Phase:** Initial Wazuh Manager Deployment

#### Problem Description
First Wazuh 4.14 installation appeared to complete, but services failed to start properly.
**Symptoms:**
- `wazuh-indexer: not-found, inactive, dead`
- `wazuh-manager: loaded, failed, failed`
- Dashboard inaccessible via browser (`ERR_CONNECTION_REFUSED`)

#### Troubleshooting Steps
1.  **Service Verification:** `sudo systemctl status wazuh-manager` returned failed state.
2.  **Network Test:** Confirmed IP assignment and Host-to-VM connectivity were functional.
3.  **Package Check:** `dpkg -l | grep wazuh` showed packages installed but non-functional.
4.  **Attempted Fixes:**
    * *Attempt 1:* Reinstall over existing (Failed - installer blocked).
    * *Attempt 2:* `apt remove --purge` (Failed - package scripts corrupted, exit code 127).
    * *Attempt 3:* Force removal `dpkg --force-remove-reinstreq` (Failed).

#### Root Cause Analysis
**Storage I/O Failure:** The VM was created on an external USB HDD with insufficient space (50GB) and low I/O throughput. The installation corrupted due to write latency during the heavy indexing setup.

#### Resolution
**Action:** Complete VM Rebuild.
1.  Deleted corrupted VM.
2.  Created new VM on **Internal SSD**.
3.  Increased disk size to **80GB** (Dynamic).
4.  Configured **Bridged Adapter** immediately.
5.  Performed clean install of Ubuntu and Wazuh.

**Result:** ✅ RESOLVED - Installation successful.

---

### Issue #2: Dashboard Connectivity (ERR_CONNECTION_REFUSED)
**Date:** November 1, 2025  
**Phase:** Initial Access Attempt

#### Problem Description
Unable to access Wazuh dashboard from Windows host browser despite services appearing to run.

#### Root Cause
This was a secondary symptom of **Issue #1**. The services were not actually running due to the corrupted installation, even though the VM responded to Ping.

#### Resolution
Resolved automatically by the clean installation performed in Issue #1.

---

### Issue #3: Dashboard Accessibility - Network Adapter Configuration
**Date:** November 1, 2025  
**Phase:** Phase 1 - Network Configuration

#### Problem Description
After a successful fresh install (Issue #1 resolution), the dashboard was still unreachable from the Windows Host.
**Symptoms:**
- VM had valid IP (10.x.x.x).
- Wazuh services active.
- Browser on Host timed out connecting to VM IP.

#### Root Cause
**VirtualBox Network Misconfiguration.**
The VM was set to **NAT** or **Host-Only**.
* *NAT:* VM allows outbound internet, but Host cannot initiate connection to VM.
* *Host-Only:* Host can see VM, but VM has no internet for updates.

#### Resolution
**Action:** Changed Network Adapter to **Bridged Mode**.
1.  VM Settings -> Network -> Adapter 1.
2.  Changed "Attached to" from NAT to **Bridged Adapter**.
3.  Rebooted VM.

**Result:** ✅ RESOLVED - Dashboard accessible via host browser at `https://10.x.x.x`.

---

### Issue #4: Lost Admin Credentials
**Date:** November 12, 2025  
**Phase:** Pre-Phase 2 (Before Agent Deployment)

#### Problem Description
Admin credentials for the Wazuh dashboard were displayed once during installation but were not saved.

#### Decision Analysis
I evaluated two options:
1.  **Password Recovery:** Using `wazuh-passwords-tool.sh`. High complexity, risk of breaking config.
2.  **Reinstallation:** Clean slate. Guaranteed success. Low time cost (~30 mins).

#### Resolution
**Decision:** **Reinstall Wazuh.**
Since the environment was <24 hours old with no data, a reinstall was more efficient than troubleshooting.
1.  Purged all Wazuh packages.
2.  Ran `wazuh-install.sh -a`.
3.  **Critical Step:** Immediately screenshotted and saved credentials to a password manager.

**Result:** ✅ RESOLVED - Access restored.

---

### Issue #5: Agent Connectivity (Dual Network Adapters)
**Date:** November 17, 2025  
**Phase:** Phase 2 - Kali Agent Deployment

#### Problem Description
Kali Linux agent could ping the Manager, but `wget` downloads failed, and the agent could not register.

**Symptoms:**
- `ping 192.168.1.102` (Manager): SUCCESS
- `ping google.com`: FAILED
- `wget` (download agent): TIMEOUT

#### Root Cause
**Routing Conflict via Dual Adapters.**
The Kali VM had two active adapters:
1.  **Bridged** (Correct)
2.  **Host-Only** (Conflicting)
The OS was trying to route internet traffic through the Host-Only adapter, which has no gateway.

#### Resolution
**Action:** Reconfigured Adapters.
1.  Kept **Adapter 1** as **Bridged** (Primary for LAN/Internet).
2.  Changed **Adapter 2** to **NAT** (Backup) or Disabled it.

**Result:** ✅ RESOLVED - Internet access restored, Agent downloaded and registered.

---

### Issue #6: Wazuh Indexer Service Failure
**Date:** November 17, 2025  
**Phase:** Phase 2 - Post-Agent Installation

#### Problem Description
Dashboard became inaccessible. `systemctl status` showed `wazuh-indexer` as **failed**.

**Log Error:**
`failed to execute systemd-entrypoint: permission denied`

#### Root Cause
**File Permission Corruption.**
The ownership of the indexer directory was incorrect (likely due to a `sudo` command run improperly or a disrupted update), preventing the service user from reading/writing data.

#### Resolution
**Action:** Reset Permissions.
```bash
# Reset Ownership
sudo chown -R wazuh-indexer:wazuh-indexer /var/lib/wazuh-indexer
sudo chown -R wazuh-indexer:wazuh-indexer /etc/wazuh-indexer

# Reset Execution Permissions
sudo chmod 755 /usr/share/wazuh-indexer/bin/systemd-entrypoint

```
**Result:** ✅ RESOLVED - Services restarted successfully.
