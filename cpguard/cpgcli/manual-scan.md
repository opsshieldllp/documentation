---
sidebar_position: 6
title: Manual Scan
description: Execute manual scans, consult logs, review reports, and configure databases or gateway upload checking modules.

---

# Manual Scan and Operations

Perform on-demand scanner executions, query diagnostic logs, submit false positive file reports, or interact with database and web upload security scanners.

---

## On-Demand Manual Scans `scan`

### Run Complete Scan
Start scan execution on all directories inside the watchlist:
```bash
cpgcli scan --all
```

### Scan Specific Path
```bash
cpgcli scan --path /home/user/public_html
```

### Run Active Modified Scans
Scan watchlist directories for files modified in the past 24 hours:
```bash
cpgcli scan --daily
```

Scan watchlist directories for files modified in the past week:
```bash
cpgcli scan --weekly
```

### Set Custom Actions during Manual Scan
Run an on-demand scan with specific threat resolution behaviors (options: `email`, `disable`, `quarantine`):
```bash
cpgcli scan --all --virus-action quarantine --binary-action disable
```

### Run with Whitelist / Blacklist Files
Input text lists containing paths to whitelist or blacklist during execution:
```bash
cpgcli scan --all --whitelist /path/whitelist.txt --blacklist /path/blacklist.txt
```

### Run Scan silently (Background)
Avoid blocking the terminal while running large filesystem scans:
```bash
cpgcli scan --all --no-wait
```

---

## Active Scans Management

Track status, check logs, or terminate active scan jobs.

### List Manual Scans
```bash
cpgcli scan --list
```
Or check paginated listings:
```bash
cpgcli scan --list --page 1
```

### View Scan Job Results
Query results of target scan by job ID:
```bash
cpgcli scan --result 426 --page 1 --limit 20
```

### Export Results to CSV
```bash
cpgcli scan --result 426 --page 1 --limit 20 --export /home/user/scan_426.csv
```

### Stop Running Scan
```bash
cpgcli scan --stop 426
```

### Delete Scan History
```bash
cpgcli scan --delete 426
```
