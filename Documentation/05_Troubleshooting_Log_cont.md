# Issue #7: Duplicate Agent Enrollment Error

**Date:** November 17, 2025  
**Phase:** Phase 2 - Kali Agent Deployment

## Problem Description
After reinstalling the agent on the Kali machine to fix configuration issues, enrollment failed.

**Error Message:**
ERROR: Duplicate agent name: kali. Unable to add agent (from manager)
ERROR: Unable to connect to '[192.168.1.102]:1514/tcp'

## Root Cause
**Orphaned Agent Entry.** The Wazuh Manager database still held the registration for "kali" from the previous failed installation. Wazuh prevents overwriting existing agents by default to preserve data integrity.

## Resolution
**Action:** Remove Orphaned Agent from Manager.

**On Wazuh Manager:**
```bash
# List agents to find ID
sudo /var/ossec/bin/manage_agents -l

# Remove the specific agent (e.g., ID 001)
sudo /var/ossec/bin/manage_agents -r 001
```
**On Kali Agent:**

```bash
# Restart agent to trigger new enrollment
sudo systemctl restart wazuh-agent
```
Result: ✅ RESOLVED - Agent "kali" re-enrolled with a new ID (002) and began sending logs.

## 🧠 Key Lessons Learned

### Technical Best Practices
- **Storage:** Always use Internal SSDs for SIEM VMs. External HDDs cannot handle the IOPS required for indexing logs.
- **Network:** Use Bridged Mode for Lab environments. It is the simple way to allow Host <-> VM and VM <-> VM communication simultaneously.
- **Permissions:** Linux services are fragile regarding ownership. If a service fails with "Permission Denied," check `chown` before reinstalling.
- **Credential Hygiene:** Never close a terminal window after installation until credentials are saved in three locations (Manager, Text File, Screenshot).

### Troubleshooting Methodology
- **Rebuild vs. Repair:**
  - New System (<24 hours): **Rebuild.** It's faster and ensures a clean baseline.
  - Established System: **Troubleshoot** to preserve data.
- **Verify Layers:** Don't assume the app is broken if the network is down. Check `ping` -> `systemctl` -> logs in that order.
- **Documentation:** Writing down the exact error message (e.g., "Exit Code 127") saved time by allowing me to Google the specific failure rather than a generic "it didn't work."
