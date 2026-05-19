---
title: Troubleshooting
sidebar_position: 5
---

If users are experiencing connectivity issues or you suspect a false positive, follow these steps.

### 1. Identify the Block

First, determine if and why an IP is being blocked:

* **Check the Status**: Use `cpgcli ip --check <IP>` to see if the address is in the local firewall, IPDB, or a temporary ban.
* **Examine Logs**: Check `/var/log/messages` or `/var/log/kern.log`. Blocked entries are typically prefixed with `IPDB Blocked:` or `CPG-FW Blocked:`.

### 2. Common Scenarios

| Issue | Potential Cause | Resolution |
| --- | --- | --- |
| **User cannot access SSH/Mail** | Log Monitoring Daemon | Check the **LMD** section in the App Portal and unblock the IP from it. |
| **Website returns 403/Timeout** | WAF or IPDB Block | Check the "Incidents" page for WAF hits. If not there, check IPDB status and **Whitelist** the IP. |
| **Port is closed** | Port Filter | Ensure the port is listed in the **Port Filter Configuration** for both TCP/UDP and IN/OUT directions. |

### 3. Conflict Resolution (CSF Migration)

The cPGuard Firewall is designed to replace legacy tools like CSF. If you are migrating:

* Ensure CSF is fully disabled to prevent rule conflicts within the NFT framework.
* If the firewall fails to start, verify no other third-party firewall service is locking the Netfilter tables.

### 4. Technical Support

If the issue persists, provide the output of the following command to cPGuard Support:

* `cpgcli fw --status`
* `cpgcli fw --help` (to see all available diagnostic flags)