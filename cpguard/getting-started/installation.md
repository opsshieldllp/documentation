---
title: cPGuard Installation
sidebar_position: 2
---

cPGuard uses a **single unified installer** for all environments. The installer adapts automatically based on your server environment and control panel.

It supports **automatic installation with deep integration** for major control panels, while also offering **flexible installation modes** for partially supported and standalone environments.

---

## Prerequisites

Before starting the installation, ensure the following:

1. **Root Access**  
   You must have root (SSH) access to the server.

2. **License Key**  
   A valid license key is required to complete installation and bind the server to the App Portal.

3. **ModSecurity (for WAF features)**  
   To use the cPGuard Web Application Firewall (WAF), **ModSecurity 2.9.4 or higher** must be available on the server.

   :::note
   cPGuard can install and configure itself automatically,  
   **but ModSecurity enablement and logging must be verified or enabled manually** in most control panels.
   :::

   → See: [WAF Configuration Guide](../waf/overview.md)

---

## Universal Installer Command

Run the following command as **root**, replacing `LICENSE-KEY` with your actual key:

```bash
cd /usr/local/src && rm -f cpguard_install.sh && curl -o cpguard_install.sh -L https://downloads.opsshield.com/cpguard/cpguard_install.sh && bash cpguard_install.sh LICENSE-KEY
```

### What Happens During Installation

- The installer attempts to detect:
  - Operating system
  - Control panel (if any)
  - Web server (Apache / Nginx / OpenLiteSpeed / LiteSpeed)

- If the environment is fully supported or details are available installation proceeds automatically
- If additional input is required, an **interactive configuration wizard** is launched

---

## Get Started here 

If you are using one of the following control panels, start with the dedicated guide:


- [Install on Webuzo](../getting-started/cpguard-webuzo-security-antivirus-antimalware)
- [Install on Enhance](../getting-started/Installing-cPGuard-on-Enhance-Control-Panel)
- [Install on Control Web Panel](../getting-started/secure-cwp-cpguard)
- [Install on Cyber Panel](../getting-started/cpguard-cyberpanel-security-addon)
- [Install on InterWorx](../getting-started/cpguard-interworx-control-panel-security)
- [Install on Runcloud](../getting-started/cpguard-runcloud-security)


---


### Quick setup on popular Control panels

The following control panels have **native integration** with cPGuard:

- cPanel / WHM
- DirectAdmin
- Plesk

#### What cPGuard Detects & Configures Automatically

When installed on a supported control panel, cPGuard automatically detects and configures the following — no manual setup needed:

- **Control panel** — Identifies the panel type and adjusts settings accordingly
- **Web server paths** — Locates Apache, Nginx, or LiteSpeed paths automatically
- **Service integration** — Hooks into control panel services without manual configuration
- **Malware scanner watchlist** — Pre-populates paths to monitor based on the detected panel

---

## WAF Requires Manual Review Before Enabling (Important)

WAF dependencies vary depending on the control panel environment. Before enabling WAF, ensure the required dependencies are installed and properly configured for your specific control panel.

:::warning 
**Skipping this step may cause WAF to malfunction or fail to load entirely.** 
:::

Check about WAF and WAF dependencies and configuration requirements for your control panel before proceeding:  
[WAF Overview](../waf/overview.md)

[cPGuard WAF Requirements and Dependencies](../waf/cpguard-waf-required-settings-and-depencies)

---

### Standalone servers/ Unsupported Control Panels

Standalone installation applies to:

- Servers without a control panel
- Unsupported or less-known control panels
- Environments where automatic integration is not possible

In this mode, cPGuard runs an **interactive configuration wizard** to collect environment details.

[Standalone Installation](../getting-started/standalone-configuration)

---

## Post-Installation Configuration

After installation, cPGuard enables most recommended modules automatically.
However, the following modules are **disabled by default** as they require manual configuration based on your server environment:

- **WAF** (Web Application Firewall)
- **Firewall**
- **OSM** (Outgoing Spam Monitor)

---

#### WAF — Web Application Firewall

WAF is disabled by default because it depends on **ModSecurity**, which must be installed and configured before enabling WAF.

:::warning
WAF dependencies differ per control panel. Configure them properly before enabling WAF or it may fail to load.
:::

check about WAF and WAF dependencies and configuration requirements for your control panel before proceeding:  
[WAF Overview](../waf/overview.md)

[cPGuard WAF Requirements and Dependencies](../waf/cpguard-waf-required-settings-and-depencies)

---

#### Firewall

The firewall module is disabled by default and must be configured based on your server's specific setup.

Please refer [Firewall Setup](../firewall/overview) for full firewall configuration

---

#### OSM — Outgoing Spam Monitor

OSM is disabled by default. Configure and enable it once your server is ready.

For OSM configuration: [OSM Setup](../general/osm)

---

#### Enabling & Configuring via UI and CLI

Once dependencies are met, you can enable and configure all modules from:

- **UI** — App Portal settings page
- **CLI** — using `cpgcli` commands

UI & CLI configuration guide: [Configuration Guide](../getting-started/configuration) 


