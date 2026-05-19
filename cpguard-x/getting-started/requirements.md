---
title: Requirements
sidebar_position: 1
description: System requirements and prerequisites for installing cPGuard X control panel.
---

# cPGuard X Installation Requirements

Before installing **cPGuard X**, ensure that your server environment meets the following requirements.

## Operating System

cPGuard X must be installed on a supported **64-bit Linux operating system**.

Currently supported installation environment:

- Ubuntu 24.04 (recommended)

The server must be a **fresh installation with no existing control panels installed**.

## Server Access

You must have:

- **Root SSH access** to the server
- Ability to execute commands as the **root user**

The installation process requires administrative privileges to install packages and configure services.

## Clean Server Requirement

The installation must be performed on an **empty server environment**.

Do **not install cPGuard X on servers that already contain another hosting control panel** or conflicting management software.

## License Requirement

Before starting the installation, ensure that you have a **valid cPGuard X license key** available. The license key is required during installation to bind the server to the cPGuard X platform.

## Supported Database Engines

cPGuard X supports multiple database engines. You can choose the appropriate one during installation.

Supported options include:

- MySQL 8.0 (default)
- MySQL 8.4
- MariaDB 10.11
- MariaDB 11.4

## Network Requirements

The server must allow communication between the **cPGuard X agent** and the **App Portal**.

The agent service runs on the following port:

- **Port:** `9098`

Ensure that firewalls or cloud security groups allow connectivity from the following IP addresses:

```
137.184.200.210
159.89.87.35
167.99.149.179
```

