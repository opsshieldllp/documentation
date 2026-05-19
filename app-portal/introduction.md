---
title: cPGuard App Portal
description: Learn how to use the cPGuard App Portal at app.opsshield.com to manage all your servers from a single login — including the consolidated dashboard, per-server features, and quick server switching.
slug: cpguard-app-portal-introduction
sidebar_position: 1
authors:
  - name: OpsShield Team
tags: [cpguard, app-portal, server-management, multi-server, dashboard, security]
hide_table_of_contents: false
---

A centralised App Portal is available at [app.opsshield.com](https://app.opsshield.com) giving you a single place to view, manage, and monitor all your servers without having to log in to each server's control panel individually.

{/* comment */}

## What is the App Portal?

The App Portal is a graphical interface that communicates with the cPGuard agent service running on each of your servers. Its core purpose is to **decouple server management from individual control panels**. So whether you run one server or fifty, everything is accessible from one login.

![Logo](../assets/img/cpguard/app-portal/server-list.png)


:::tip
The App Portal works across all supported control panel types. You manage cPGuard through the portal regardless of whether your servers run cPanel, DirectAdmin, Plesk, or cPGuard X Standalone.
:::

---

## Accessing the App Portal

Visit **[https://app.opsshield.com](https://app.opsshield.com)** and log in with your OpsShield account credentials.

---

## Portal Navigation Overview

The App Portal has two distinct navigation states depending on whether you have a server open or not.

### Default View : No Server Selected

Before selecting a server, the main navigation shows two items:

| Section | Description |
|---|---|
| **Consolidated Dashboard** | An at-a-glance overview of security status across all your servers |
| **Server List** | A full list of all servers linked to your account |

### Per-Server View : After Opening a Server

Once you open a server from the server list, the navigation expands to show the full range of cPGuard features for that server:

| # | Feature | Description |
|---|---|---|
| 1 | **Server Dashboard** | Overview of security events and server health |
| 2 | **Manual Scans** | Trigger on-demand malware scans |
| 3 | **Scanner Logs** | Review results from previous scans |
| 4 | **CMS Threats** | Detected threats in WordPress, Joomla, and other CMS platforms |
| 5 | **WAF Logs** | Web Application Firewall event logs |
| 6 | **IPDB** | IP database of blocked and monitored addresses |
| 7 | **Bot Attacks** | Detected bot traffic and attack attempts |
| 8 | **Domain Reputation** | Reputation status of domains hosted on the server |
| 9 | **IP Reputation** | Reputation checks on IPs interacting with your server |
| 10 | **Rootkit Scanner** | Results from rootkit detection scans |
| 11 | **CSF Configuration** | ConfigServer Security & Firewall settings |
| 12 | **Settings** | cPGuard configuration options for the selected server |

---

## Switching Between Servers

You can switch between servers at any time without returning to the server list manually.

### Quick Search (Keyboard Shortcut)

Press the **`/`** key anywhere in the portal to open the Quick Search bar. Type the server name or IP to locate and switch to it instantly, ideal when managing a large number of servers.

![Logo](../assets/img/cpguard/app-portal/quick-search.jpg)


### Via the Navigation Menu

Alternatively, navigate back to the **Server List** from the main menu, locate the server, and open it from there.

---

## Why Use the App Portal?

| Without App Portal | With App Portal |
|---|---|
| Log in to each server's control panel separately | Single login manages all servers |
| Switch between multiple browser tabs or sessions | Switch servers instantly with Quick Search (`/`) |
| No consolidated view across servers | Unified dashboard shows all servers at a glance |
| Security logs scattered across individual panels | All logs accessible from one interface |

---

## Summary

The cPGuard App Portal at [app.opsshield.com](https://app.opsshield.com) is the recommended way to manage cPGuard across multiple servers. With a consolidated dashboard, per-server feature access, and fast server switching via Quick Search, it significantly reduces the overhead of managing security across a fleet of servers.
