---
sidebar_position: 12
title: Monitors, LFD & Mail Queue
description: Configure LFD tracking, Outbound Spam Monitor (OSM), live process and cron monitoring, and IP reputation monitoring (DNSBL).
tags: [cpguard, cpgcli, lfd, osm, process-monitor, cron-monitor, ip-reputation, dnsbl]
---

# Monitors, LFD & Mail Queue

Govern system tracking daemons, monitor outgoing SMTP mail queues, set up live process and crontab sensors, and query public blacklist reputations (DNSBL).

---

## Log Failure Daemon

Control background monitoring rules, trace jail variables, or exclude friendly systems from tracking routines.

### Manage LFD Daemon State

Get current LFD runner state:
```bash
cpgcli lfd --status
```

Restart LFD engine:
```bash
cpgcli lfd --restart
```

### Exclude Friendly Services from Tracking

Prevent service processes (such as SSH) from triggering jail bans:
```bash
cpgcli lfd --ignore sshd
```

Check currently ignored services:
```bash
cpgcli lfd --ignore --list
```

Remove service exceptions:
```bash
cpgcli lfd --ignore --remove sshd
```

---

## Outbound Spam Monitor

Deploy SMTP safeguards to track mail queues, rate limits, and block spam outbreaks.

### List OSM Configuration Space
Check existing SMTP policies and thresholds:
```bash
cpgcli osm --list
```

### Enable OSM
```bash
cpgcli osm --enable
```

### Disable OSM
```bash
cpgcli osm --disable
```

### Set Rate Threshold Metrics
Set strict message caps per minute:
```bash
cpgcli osm --minute-threshold 60
```
Or specify maximum safe metrics on hourly parameters:
```bash
cpgcli osm --hourly-threshold 500
```

### Exclude Trusted IP Addresses
```bash
cpgcli osm --whitelist-ips --add 10.0.0.1
```
Remove IP exception:
```bash
cpgcli osm --whitelist-ips --remove 10.0.0.1
```

### Exclude Trusted Sender Emails
```bash
cpgcli osm --whitelist-email --add admin@example.com
```

### Exclude Active Working Directories (CWD)
Force exception paths for safe mass-mailing scripts:
```bash
cpgcli osm --whitelist-cwd-prefix --add /home/user/tmp
```

### Register Custom Spam Header and Body Patterns
Block outgoing emails matching custom regex text:
```bash
cpgcli osm --spam-pattern --add 'from .* viagra'
```

---

## Live Process Monitor

Observe running host files, trace patterns, and bypass specific background tasks.

### Enable Process Monitoring
```bash
cpgcli process-monitor --enable
```

### Disable Process Monitoring
```bash
cpgcli process-monitor --disable
```

### Exclude Friendly Processes
Prevent matching executables or command lines from triggering protection alerts:
```bash
cpgcli process-monitor --whitelist --add '/usr/bin/safe-job'
```

### Review Process Whitelist Entries
```bash
cpgcli process-monitor --whitelist --list
```

### Remove Process Exclusion
```bash
cpgcli process-monitor --whitelist --remove '/usr/bin/safe-job'
```

### Exclude Trustworthy Users
Bypasses all active processes launched by specific users:
```bash
cpgcli process-monitor --whitelist-users --add user1
```

### List User Whitelisting
```bash
cpgcli process-monitor --whitelist-users --list
```

### Remove User Exception
```bash
cpgcli process-monitor --whitelist-users --remove user1
```

---

## Cron Monitor

Scan and manage host cron spaces for suspicious modifications or anomalous background injections.

### Enable Cron Monitoring
```bash
cpgcli cron-monitor --enable
```

### Disable Cron Monitoring
```bash
cpgcli cron-monitor --disable
```

### Exclude Friendly Cron Spaces
Prevent specific users' cron schedules from being flagged:
```bash
cpgcli cron-monitor --whitelist-users --add user1
```

### Review Cron User Whitelist
```bash
cpgcli cron-monitor --whitelist-users --list
```

### Remove User Cron Exception
```bash
cpgcli cron-monitor --whitelist-users --remove user1
```

---

## IP reputation monitoring

Monitor and query public sender space blocks to prevent blacklist drops.

### Enable Automatic Reputation Monitor
```bash
cpgcli ip-reputation --enable
```

### Disable Automatic Reputation Monitor
```bash
cpgcli ip-reputation --disable
```

### Check Specific Host IP
Run live queries against the DNSBL / RBL database feeds:
```bash
cpgcli ip-reputation --check 1.1.1.1
```

### Review Historical Status Findings
Check overall scanner evaluations:
```bash
cpgcli ip-reputation --result
```
Or filter for a target address:
```bash
cpgcli ip-reputation --result 1.1.1.1
```

### Add Host/IP to Monitor
```bash
cpgcli ip-reputation --add-ip 198.51.100.10
```

### Remove Monitored IP
```bash
cpgcli ip-reputation --remove-ip 198.51.100.10
```

### Manage DNSBL Query Servers
Query curated block lists check engines:
```bash
cpgcli ip-reputation --list-hosts --available
```

### Add Custom Check Hostnames
Add customized check hostnames manually:
```bash
cpgcli ip-reputation --add-host zen.spamhaus.org
```
