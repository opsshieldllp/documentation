---
title: Overview
sidebar_position: 1
---

# Virus Scanner Overview

The cPGuard Virus Scanner is a high-performance security engine designed to monitor and scan web-related files for malicious code, suspicious patterns, and unsafe behaviors. Optimized specifically for web hosting environments, it focuses on website document roots and publicly accessible directories to ensure server integrity without compromising performance.

The scanner operates using a multi-layered approach: real-time monitoring, scheduled re-checks, on-demand manual scans, and automated cleanup routines powered by signature-based detection and AI analysis.

---

## Real-Time Scanner

The real-time scanner runs as a background service, monitoring file system events to intercept threats the moment they are uploaded or modified.

- **Watchlist Logic**: It automatically identifies and monitors website document roots.
- **Location**: The core watchlist is maintained at `/etc/cpguard/watchlist.txt`.
- **Detection**: Applies signature matching and heuristic AI analysis to every file event.

### Managing Watch Directories
You can add directories outside of standard document roots (e.g., custom upload folders) via the App Portal or CLI:

- **Add directory**: `cpgcli watch --add /path/to/directory`
- **List directories**: `cpgcli watch --list`
- **Remove directory**: `cpgcli watch --remove /path/to/directory`

:::danger
Do not add system directories (like `/etc` or `/usr`) to the watchlist. cPGuard is optimized for web files; scanning system files may lead to stability issues or false positives.
:::

---

## Scheduled Scans

cPGuard performs automated scheduled scans to provide an additional layer of security beyond real-time monitoring. These scans ensure that any files that may have been bypassed or missed during the initial upload are re-evaluated.

* **Daily Scan**: Automatically re-scans all files that were modified within the last 24 hours.
* **Weekly Scan**: A broader automated scan that re-checks all files modified within the last 7 days.

This "re-check" logic is vital for catching threats that may have been obfuscated or were part of a delayed execution chain that the real-time scanner caught but require a deeper second pass.

---

## Manual Scans

Initiate on-demand audits via the **App Portal (Virus Scanner > Manual Scans)** or the CLI:

| Scan Type | Description | CLI Command |
| :--- | :--- | :--- |
| **All Public Files** | Scans all document roots for all websites on the server. | `cpgcli scan --all` |
| **Quick Scan** | Scans document roots of active websites. | `cpgcli scan --daily` |
| **Path Scan** | Scans a specific directory path defined by the user. | `cpgcli scan --path /path/to/dir` |


:::tip
Malware signatures are updated frequently (often multiple times a day). It is critical to run a **Full Scan (`--all`)** at regular intervals. Files that were not detected as malicious during their initial upload may be identified and handled later as new signatures for emerging threats are added to the cPGuard database.
:::

---

## Scanner Settings & Automation

You can configure how the engine handles detections under **Settings > Scanner**. Policies can be set independently for **Virus Files**, **Suspicious Files**, and **Binary Files**.

### Action on Detection ("What to do with")

* **Email Only**: Logs the event and sends an alert. No action is taken against the file.
* **Quarantine (Recommended)**: Moves the file to a secure, isolated directory. This neutralizes the threat while allowing for manual restoration.
* **Disable File**: Changes the file permissions to `000`, rendering it unreadable and non-executable by the webserver.

### Advanced Cleanup & Protection

* **Auto Clean Infected Files**: Automatically strips malicious injections (e.g., malware headers) from legitimate files to restore site functionality without deleting the entire file.
* **WordPress Core Protection**: If a modification is detected in WordPress core files, cPGuard verifies the file against official checksums. If a mismatch is found, the correct version is automatically pulled from the cPGuard CDN and restored.


---

### Suspicious Files
Files that exhibit risky behavior, contain heavily encoded payloads, or show signs of obfuscation but do not match a known virus signature are flagged as **Suspicious**. These should be reviewed manually by an administrator.

### AI Scanner
When enabled, the AI engine evaluates files using machine-learning heuristics. This layer is critical for detecting "Zero-Day" exploits and polymorphic malware that traditional signature-based scanners might miss.

---

## Post-Detection Hook Script

Automate your response workflow by executing a custom PHP script whenever a threat is detected.

* **Location**: `/opt/cpguard/app/scripts/virus_hook.php`
* **Template**: `/opt/cpguard/app/scripts/virus_hook_sample.php`

The script receives six arguments: **Original Path, Quarantine Path, Virus Description, Category, Username,** and **User Email**. This is commonly used to trigger custom notifications of take actions.

For a complete guide and sample implementation, see [Virus Hook](../api-and-hooks/virus-hook).


