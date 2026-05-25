# cPGuard CLI `cpgcli` Overview

The cPGuard CLI `cpgcli` provides a centralized command-line interface to manage and automate various cPGuard features directly from the server terminal.

It is designed for system administrators, DevOps engineers, hosting providers, and automation workflows that require direct access to cPGuard functionality without using the web interface.


---

## Command Syntax

All interactions with the CLI follow a standardized structure:

```bash
cpgcli <command> --<option> [value]
```

* **`command`**: The primary security module or subsystem you want to manage (e.g., `scanner`, `fw`, `waf`).
* **`--option`**: The specific action or setting configuration.
* **`value`**: Additional required arguments, paths, or variables.

---

## Viewing Available Commands

To display all available commands:

```bash
cpgcli --help
```

To view help for a specific command:

```bash
cpgcli <command> --help
```

Example:

```bash
cpgcli scan --help
```

---


## Permissions & Security

Many commands interact directly with:

* Firewall rules
* System services
* Malware quarantine
* Server-wide configurations

Because of this, cpgcli commands should be executed as:

```bash
root
```
---

## Command Index

Use the command index below to quickly find details, instructions, and command options for any `cpgcli` module.

| Command | Description / Purpose | Reference Guide |
|---|---|---|
| `license` | License activation, status checks, and metadata sync | [License, Setup & Config](./general) |
| `config` | Export and import full configuration profiles | [License, Setup & Config](./general) |
| `cloud` | Connection checking and syncing to cloud nodes | [License, Setup & Config](./general) |
| `update` | Manage application and signature database updates | [License, Setup & Config](./general) |
| `support-access` | Grant or revoke remote technical access keys | [License, Setup & Config](./general) |
| `api` | Manage local API keys for automation portal hooks | [License, Setup & Config](./general) |
| `standalone-conf` | Hardening profile wizards and setups for direct OS installations | [License, Setup & Config](./general) |
| `panel-integration` | Configure Single Sign-On and user features for control panels | [License, Setup & Config](./general) |
| `branding` | White-label setups and branding assets configurations | [License, Setup & Config](./general) |
| `ui-users` | Authorize specific login accounts with UI permissions | [License, Setup & Config](./general) |
| `notification` | Set up SMTP mail relays and customer alerts options | [License, Setup & Config](./general) |
| `scanner` | Start, stop, or query background scanner state | [Malware Scanner](./malware-scanner) |
| `dailyscan` | Configure midnight incremental file scans | [Malware Scanner](./malware-scanner) |
| `weeklyscan` | Configure Sunday midnight full file scans | [Malware Scanner](./malware-scanner) |
| `ai-scan` | Toggle machine-learning heuristic engines | [Malware Scanner](./malware-scanner) |
| `watch` | Manage directories continuously monitored by the sensor | [Malware Scanner](./malware-scanner) |
| `whitelist` | Exclude friendly system users and clean files from scans | [Malware Scanner](./malware-scanner) |
| `blacklist` | Add specific script indicators to force-block during scans | [Malware Scanner](./malware-scanner) |
| `file-action` | Map baseline active resolution metrics for found viruses | [Malware Scanner](./malware-scanner) |
| `cleanup` | Control automatic injection purging on scripts | [Malware Scanner](./malware-scanner) |
| `log-action` | Delete, quarantine, disable, or restore files from logs | [Malware Scanner](./malware-scanner) |
| `report` | Submit false positives or unchecked malware to labs | [Malware Scanner](./malware-scanner) |
| `scan` | Run manual scans, check active scan results, and manage scan jobs | [Manual Scan and Operations](./manual-scan) |
| `dbscan` | Scan database tables for persistent malware injections | [Database Scanner](./db-scanner) |
| `waf` | Turn on/off ModSecurity engines, manage rule whitelists, watch logs | [Web Application Firewall & Upload Scanner](./waf) |
| `upload-scanner` | Scan file uploads & block malicious script streams | [Web Application Firewall & Upload Scanner](./waf) |
| `fw` | Manage system firewall state and engine wrappers | [Firewall & IP Management](./firewall-and-ip) |
| `ip` | Allow, deny, and clean specific IP boundaries | [Firewall & IP Management](./firewall-and-ip) |
| `lfd` | Control LFD services and intrusion jails | [Monitors, LFD & Mail Queue](./monitors) |
| `osm` | Configure outbound SMTP rate limits and pattern blocks | [Monitors, LFD & Mail Queue](./monitors) |
| `bot-check` | Enable or disable bad bot and scraper filtration | [Protection Modules](./modules) |
| `account-suspend` | Suspend compromised control panel accounts automatically | [Protection Modules](./modules) |
| `rootkit` | Execute rootkit scanning schedule parameters | [Protection Modules](./modules) |
| `srbl` | Enable or disable SRBL DNSBL spam checks | [Protection Modules](./modules) |
| `cms` | Auto WP update, WP cron overrides, and addon hardeners | [Protection Modules](./modules) |
| `process-monitor` | Configure runtime tracking of host binaries | [Monitors, LFD & Mail Queue](./monitors) |
| `cron-monitor` | Monitor cron tables for unauthorized injection entries | [Monitors, LFD & Mail Queue](./monitors) |
| `ip-reputation` | Query block status of outbound server IPs in public feeds | [Monitors, LFD & Mail Queue](./monitors) |


