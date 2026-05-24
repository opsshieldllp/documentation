---
sidebar_position: 12
title: Web Application Firewall
description: Manage the cPGuard Web Application Firewall (WAF), custom modsec exemptions, custom rules, and secure file upload settings via HTTP Upload Scanner.
tags: [cpguard, cpgcli, waf, upload-scanner, modsecurity, firewall, upload]
---

# Web Application Firewall

Protect incoming HTTP traffic, exclude false-positive rules globally or per domain, inspect real-time web logs, and configure security filters for file uploads.

---

## Web Application Firewall `waf`

Manage application-layer filters, enable specific inspection engines, and handle rule exceptions.

### View WAF Settings
```bash
cpgcli waf
```

### Enable WAF Completely
```bash
cpgcli waf --enable
```

### Disable WAF Completely
```bash
cpgcli waf --disable
```

### Enable Selective Web-Filter Modules
Modulate active inspection engines (supported modules: `scanner`, `webshell`, `captcha`, `captchav2`, `crawler`, `proxy`):
```bash
cpgcli waf --enable scanner,webshell,captcha
```

### Disable Selective Web-Filter Modules
```bash
cpgcli waf --disable crawler,proxy
```

### Global Rule Exclusion Whitelist
Exclude false positive mod-rule triggers server-wide:
```bash
cpgcli waf --whitelist --add 1007
```
Or remove a rule from the whitelist:
```bash
cpgcli waf --whitelist --remove 1007
```

### Domain-Scoped Rule Exclusion Whitelist
Limit a whitelist scope exclusively to a specific hosting domain:
```bash
cpgcli waf --whitelist --add 1007 --domain example.com
```

### Live Log Streaming Interactive Inspector
Check log traces in real time and easily select rules to whitelist interactively:
```bash
cpgcli waf --watch
```

---

## HTTP Upload Scanner `upload-scanner`

Manage continuous security rules during file-upload streams to block untrusted script uploads.

### List Upload Whitelist
```bash
cpgcli upload-scanner --whitelist --list
```

### Add Hash to Whitelist
Specify file MD5 hash (if a file path is provided indeed, MD5 is generated dynamically):
```bash
cpgcli upload-scanner --whitelist --add d41d8cd98f00b204e9800998ecf8427e
```

### Remove Hash from Whitelist
```bash
cpgcli upload-scanner --whitelist --remove d41d8cd98f00b204e9800998ecf8427e
```

### Block HTTP PHP Uploads
Block requests uploading raw executable `.php` files:
```bash
cpgcli upload-scanner --block-php enable
```

### Allow HTTP PHP Uploads
```bash
cpgcli upload-scanner --block-php disable
```
