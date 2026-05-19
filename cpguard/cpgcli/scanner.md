---
slug: cpguard-scanner-cli
title: cPGuard Scanner – Full CLI Command Reference
authors: [your-name]
tags: [cpguard, scanner, cli, command-line, malware, opsshield, security, autoclean]
date: 2026-04-16
description: A complete command-line reference for the cPGuard scanner engine — covering how to enable/disable the service, start scans, schedule daily and weekly scans, configure auto-cleanup, and set file actions.
---

# cPGuard Scanner – Full CLI Command Reference

The cPGuard scanner engine is a powerful malware detection tool that runs directly on your server. Most of its commonly used settings and operations can be controlled entirely from the **command line** using the `cpgcli` utility — no need to log into the App Portal for routine tasks.

This guide covers all available scanner CLI commands in one place.

{/* comment */}

---

## Enable / Disable the Scanner Engine

You can start, stop, or restart the cPGuard scanner background service with the following commands:



| Command | Action |
|---|---|
| `cpgcli scanner --enable` | Enable the scanner background service |
| `cpgcli scanner --disable` | Disable the scanner background service |
| `cpgcli scanner --restart` | Restart the scanner background service |

---

## Start a Scan

You can trigger an immediate scan against various targets using the `cpgcli scan` command. The following scan modes are available:



| Command | What It Scans |
|---|---|
| `cpgcli scan --all` | All directories currently in the watchlist |
| `cpgcli scan --daily` | Files modified within the last 24 hours |
| `cpgcli scan --wekly` | Files modified within the last 7 days |
| `cpgcli scan --path directory` | A specific path you provide |

:::tip
Use `cpgcli scan --daily` or `cpgcli scan --wekly` for faster, incremental scans on active servers. Reserve `--all` for deep scans during maintenance windows or after a suspected compromise.
:::

---

## Schedule Automatic Daily & Weekly Scans

Instead of triggering scans manually, you can schedule them to run automatically on a daily or weekly basis:



| Command | Action |
|---|---|
| `cpgcli dailyscan --enable` | Enable automatic daily scan |
| `cpgcli dailyscan --disable` | Disable automatic daily scan |
| `cpgcli weeklyscan --enable` | Enable automatic weekly scan |
| `cpgcli weeklyscan --disable` | Disable automatic weekly scan |

:::tip
Enabling both daily and weekly scans together provides layered coverage,  daily scans catch recent infections quickly, while weekly scans ensure nothing was missed over a longer time window.
:::

---

## Automatic File Cleanup

The **Autoclean** feature allows cPGuard to automatically remove injected malicious code from infected PHP and JavaScript files, restoring them to a clean state without deleting the files entirely.



| Command | Action |
|---|---|
| `cpgcli cleanup --enable` | Enable automatic cleanup of supported files |
| `cpgcli cleanup --disable` | Disable automatic cleanup of supported files |

:::note
Autoclean works on supported file types only (PHP and JS). Files that cannot be safely cleaned — such as binary malware or fully malicious scripts — are handled according to your configured **File Actions** (see below).
:::

---

## File Actions

You can define what action cPGuard should automatically take when the scanner detects an infected file. Actions can be set independently for three file categories: **virus**, **suspicious**, and **binary**.



The allowed options for `<option>` are:

| Option | Behaviour |
|---|---|
| `email` | Send an alert email to the administrator |
| `disable` | Disable the file by changing its permissions |
| `quarantine` | Move the file to a quarantine zone |

**Example — quarantine virus files and email on suspicious detections:**



:::tip
For most production servers, setting `--virus` and `--binary` to `quarantine` and `--suspicious` to `email` is a sensible default. This takes immediate action on confirmed threats while alerting you to review borderline detections manually.
:::



---
