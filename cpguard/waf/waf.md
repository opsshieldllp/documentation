---
slug: cpguard-waf-integration-cli
title: cPGuard WAF Integration
authors: [your-name]
tags: [cpguard, waf, cli, command-line, modsecurity, whitelist, opsshield, web-application-firewall]
date: 2024-06-30
description: Learn how to enable and disable the cPGuard Web Application Firewall, toggle optional WAF modules, and manage rule whitelists — all from the command line using cpgcli.
---

# cPGuard WAF Integration: Enable, Configure & Whitelist Rules via CLI

The **Web Application Firewall (WAF)** integrated into cPGuard is one of its most powerful features for defending against web-based attacks. Its rules are carefully crafted, regularly updated, and fully tested across all major web servers making it a reliable frontline defence for your hosted applications.

This guide covers how to manage WAF settings entirely from the command line using the `cpgcli` utility.

{/* comment */}

---

## Why WAF Is Disabled by Default

When cPGuard is first installed, the WAF is **turned off by default**. This is an intentional design decision. Many server administrators already have their own WAF rule sets or ModSecurity configurations in place. Enabling cPGuard's WAF on top of existing rules without review could cause conflicts.

You can safely enable it at any time once you have reviewed your server's existing configuration.

:::note
Before enabling WAF, make sure your server meets all the prerequisites. Refer to the [WAF Requirements guide](../waf/cpguard-waf-required-settings-and-depencies.md) before proceeding.
:::

---

## Supported Web Servers

The cPGuard WAF rules are fully compatible with all of the following web servers:

- **Apache**
- **LiteSpeed**
- **OpenLiteSpeed**
- **Nginx**

---

## Enable / Disable WAF

To turn the WAF on or off globally on your server:

```bash
# Enable WAF
cpgcli waf --enable

# Disable WAF
cpgcli waf --disable
```

:::tip
You can also manage a wider range of WAF settings from the **cPGuard App Portal UI**. Open your server, then navigate to **Settings → WAF**.
:::

---

## Enable / Disable Optional WAF Modules

Beyond the core rule set, cPGuard WAF includes several optional modules that can be enabled or disabled independently. You can control them by passing a comma-separated list of module names:

```bash
# Enable specific optional modules
cpgcli waf --enable=scanner,webshell,capthca,crawler

# Disable specific optional modules
cpgcli waf --disable=scanner,webshell,capthca,crawler
```

### Available Optional Modules

| Module | What It Does |
|---|---|
| `scanner` | Blocks bad crawlers and malicious scanning tools |
| `webshell` | Prevents PHP web shell command execution |
| `capthca` | Enforces CAPTCHA verification on CMS login pages |
| `crawler` | Blocks abusive and fake search engine crawlers |

:::tip
You can mix and match modules freely. For example, to enable only CAPTCHA and scanner protection without touching the other modules:

```bash
cpgcli waf --enable=capthca,scanner
```
:::

---

## Whitelisting WAF Rules

Even well-tuned WAF rules can occasionally produce **false positives**. Legitimate requests that are incorrectly flagged and blocked. When this happens, you can whitelist specific rule IDs to exempt them from enforcement.

Rule IDs can be found in:
- The **WAF Logs** section of the cPGuard App Portal UI
- Your web server's **ModSecurity logs**

### Add a Rule to the Whitelist

```bash
waf --whitelist --add 4500006
```

### Remove a Rule from the Whitelist

```bash
waf --whitelist --remove 4500006
```

:::note
WAF configuration changes are **not applied instantly**. There is a brief time delay before updates take effect, and the change is typically followed by a **web server restart**. Plan accordingly if applying changes on a live production server.
:::


---
