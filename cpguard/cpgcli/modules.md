---
sidebar_position: 13
title: Protection Modules
description: Manage bot checks, account suspension, rootkit checks, SRBL, and CMS security controls.
tags: [cpguard, cpgcli, cms, reputation, security]
---

# Protection Modules

Deploy layered, real-time defenses for content management systems (CMS), bad bots, systems integrity, and network reputation feeds.

---

## Bot Check

Inspect and filter requests based on known bad user-agents or scrapers.

### Enable Bot Checks
```bash
cpgcli bot-check --enable
```

### Disable Bot Checks
```bash
cpgcli bot-check --disable
```

---

## Account Suspension

Block hosting space accounts automatically if catastrophic malware compromises or domain threats occur.

### Enable Global Suspensions
```bash
cpgcli account-suspend --enable
```

### Disable Global Suspensions
```bash
cpgcli account-suspend --disable
```

### Enable Under Specific Attack Conditions
Support options: `virus`, `domain`:
```bash
cpgcli account-suspend --enable virus,domain
```

---

## Rootkit Checks

Prevent deeply rooted binary modifications or kernels hijacks.

### Enable Rootkit Scan Runs
```bash
cpgcli rootkit --enable
```

### Disable Rootkit scans
```bash
cpgcli rootkit --disable
```

---

## Spam Righteous Block List

Query email senders against active DNS block lists to protect against incoming mail spam.

### Enable SRBL Check
```bash
cpgcli srbl --enable
```

### Disable SRBL Check
```bash
cpgcli srbl --disable
```

---

## Content Management System Protection

Strengthen, patch, or monitor WordPress instances and underlying themes or plugins.

### WordPress Core Verification
Turn on automatic core checksum integrity files checks and replacement:
```bash
cpgcli cms --wp-checksum enable
```

### WordPress Cron Overrides
Configure WordPress cron systems to bypass HTTP run limits and execute via CLI crontab task instead:
```bash
cpgcli cms --wp-cron enable --interval 2
```
And turn off custom WP-cron schedules:
```bash
cpgcli cms --wp-cron disable
```

### WordPress Auto Updater
Automatically patch vulnerable plugins, core builds, or themes:
```bash
cpgcli cms --wp-auto-update enable
```

### Whitelist WordPress Users
Prevent automated cPGuard containment actions from suspending specialized websites:
```bash
cpgcli cms --wp-whitelist --add user1
```

### Blacklist Incompatible or Vulnerable Plugins
Deactivate matching plugins completely across all CMS spaces:
```bash
cpgcli cms --plugin-blacklist --add vulnerable-plugin
```

### CMS Addon Metadata Trackers
Allow collecting WordPress version, theme, and plugin list data via wpcli:
```bash
cpgcli cms --wp-addon-check enable
```

