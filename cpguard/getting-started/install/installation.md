---
title: Install cPGuard
sidebar_position: 1
---

cPGuard uses a **single unified installer** that adapts automatically to your operating system, web server, and control panel. 

---

## Prerequisites

Before running the installer, ensure your server meets these three requirements:

1.  **Root Access:** You must have root (SSH) access to the server.
2.  **License Key:** A valid license key is required to bind the server to your App Portal.
3.  **ModSecurity (For WAF Features):** **ModSecurity 2.9.4 or higher** must be available on the server to use the cPGuard Web Application Firewall.

:::note 
While cPGuard can install its own components automatically, ModSecurity enablement and logging must be verified or enabled manually in most control panels before turning on the WAF.
:::

---

## Choose Your Installation Path

Identify your server setup below to get the correct installation instructions:

### 1. Dedicated Control Panels
If you are using one of these panels,you can follow our dedicated, panel-specific guides:

*   [Install on Webuzo](webuzo.md)
*   [Install on Enhance](enhance.md)
*   [Install on Control Web Panel (CWP)](cwp.md)
*   [Install on CyberPanel](cyberpanel.md)
*   [Install on InterWorx](interworx.md)
*   [Install on Runcloud](runcloud.md)

### 2. cPanel, DirectAdmin, Plesk
cPGuard features deep native integration with **cPanel/WHM, DirectAdmin, and Plesk**. For these panels, simply run the **Universal Installer Command** below. 

Log into your server via SSH as **root** and run the following command (replace `LICENSE-KEY` with your actual cPGuard license key):

```bash
cd /usr/local/src && rm -f cpguard_install.sh && curl -o cpguard_install.sh -L [https://downloads.opsshield.com/cpguard/cpguard_install.sh](https://downloads.opsshield.com/cpguard/cpguard_install.sh) && bash cpguard_install.sh LICENSE-KEY

```

The installer automatically probes your system for the Operating System, Control Panel, and Web Server (Apache, Nginx, OpenLiteSpeed, or LiteSpeed). If it recognizes a native panel, it completes the installation silently. If it requires more information, it will prompt you via an interactive CLI wizard.

### 3. Standalone Servers & Unsupported Panels
If your server does not use a control panel, or uses an unlisted/custom panel, the installer will automatically launch an **interactive configuration wizard** to collect your environment details.
*   👉 See the [Standalone Overview & Installation](../../standalone/overview) for the install flow and [Standalone Configuration Reference](../../standalone/configuration) for the parameter details.

---

## Post-Installation Steps

After successfully running the installer, there are a few important steps to complete your setup and ensure cPGuard can communicate and operate correctly:

### 1. Whitelist App Portal IPs in Your Firewall
To allow cPGuard to communicate with the centralized App Portal, you must whitelist the App Portal IP addresses in your server-level and cloud-level firewalls for TCP port `9098`. 

Please refer to the [App Portal IPs List](../../../app-portal/errors.md#app-portal-ips-must-be-whitelisted) for the specific IP addresses to whitelist, as well as directions for cloud firewalls (like AWS, DigitalOcean, Linode, etc.).

### 2. Initial Configuration
To prevent unintended blocks or configuration conflicts on startup, some modules are disabled by default. You can configure and enable modules and services by following our [Initial Configuration Guide](../configuration.md).

### 3. Introducing the `cpgcli` Command-Line Interface
cPGuard comes with a powerful CLI tool, `cpgcli`, which allows you to manage scanner processes, configure modules, check status, and perform administrative operations directly from your terminal.


To view all available commands and options, run:

```bash
cpgcli --help
```

For a comprehensive commands reference, refer to the [cpgcli documentation](../../cpgcli/overview.md).
