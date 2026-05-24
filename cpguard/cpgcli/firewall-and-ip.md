---
sidebar_position: 9
title: Firewall & IP Management
description: Manage firewall state, country and port controls, IP lists, temporary bans, source feeds, and DDNS entries.
tags: [cpguard, cpgcli, firewall, ip, fail2ban]
---

# Firewall & IP Management

Govern incoming and outgoing network traffic, manage blocking feeds, custom port rules, and country locks using cPGuard CLI tools.

---

## Firewall Controls

### Manage Firewall Service State

Get firewall daemon information:
```bash
cpgcli fw
```

Or check status explicitly:
```bash
cpgcli fw --status
```

Enable system firewall integration:
```bash
cpgcli fw --enable
```

Disable system firewall:
```bash
cpgcli fw --disable
```

Restart firewall engine to flush and reload tables:
```bash
cpgcli fw --restart
```

Switch active backend provider (e.g. `nftables` or `iptables` rules):
```bash
cpgcli fw --provider nftables
```

### Toggle Security Integrations & Debug Modes

Configure CAPTCHA verification layer:
```bash
cpgcli fw --captcha enable
```

Disable Captcha:
```bash
cpgcli fw --captcha disable
```

Configure IP Reputation Threat database matches:
```bash
cpgcli fw --ipdb enable
```

Disable IPDB integration:
```bash
cpgcli fw --ipdb disable
```

Toggle detailed debug levels to trace packet actions inside `/var/log` files:
```bash
cpgcli fw --debug
```

### Manage Fail2Ban Configurations

Enable/Disable Fail2Ban integration completely:
```bash
cpgcli fw --fail2ban enable
```

Check active jail status parameters:
```bash
cpgcli fw --fail2ban status
```

Force apply configuration files to custom jails:
```bash
cpgcli fw --fail2ban --enable-jail ssh-jail,postfix-jail
```

### Lock / Whitelist Countries

Filter network requests by geographical region ISO codes (e.g., `US`, `CN`, `IN`):

Whitelist a specific country completely:
```bash
cpgcli fw --allow-country US
```

Remove country from allowed whitelist:
```bash
cpgcli fw --allow-country --remove US
```

List all whitelisted country configurations:
```bash
cpgcli fw --allow-country --list
```

Blacklist a country entirely:
```bash
cpgcli fw --deny-country CN
```

Remove a country from the blacklisted registry:
```bash
cpgcli fw --deny-country --remove CN
```

List blacklisted countries:
```bash
cpgcli fw --deny-country --list
```

### Port Filtering Controls

:::warning
Enabling Port Filtering will **discard packets across all UDP/TCP interfaces** except for port numbers explicitly whitelisted below.
:::

Check active port-filter protection rules status:
```bash
cpgcli fw --port-filter status
```

Enable global Port Filter rules:
```bash
cpgcli fw --port-filter enable
```

Disable global Port Filter rules:
```bash
cpgcli fw --port-filter disable
```

Whitelisting allowed ports:
```bash
cpgcli fw --port tcp-in --add 22
```

Check all whitelisted inbound TCP rules:
```bash
cpgcli fw --port tcp-in --list
```

Remove an inbound port rule:
```bash
cpgcli fw --port tcp-in --remove 22
```

Configure ranges or protocols (e.g., UDP output):
```bash
cpgcli fw --port udp-out --add 80-90
```

---

## IP Management

Check database parameters or assign target addresses, CIDR blocks, or external web feeds to white/blacklists.

### Probe IP Registration Space

Check if a given host or client IP is registered in any active filter state (e.g. whitelist, temp-ban):
```bash
cpgcli ip --check 10.0.0.1
```

### Access Lists Whitelist

Whitelist a static server or employee CIDR range:
```bash
cpgcli ip --allow 10.0.0.1 --reason 'Office Gateway'
```

Remove from the whitelist:
```bash
cpgcli ip --allow --remove 10.0.0.1
```

List current whitelisted IPs:
```bash
cpgcli ip --allow --list
```

### Access Lists Blacklist

Block custom attacker address spaces permanently:
```bash
cpgcli ip --deny 203.0.113.0/24 --reason 'Abuse Node'
```

Remove from blacklisted registry:
```bash
cpgcli ip --deny --remove 203.0.113.0/24
```

List blocked IP resources:
```bash
cpgcli ip --deny --list
```

### Temporary Rules (Auto-Expires)

Whitelist employees or API proxies temporarily:
```bash
cpgcli ip --temp-allow 10.0.0.1 --expiry 2h --reason 'Maintenance session'
```

Remove temporary allowances:
```bash
cpgcli ip --temp-allow --remove 10.0.0.1
```

Ban hostile attackers temporarily (supports `m` (minutes), `h` (hours), `d` (days)):
```bash
cpgcli ip --temp-ban 10.0.0.2 --expiry 30m --reason 'Brute Force attempt'
```

List current temporarily banned hosts:
```bash
cpgcli ip --temp-ban --list
```

### Synchronize Remote Feeds & DDNS

Feed dynamic TXT files or URL blocks straight into lists:
```bash
cpgcli ip --allow-source https://trusted-cdn.net/approved_ips.txt
```

Remove remote feed integration:
```bash
cpgcli ip --allow-source --remove https://trusted-cdn.net/approved_ips.txt
```

Register Dynamic DNS hostname mapping endpoints:
```bash
cpgcli ip --ddns my-office.example.net
```

List ongoing dynamic DNS mappings:
```bash
cpgcli ip --ddns --list
```
