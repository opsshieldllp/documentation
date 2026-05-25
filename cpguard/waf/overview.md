---
title: WAF Overview
sidebar_position: 1
---

cPGuard WAF is a Layer 7 ModSecurity ruleset for blocking common web attacks with low operational overhead. It combines Malware.Expert commercial signatures with cPGuard in-house rules tuned for hosting workloads.

cPGuard WAF inspects HTTP requests and responses (headers, body, cookies, and parameters). When a request matches a malicious pattern:

- The request is blocked
- A `403` or `406` response is returned
- A Rule ID is logged for review and troubleshooting

## Protection Coverage

### Generic Attack Categories

- **SQL Injection (SQLi)** — Prevents database manipulation attacks  
- **Cross-Site Scripting (XSS)** — Blocks script injection attempts  
- **Local File Include (LFI)** — Stops unauthorized file access  
- **Remote File Include (RFI)** — Prevents remote code execution  
- **File Upload Exploits** — Blocks malicious upload attempts  
- **Zero-Day Exploits** — Protection against newly discovered attack vectors  
- **Web Shell Execution** — Prevents backdoor script activity  

### Application-Specific Rules

Optimized protection for:

- WordPress (including brute-force login protection)
- Joomla (including brute-force login protection)
- Drupal

## Requirements

Before enabling WAF:

- ModSecurity version **2.9.4 or higher**
- Supported Web Server:
  - Apache
  - Nginx
  - LiteSpeed
  - OpenLiteSpeed
- Public IPv4 or IPv6 address
- `SecRuleEngine` must be enabled
- OWASP rules must be **disabled** (incompatible with cPGuard WAF)

:::tip
See [Panel-Specific Steps](panel-specific-steps.md) for control panel setup.
:::

## Enable / Disable WAF

WAF is turned **OFF by default during installation** to avoid conflicts with existing ModSecurity rule sets.

You may enable it anytime after verifying requirements.

![Logo](../../assets/img/cpguard/waf/waf.png)

### Enable WAF

```bash
cpgcli waf --enable
```

Enable with optional modules:

```bash
cpgcli waf --enable=scanner,webshell,captcha,crawler
```

### Disable WAF

```bash
cpgcli waf --disable
```

Disable specific modules:

```bash
cpgcli waf --disable=scanner,webshell,captcha,crawler
```

## Optional Rule Sets

You may enable additional rule modules for enhanced protection.

### RBL Protection

Advanced POST DDoS and brute-force protection.  
Blocks abusive IP addresses collected through the cPGuard distributed network.

### Captcha Protection  (Recommended)

Forces human verification before accessing CMS login pages.

- Protects WordPress / Joomla login
- Reduces brute-force load
- Significantly lowers server resource usage

### Webshell Protection

Blocks execution of common PHP shells such as:

- Phoenix WebShell  
- FilesMan  
- c99  
- b374k  
- WSO  
- Ani-Shell  

The shell interface may load, but command execution (copy, delete, move, etc.) is blocked.

### Scanner Protection (Recommended)

Blocks:

- Malicious user agents
- Aggressive crawlers
- Resource-intensive bots
- Abusive search engine scanners

Helps reduce unnecessary CPU and I/O load.

### Proxy IP Check
A Layer 7 extension that extracts the real visitor IP behind proxies/CDNs, so blocking decisions are made on the true client address instead of the proxy IP.

See [Proxy IP Check](proxy-ip-check.md) for supported headers, usage scenarios, and configuration notes.

## PHP Upload Blocking (WAF Module)

You can prevent PHP file uploads via:

* Web forms
* Web file managers
* Web shells

⚠ When enabled, PHP files cannot be edited through web-based file managers.

### Enable

```bash
cpgcli upload-scanner --block-php=enable
```

### Disable

```bash
cpgcli upload-scanner --block-php=disable
```

## Whitelisting Rules

If a rule causes a false positive:

```bash
cpgcli waf --whitelist --add RULE_ID
cpgcli waf --whitelist --remove RULE_ID
```

Use the Rule ID from WAF logs to whitelist specific triggers instead of disabling modules globally.

## Applying Changes

WAF configuration updates are applied with a short delay. cPGuard can automatically reload ModSecurity and restart the web server when required, so manual restart is usually not needed.

