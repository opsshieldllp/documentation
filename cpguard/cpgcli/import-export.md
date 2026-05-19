---
slug: cpguard-import-export-configuration
title: How to Import and Export cPGuard Configuration
authors: [your-name]
tags: [cpguard, cli, configuration, import, export, opsshield, server-management, automation]
date: 2024-06-07
description: Learn how to export your cPGuard configuration from one server and import it to another — perfect for maintaining a consistent security setup across multiple servers.
---

# How to Import and Export cPGuard Configuration

When managing a fleet of servers, maintaining a **consistent security configuration** across all of them is critical. Manually replicating settings on each server is time-consuming and error-prone. cPGuard solves this with a built-in **export and import** feature that lets you capture the full configuration from one server and apply it to any number of others even via a URL for full automation.

{/* comment */}

---

## Why Use Config Export & Import?

The configuration export/import workflow is particularly useful in the following scenarios:

- **New server provisioning** — apply a known-good configuration immediately after installing cPGuard
- **Fleet consistency** — ensure all servers share the same scanner settings, file actions, WAF rules, and notification preferences
- **Disaster recovery** — keep a backup of your configuration and restore it quickly if needed
- **Configuration auditing** — export the current state to review or document your security settings

:::note
Configuration export and import is a **CLI-only** feature. There is no option in the cPGuard App Portal UI to perform this action. All operations are done via the `cpgcli config` command.
:::

---

## Getting Help

To view the full usage details for the config command at any time:

```bash
cpgcli config --help
```

This will display all available options and their descriptions directly in your terminal.

---

## View the Current Configuration

Before exporting, you may want to review the current configuration settings on your server. Use either of the following:

```bash
# View all current configuration (default behaviour)
cpgcli config

# Explicitly view all current configuration
cpgcli config --show
```

Both commands display the full set of currently active cPGuard settings on the server.

---

## Exporting the Configuration

To export your cPGuard configuration to a file:

```bash
cpgcli config --export
```

By default, if no file path is specified, the configuration is saved to:

```
/opt/cpguard/tmp/config.ini
```

To export to a **custom location**, specify the full file path:

```bash
cpgcli config --export /root/my-cpguard-config.ini
```

:::tip
Save your exported configuration to a location that is easy to access remotely such as a web server, a shared storage location, or an internal configuration management system. This makes it straightforward to import it on multiple servers via URL (see below).
:::

---

## Importing the Configuration

To import a configuration onto a server, use:

```bash
cpgcli config --import 'file path or URL'
```

Replace `'file path or URL'` with either:

- The **local path** to the configuration file on the server
- A **URL** pointing to a remotely hosted configuration file

### Import from a Local File

```bash
cpgcli config --import '/root/my-cpguard-config.ini'
```

### Import from a URL

```bash
cpgcli config --import 'https://config.example.com/cpguard/config.ini'
```

URL-based import is the most powerful option for automation. Host your configuration file at a fixed URL and point every new server to it during provisioning with a single command.

---

## Command Summary

| Command | Purpose |
|---|---|
| `cpgcli config` | View current configuration |
| `cpgcli config --show` | Explicitly view current configuration |
| `cpgcli config --export` | Export config to default path |
| `cpgcli config --export /path/file.ini` | Export config to a custom path |
| `cpgcli config --import '/path/file.ini'` | Import config from a local file |
| `cpgcli config --import 'URL'` | Import config from a remote URL |

---
