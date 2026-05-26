---
sidebar_position: 7
title: Database Scanner
description: Configure and execute the cPGuard Database Scanner (dbscan) to find malicious injections in your databases.

---

# Database Scanner (`dbscan`)

cPGuard's Database Scanner analyzes WordPress database tables to identify malicious modifications, persistent injections, and suspicious content. Use these commands to manage settings, signatures, and scan jobs.

---

## DB Scanner Service

### Check DB Scanner Status
View the execution and integration status of the database scanner:
```bash
cpgcli dbscan --status
```

### Enable DB Scanner
```bash
cpgcli dbscan --enable
```

### Disable DB Scanner
```bash
cpgcli dbscan --disable
```

---

## Scanning Database Tables

### Run Server-wide DB Scans
Scan databases for all configured websites on the server:
```bash
cpgcli dbscan --scan --all
```

### Run Targeted DB Scan
Query a specific WordPress installation by path:
```bash
cpgcli dbscan --scan /home/user/public_html
```

---

## Managing Database Signatures

### Whitelist Database Signatures
Add signature rules you wish to exclude (multiple IDs can be separated by spaces or commas):
```bash
cpgcli dbscan --whitelist --add SIGNATURE_ID
```

### Remove Database Signature Exclusion
```bash
cpgcli dbscan --whitelist --remove SIGNATURE_ID
```

### Force Database Signatures Sync
Download the latest database scanner definitions instantly:
```bash
cpgcli dbscan --update
```
