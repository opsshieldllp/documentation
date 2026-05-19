---
slug: cpguard-add-directories-watchlist
title: cPGuard Automatic Scan Watchlist
authors: [your-name]
tags: [cpguard, scanner, watchlist, cli, command-line, opsshield, malware, security]
date: 2022-11-15
description: Learn what the cPGuard watchlist is, why you might need to extend it, and how to add, list, and remove custom directories from the automatic scanner watchlist using cpgcli.
---

# How to Add Additional Directories to the cPGuard Automatic Scan Watchlist

The cPGuard scanner engine is built to continuously monitor website files for threats. By default, it already knows where to look. But there are situations where you need to extend its reach beyond the standard document root. This guide explains what the watchlist is and how to manage it from the command line.

{/* comment */}

---

## What Is the cPGuard Watchlist?

At its core, cPGuard is a **Web Security Suite** designed to scan website files and detect threats within them. When installed, it automatically identifies all website document roots configured on the server, compiles them into a list, and continuously monitors those directories for newly uploaded or modified files that are publicly accessible.

This list of monitored directories is called the **watchlist**, and it lives at:

```
/etc/cpguard/watchlist.txt
```

You can open this file at any time to see exactly which directories are currently being monitored by the scanner engine.

---

## Why You Might Need to Extend the Watchlist

In most cases, the auto-detected document roots are sufficient. However, there are scenarios where files uploaded to your server land **outside** the standard document root     and those directories won't be scanned by default.

Common examples include:

- **Upload directories** used by web forms or custom file upload scripts
- **Gallery software** that stores user-uploaded media in a separate path
- **Temporary staging directories** outside the public web root
- **Custom application directories** that process or store user-supplied content

For any of these cases, cPGuard provides a simple CLI option to add custom paths to the watchlist.

---

## Important: Do Not Add System Directories

:::danger
The cPGuard scanner is designed specifically to scan **website files**. Adding system directories (such as `/etc`, `/usr`, `/bin`, `/var`, etc.) to the watchlist is **strongly discouraged**.

Scanning system files and automatically taking action on them (quarantine, disable) can cause **unexpected system errors** and may render your server unstable. Only add directories that contain website or web application files.
:::

---

## Managing the Watchlist via CLI

All watchlist management is done using the `cpgcli watch` command.

### Add a Directory to the Watchlist

```bash
cpgcli watch --add /full/path/to/directory
```

Replace `/full/path/to/directory` with the **complete absolute path** of the directory you want to add. Relative paths are not supported. Always use the full path starting from `/`.

**Example:**

```bash
cpgcli watch --add /home/user/uploads/gallery
```

Once added, the scanner will include this directory in all automatic scans going forward.

---

### List All Custom Watchlist Directories

To view all the additional directories you have manually added to the watchlist:

```bash
cpgcli watch --list
```

This returns only the directories you have added via CLI. It does not list the auto-detected document roots. To see the full watchlist including auto-detected paths, view the file directly:

```bash
cat /etc/cpguard/watchlist.txt
```

---

### Remove a Directory from the Watchlist

To stop monitoring a custom directory, remove it from the watchlist with:

```bash
cpgcli watch --remove /full/path/to/directory
```

**Example:**

```bash
cpgcli watch --remove /home/user/uploads/gallery
```

This removes the directory from automatic scanning without affecting any other monitored paths.

---


## Command Summary

| Command | Purpose |
|---|---|
| `cpgcli watch --add /path` | Add a directory to the automatic scan watchlist |
| `cpgcli watch --list` | List all manually added watchlist directories |
| `cpgcli watch --remove /path` | Remove a directory from the watchlist |

---

## How It Fits Into the Scanning Workflow

Once a directory is added to the watchlist, it is automatically included in:

- **Daily scans** (`cpgcli scan --daily`) — files modified in the last 24 hours
- **Weekly scans** (`cpgcli scan --wekly`) — files modified in the last 7 days
- **Full scans** (`cpgcli scan --all`) — all files across all watchlist directories

This means you don't need to manually trigger separate scans for the new directory — it integrates seamlessly into the existing scan schedule.

:::tip
If you've just added a new directory that already contains uploaded files, run a manual scan immediately to catch any existing threats before the next scheduled scan:

```bash
cpgcli scan --path /full/path/to/directory
```
:::

---