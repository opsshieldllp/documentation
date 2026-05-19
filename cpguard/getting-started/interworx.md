---
slug: cpguard-interworx-control-panel-security
title: Secure Your InterWorx Control Panel with cPGuard
sidebar_label: Install on InterWorx Control Panel
sidebar_position: 9
authors: [your-name]
tags: [cpguard, interworx, nodeworx, siteworx, standalone, modsecurity, waf, opsshield, security]
date: 2023-10-02
description: Step-by-step guide to installing and configuring cPGuard on an InterWorx server — including installation, automatic and manual standalone configuration, and App Portal setup.
---


## What Is InterWorx Control Panel?

InterWorx is a web hosting control panel built around two separate interfaces:

- **NodeWorx** : used by server administrators to manage the server, its daemons, and configurations. It also supports reseller functionality, allowing resellers to manage multiple SiteWorx accounts without granting them access to server-level settings.
- **SiteWorx** : used by individual website owners to manage their specific website, email, databases, and files.

This dual-interface design makes InterWorx a strong choice for hosting providers and resellers who need clear role separation between server admins, resellers, and end users.

---

## How cPGuard Protects InterWorx Servers

cPGuard integrates with InterWorx through the **Standalone** configuration pathway, protecting the server across multiple security layers:

- **Malware Scanner** — continuously monitors website directories for malicious files
- **Web Application Firewall (WAF)** — blocks common web attacks through ModSecurity rules
- **Automatic File Cleanup** — removes injected malicious code from PHP and JS files
- **IP Reputation & Country Blocking** — filters traffic from known-bad IPs and blocked regions
- **Email Notifications** — alerts on detections with daily summary reports

---

## Step 1 : Install cPGuard on InterWorx

Run the following command on your InterWorx server as root to download and execute the cPGuard installer:

```bash
cd /usr/local/src && rm -f cpguard_install.sh && curl -o cpguard_install.sh -L https://downloads.opsshield.com/cpguard/cpguard_install.sh && bash cpguard_install.sh LICENCE-KEY
```

Replace `LICENCE-KEY` with your actual cPGuard license key from OPSSHIELD.

**What the installer does:**

1. Downloads the latest cPGuard installer script
2. Installs all required dependency packages for your operating system
3. Applies the license key and binds the server to your **cPGuard App Portal** account
4. Displays a success message with the App Portal link for your server (e.g. `https://app.opsshield.com/server/0001`)

:::note
The license key is **mandatory**. Without it, installation cannot be completed and the server will not appear in the App Portal.
:::

---

## Step 2 : Web Server & WAF Configuration

After the dependency packages are installed, cPGuard proceeds with the server configuration. The configuration covers two key areas:

- **Web Server Configuration** — detects where your InterWorx virtual host configuration files are located
- **WAF Configuration** — connects cPGuard's ModSecurity rule set to your web server's ModSecurity instance

In most cases, cPGuard will **automatically detect and configure** the correct settings for an InterWorx server without requiring any manual input. Once the installer completes with a success message, your server is ready to manage from the App Portal.

:::tip
If the automatic configuration completes successfully, you can skip ahead to Step 4 and start managing cPGuard from the App Portal immediately. The standalone wizard in Step 3 is only needed if you want to customise the configuration.
:::

---

## Step 3 : Optional: Manual Standalone Configuration

If you want to **manually provide configuration values** or customise how cPGuard fetches details from your InterWorx server, run the standalone configuration wizard after installation:

```bash
cpgcli standalone-conf --wizard
```

This launches an interactive wizard that prompts you for:

- Web server type and virtual host config path
- WAF settings and ModSecurity include path
- User list source
- Suspend hook script path

:::note
This step is **only required** if you need a custom configuration. The automatic detection during installation handles this correctly for most standard InterWorx setups.
:::

---

## Step 4 : Manage cPGuard from the App Portal

Once installation is complete, log in to the cPGuard App Portal using your OPSSHIELD client area credentials:

[https://app.opsshield.com](https://app.opsshield.com)

From the server dashboard, you can:

- Navigate to **Settings** to configure WAF, scanner, notifications, and more
- View **Scanner Logs** to review detected threats and file actions taken
- Access **WAF Logs** to see blocked requests in real time
- Check **Stats** for a security overview of the server
- Manage **Country Blocking** and **IP Reputation** rules

---


## Installation Checklist

| Step | Task | Status |
|---|---|---|
| 1 | Run the cPGuard installer with your license key | ☐ |
| 2 | Confirm dependency packages install successfully | ☐ |
| 3 | Confirm Web Server and WAF auto-configured successfully | ☐ |
| 4 | Confirm success message and App Portal server link | ☐ |
| 5 | Log in to the App Portal and verify the server appears | ☐ |
| 6 | Configure WAF, scanner, and notification settings | ☐ |
| 7 | (Optional) Run `cpgcli standalone-conf --wizard` for custom config | ☐ |

---

