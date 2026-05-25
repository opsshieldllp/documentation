---
sidebar_position: 1
title: License, Setup & Config
description: A comprehensive guide to managing your cPGuard license, server setup, config import/export, email notifications, and platform integration command-line controls.
tags: [cpguard, cpgcli, license, cloud, update, api, integration, setup, config, notification]
---

# License, Setup & Config

This page covers server onboarding, license activation, system updates, and control panel migrations. Using the `cpgcli` platform tools, you can automate license registration, secure root credentials, provision configurations, and integrate third-party billing or hosting interfaces.

---

## License Management

Managing your cPGuard license is handled entirely through the `license` command. This is essential during initial installations, server migrations, or automated bare-metal provisioning.

### Check License Status
Query active status, expiration date, and local cache details:
```bash
cpgcli license --status
```

### Apply License Key
Bind the server to your OPSSHIELD license key and sync with the App Portal (replace `LICENSE-KEY` with your actual key):
```bash
cpgcli license --key LICENSE-KEY
```

### Renew License Data
Force a sync or pull of fresh license metadata from OPSSHIELD servers:
```bash
cpgcli license --renew
```

### Remove License Key
Delete local activation payload and unbind the server from the App Portal:
```bash
cpgcli license --remove
```

:::warning
Using `cpgcli license --remove` will completely **delete the server profile, system history, and active policies from the cPGuard App Portal**. Only run this if decommissioning or repurposing the instance.
:::

---

## Configuration Management

Replicate security settings across your cluster easily by importing or exporting core `.ini` baselines. 

### View Active Configuration
View active configuration parameters in a key-value structure:
```bash
cpgcli config
```
Or use the show option:
```bash
cpgcli config --show
```

### Export Configuration
Export configuration to default file path `/opt/cpguard/tmp/config.ini`:
```bash
cpgcli config --export
```
Or export to a custom file path:
```bash
cpgcli config --export /root/custom-cpguard.ini
```

### Import Configuration
Import configuration from a local file path:
```bash
cpgcli config --import /root/custom-cpguard.ini
```
Or import configuration dynamically from a secure URL:
```bash
cpgcli config --import 'https://internal.secops.net/cpguard/baseline.ini'
```


---

## Cloud Connectivity & Status

Allows you to verify the server's backend connection to the OPSSHIELD cloud nodes and automatically configure firewall exemptions so your services are never interrupted.

### View Cloud Status
Get a basic cloud status overview:
```bash
cpgcli cloud
```

### Check Connection
Verify the connection and latency to cPGuard secure backend nodes:
```bash
cpgcli cloud --status
```

### Whitelist Cloud Services
Automatically whitelist cPGuard cloud IP blocks in the system firewall (such as CSF or nftables):
```bash
cpgcli cloud --whitelist
```

---

## Software Updates

Use the software updater module to inspect new packages, download fresh definitions, or trigger rolling engine patches.

### Check For Updates
Check if a newer application version or threat signatures release is available:
```bash
cpgcli update --check
```

### Start Update
Run software installation or trigger signature updates manually:
```bash
cpgcli update --start
```

---

## Support Access Delegation

Securely grant or revoke temporary root-level shell access to the OPSSHIELD support team for remote troubleshooting.

### Check Status
Check active hours or verify if any support sessions are open:
```bash
cpgcli support-access
```
Or show status explicitly:
```bash
cpgcli support-access --status
```

### Grant Support Access
Create and grant secure support credentials for OPSSHIELD technicians:
```bash
cpgcli support-access --grant
```

### Revoke Support Access
Revoke all support session keys immediately and lock down SSH access:
```bash
cpgcli support-access --revoke
```

---

## Local API Keys

Manage authentication tokens for the local HTTP daemon, enabling custom provisioning portals or internal monitoring frameworks to query local states securely.

### List API Keys
List all active local API keys and their timestamps:
```bash
cpgcli api --list-keys
```

### Generate API Key
Generate a new cryptographic API key:
```bash
cpgcli api --generate
```

### Delete API Key
Delete a specific API key to revoke access immediately (replace `your-api-key` with the target key):
```bash
cpgcli api --delete your-api-key
```

