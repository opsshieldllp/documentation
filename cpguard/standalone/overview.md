---
sidebar_position: 1
sidebar_label: Overview & Installation
title: Standalone Overview & Installation
description: What standalone means in cPGuard, what information you need to provide, and how to install it.
---

# Standalone Overview & Installation

## What "Standalone" Means

cPGuard has built-in profiles for supported control panels and environments where it already knows how to collect the details it needs, such as domains, document roots, users, and WAF integration points.

**Standalone** is used when cPGuard does **not** know where to look for those details automatically. This usually applies to:

- custom or unsupported control panels
- bare Linux servers without a control panel
- custom web server layouts
- mixed or reverse-proxy setups where hosting and WAF are split

In standalone mode, the customer provides the required data directly using `cpguard.ini`, a JSON file, or a script that outputs JSON in the required format.

:::note
Standalone does not mean reduced functionality. It means cPGuard needs you to point it to the right files or provide the data explicitly.
:::

---

## What Information cPGuard Needs

cPGuard needs two groups of information.

### Website data

This is used to build the malware scanner watchlist and related mappings:

- domain names
- document roots
- account/system users

You can provide this in either of these ways:

1. **Web server parsing** using `web_server` and `web_server_conf`
2. **Explicit data source** using `domain_list`, which points to a JSON file or executable script

If `domain_list` is set, it takes priority over `web_server` and `web_server_conf`.

### WAF data

This is used to load cPGuard WAF rules into ModSecurity and collect WAF events:

- the web server where ModSecurity runs
- the include file path where cPGuard should write its WAF configuration
- the restart command for that server
- the audit log path
- the server error log path

The hosting web server and the WAF server can be different. For example, in an Nginx + Apache reverse-proxy setup, cPGuard may parse Nginx virtual hosts for domains while loading ModSecurity rules into Apache.

:::info
WAF settings should be treated as a complete block. If you want to enable WAF, provide all WAF-related values together.
:::

---

## Installation

The cPGuard installer is universal. It detects the operating system and tries to identify the hosting environment. If it cannot use a built-in profile, it switches to standalone mode and asks for the required values.

Run the installer as **root** over SSH:

```bash
cd /usr/local/src && rm -f cpguard_install.sh && \
curl -o cpguard_install.sh -L https://downloads.opsshield.com/cpguard/cpguard_install.sh && \
bash cpguard_install.sh LICENSE-KEY
```

### Interactive install

If you do not provide a prebuilt config file, the installer prompts for the standalone values it needs. This is the normal path for one-off installations.

### Unattended install

For automation, prepare a standalone `cpguard.ini` in advance and pass its path as the second argument:

```bash
bash cpguard_install.sh LICENSE-KEY /path/to/cpguard.ini
```

The file is copied to `/opt/cpguard/cpguard.ini` during installation.

---

## After Installation

Standalone settings are stored in:

```text
/opt/cpguard/cpguard.ini
```

You can modify the file later and apply the changes with:

```bash
cpgcli standalone-conf --update
```

Or reopen the interactive wizard with:

```bash
cpgcli standalone-conf --wizard
```

For the full parameter reference, JSON formats, priorities, and examples, see [Standalone Configuration Reference](./instalation).

