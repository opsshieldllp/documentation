---
sidebar_position: 3
sidebar_label: Webuzo
title: Installing on Webuzo

description: Learn how to secure your Webuzo server using cPGuard — including the required ModSecurity pre-configuration, installation steps, App Portal setup, and optional standalone configuration via CLI.
---

**Webuzo** is a versatile multi-user control panel for managing cloud and dedicated servers covering domains, emails, websites, databases, and web application deployments from a single interface. Despite its feature-rich environment, Webuzo doesn't ship with a built-in malware scanner or a fully configured WAF out of the box.

**cPGuard** fills that gap, providing multi-layered web security for Webuzo servers acting as the antivirus, antimalware, and firewall layer that Webuzo doesn't include natively.

{/* comment */}

---

## What Is Webuzo?

Webuzo is a multi-user web hosting control panel designed for cloud and dedicated server management. It simplifies the administration of:

- Domains and DNS
- Email accounts
- Website file management
- Databases
- Web application deployments (via its one-click app installer)

It functions both as an end-user hosting panel and as a server management tool making it popular among hosting providers and developers who want a lightweight, flexible alternative to cPanel or Plesk.

---

## How cPGuard Protects Webuzo Servers

cPGuard integrates with Webuzo through its **standard installation procedure**, protecting the server across multiple layers:

- **Malware Scanner** — continuously monitors website directories for threats
- **Web Application Firewall (WAF)** — blocks web attacks through ModSecurity rules
- **Automatic File Cleanup** — removes injected malicious code from PHP and JS files
- **IP Reputation & Country Blocking** — filters abusive and geo-targeted traffic
- **Single Sign-On (SSO)** — allows Webuzo admins to jump into the cPGuard App Portal without separate login credentials

---

## Important: Pre-Installation ModSecurity Setup

:::danger
**Do not skip this step.** Webuzo does **not** enable ModSecurity by default. If you install cPGuard before completing the ModSecurity pre-configuration, the WAF will not function correctly.

You must complete the following steps **before** running the cPGuard installer:

1. Install **OWASP vendor rules** in Webuzo.
2. **Disable** the OWASP vendor rules from within Webuzo (cPGuard will manage its own rule set — having both active causes conflicts).

Full instructions are in the dedicated guide under the **"Settings required for Webuzo Control Panel"** section:

[cPGuard WAF Panel-Specific Steps](../../waf/panel-specific-steps)
:::

Once the ModSecurity pre-configuration is complete, proceed with the cPGuard installation below.

---

## Step 1 : Install cPGuard on Webuzo

Run the following command on your Webuzo server as root to download and execute the cPGuard installer:

```bash
cd /usr/local/src && rm -f cpguard_install.sh && curl -o cpguard_install.sh -L https://downloads.opsshield.com/cpguard/cpguard_install.sh && bash cpguard_install.sh LICENCE-KEY
```

Replace `LICENCE-KEY` with your actual cPGuard license key purchased from OPSSHIELD.

**What the installer does:**

1. Downloads the latest cPGuard installer script
2. Installs all required dependency packages for your operating system
3. Applies the license key and binds the server to your **cPGuard App Portal** account
4. Displays the App Portal login URL for your server (e.g. `https://app.opsshield.com/server/0001`)

:::note
The license key is **mandatory**. Without it, installation cannot be completed and the server will not be bound to the App Portal.
:::

---

## Step 2 : Access the App Portal

Once installation completes, log in to the cPGuard App Portal using your **OPSSHIELD client area credentials**:

[https://app.opsshield.com](https://app.opsshield.com)

From the server dashboard inside the App Portal, you can:

- Navigate to **Settings** to configure all cPGuard modules
- View **Scanner Logs** to see detected threats and actions taken
- Access **WAF Logs** to monitor blocked requests
- Check **Stats** for an overview of server security activity
- Enable and manage **Country Blocking**, **IP Reputation**, and more

:::tip
Webuzo also supports **Single Sign-On (SSO)** with cPGuard — meaning Webuzo admins can navigate directly to the cPGuard App Portal from within Webuzo without entering separate login credentials. See the [SSO guide](../../../app-portal/sso.md) for setup details.
:::

---

## Step 3 : Optional: Manual Standalone Configuration

In most cases, cPGuard automatically detects the correct configuration for Webuzo during installation. However, if you want to **manually customise the configuration** or explicitly set cPGuard to standalone mode for your Webuzo server, run the following command after installation:

```bash
cpgcli standalone-conf --wizard
```

This launches an interactive wizard that guides you through providing the server details manually including the web server type, virtual host configuration paths, WAF settings, user list source, and suspend hook.

:::note
The standalone configuration wizard is **only needed if you want to customise** cPGuard's configuration beyond the automatic defaults. Most Webuzo installations will work correctly without this step.
:::



---

## Installation Checklist

| Step | Task | Status |
|---|---|---|
| Pre-install | Install OWASP vendor rules in Webuzo | ☐ |
| Pre-install | Disable OWASP vendor rules in Webuzo | ☐ |
| 1 | Run cPGuard installer with license key | ☐ |
| 2 | Confirm successful installation and App Portal URL | ☐ |
| 3 | Log in to App Portal with OPSSHIELD client credentials | ☐ |
| 4 | Configure WAF, scanner, and notifications in Settings | ☐ |
| 5 | (Optional) Run `cpgcli standalone-conf --wizard` if needed | ☐ |

---