---

## Standalone Settings

Used to establish configuration profiles when cPGuard is installed on servers without supported management interfaces (e.g., plain Linux instances or Webmin integration environments).

### Interactive Wizard
Run the interactive onboarding standalone configuration wizard:
```bash
cpgcli standalone-conf --wizard
```

### WAF Integration Setup
Deploy custom configurations specifically for standalone WAF integration:
```bash
cpgcli standalone-conf --waf
```

### Update Apply Check
Commit and force apply values changed in `/opt/cpguard/cpguard.ini`:
```bash
cpgcli standalone-conf --update
```

### Reset Profile
Reset all active standalone profiles back to standard control panel integration defaults:
```bash
cpgcli standalone-conf --reset
```

---

## Panel and User Integrations

Configure the level of visibility and control granted to standard/non-root clients logged into control panels like cPanel directly from your terminal.

### View Settings
View current userpanel status policies:
```bash
cpgcli panel-integration
```

### Admin SSO Configuration
Enable admin Single Sign-On (SSO):
```bash
cpgcli panel-integration --admin-sso enable
```
Disable admin Single Sign-On (SSO):
```bash
cpgcli panel-integration --admin-sso disable
```

### User Manual Scan Control
Allow users to trigger manual scans from user panels:
```bash
cpgcli panel-integration --user-scan enable
```
Disable user-panel manual scans:
```bash
cpgcli panel-integration --user-scan disable
```

---

## UI Access Control

Explicitly define administrative emails permitted to access the cPGuard graphical user interface backend.

### Add Authorized Email
Add a registered email address to authorize login permission:
```bash
cpgcli ui-users --add manager@example.com
```

### Remove Authorized Email
Revoke UI session permissions immediately:
```bash
cpgcli ui-users --remove manager@example.com
```

---

## Branding Controls

Provide customized White-Label setups to tailor the branding of the cPGuard user plugin to align with your organization’s identity.

### Custom Branding
Launch interactive prompts to change names, icons, and logo assets (cPanel Only):
```bash
cpgcli branding
```

---

## SMTP & Email Notifications

Configure administration and customer alerting profiles.

### Manage Notification Baselines

View current email configuration settings:
```bash
cpgcli notification
```

Enable all email notifications completely:
```bash
cpgcli notification --enable
```

Disable all email notifications completely:
```bash
cpgcli notification --disable
```

Set primary admin notification address:
```bash
cpgcli notification --primary-email admin@example.com
```

Set secondary CC notification address:
```bash
cpgcli notification --secondary-email secops@example.com
```

### Configure Mail-Delivery Relays

Inspect active mailing methods:
```bash
cpgcli notification --method
```

Relay emails locally using the server's default MTA (e.g. Exim, Postfix):
```bash
cpgcli notification --method local
```

Connect directly to an external SMTP account:
```bash
cpgcli notification --method smtp
```
*(Provide the SMTP credentials and host details when prompted indeed)*

### Filter Administrative Alerting Categories

Selective enables (supported categories: `virus`, `suspicious`, `binary`, `iprep`, `daily_report`):
```bash
cpgcli notification --enable virus,suspicious,binary,iprep,daily_report
```

Turn off specific administration email categories:
```bash
cpgcli notification --disable daily_report
```

### Configure End-User Control Panel Alerts

Define email alerts pushed directly to regular panel target accounts (supported options: `infected_files`, `account_suspension`, `wp_autoupdate`):
```bash
cpgcli notification --enable-user infected_files,account_suspension
```

Turn off user-level options:
```bash
cpgcli notification --disable-user wp_autoupdate
```

Configure WordPress and universal CMS threat summary email frequency pushed to users:
```bash
cpgcli notification --cms-threats-user daily
```
*(Acceptable parameters: `daily`, `weekly`, `monthly`, `never`)*

Specify users to exclude completely from notifications:
```bash
cpgcli notification --excluded-users user1,user2
```

### Validate Email Deliverability

Verify connection and send a test mock template:
```bash
cpgcli notification --test
```
