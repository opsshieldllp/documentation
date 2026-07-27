---
sidebar_position: 2
sidebar_label: Manual scan
title: Manual virus scans

description: Learn how to start a manual malware scan in cPGuard — against all public files, a specific document root, or a custom directory — using both the App Portal UI and the CLI.
---


While cPGuard's automatic scanner continuously monitors your server for newly modified or uploaded threats, there are times when you need to trigger a **manual scan on demand** after restoring a backup, onboarding a new website, responding to a suspected compromise, or simply running a routine deep check. cPGuard provides full flexibility to initiate manual scans against any target, from both the App Portal UI and the command line.

{/* comment */}

---

## Scan Targets Available

When starting a manual scan, you can choose from three types of targets depending on how broad or focused you need the scan to be:

| Scan Target | What It Scans |
|---|---|
| **All public files** | All files within the document root of every website on the server. Whitelisted users are excluded. |
| **Specific document root** | A single website's document root, selected from a dropdown list of hosted sites |
| **Specific directory** | Any custom path you enter manually |

:::note
cPGuard is a **Web Security Suite**. Its scanner engine is specifically designed to scan web-related files. It is strongly recommended to scan only web-specific directories (document roots, upload directories, etc.) to avoid false positive reports on system or application files that are not web-related.
:::

---

## Method 1 : Start a Scan from the App Portal UI

The App Portal provides a visual interface for initiating and monitoring manual scans.

**Steps:**

1. Log in to the **cPGuard App Portal** and open your server.
2. Navigate to **Virus Scanner → Manual Scans**.
3. Choose your scan target:
   - **Full Scan** : Start a full scan on all accounts on the Server
   - **Quick Scan** : Scan directory that you selected from the dropdown list of hosted sites
   - **Path Scan** : Absolute path of the directory you wish to scan


![Logo](../../assets/img/cpguard/scanner/cpg_man_scan_ui.png)


4. Click **Scan** button to begin.

Once the scan is running, you can monitor its **real-time progress** directly from the Manual Scans page. After completion, the results are available for review in the same interface, where you can take action on any detected files.

:::tip
The App Portal UI is the best option when you want to monitor scan progress in real time or when scanning a specific user's document root from the dropdown. it's faster and more visual than using the CLI for one-off scans.
:::

---

## Method 2 : Start a Scan via CLI

For administrators who prefer the terminal, or need to automate scan triggers as part of a maintenance or incident response script, all manual scan options are available through `cpgcli`.

### Scan All Public Files

```bash
cpgcli scan --all
```

Scans all files within the document roots of all websites on the server. Whitelisted users are excluded from this scan.

### Scan Files Modified in the Last 24 Hours

```bash
cpgcli scan --daily
```

Scans only files modified within the last 24 hours across all watchlist directories — faster than a full scan and ideal for catching recent infections.

### Scan Files Modified in the Last 7 Days

```bash
cpgcli scan --weekly
```

Scans files modified within the last 7 days — a broader incremental scan that balances coverage and speed.

### Scan a Specific Directory

```bash
cpgcli scan --path /path/to/directory
```

Scans a specific directory path you provide. Replace `/path/to/directory` with the full absolute path of the target directory.

**Example — scan a specific user's public_html:**

```bash
cpgcli scan --path /home/username/public_html
```

---


## Viewing Scan Results and Taking Action

After a manual scan completes, the results are available in the **App Portal → Virus Scanner → Manual Scans** section. From there you can:

- **Review** detected files with their threat classifications
- **Quarantine** confirmed threats
- **Restore** false positives to their original location
- **Whitelist** files or users you want excluded from future scans
- **View results** of the scan with ID via CLI: `cpgcli scan --result ID`

---

## Scanner Management

The `scanner` command is used to manage the cPGuard malware scanner service. This service is responsible for the automatic malware scanning functionality in cPGuard.

To view the current scanner status:

```bash
cpgcli scanner
```

or

```bash
cpgcli scanner --status
```

### Enable the Scanner

To enable the automatic malware scanner service:

```bash
cpgcli scanner --enable
```

### Disable the Scanner

To disable the automatic malware scanner service:

```bash
cpgcli scanner --disable
```

> **Note:** Disabling the scanner only stops the automatic malware scanning service. You can still perform manual scans at any time using the `cpgcli scan` command.

### Restart the Scanner

If required, you can restart the scanner service using:

```bash
cpgcli scanner --restart
```

This is useful after changing scanner-related settings or when troubleshooting the scanner service.

## Scheduled Scans

In addition to manual scans, cPGuard provides several scheduled scanning options.

### Daily Scan

The daily scan re-checks all files in the watchlist that have been modified within the last 24 hours.

To view the current configuration:

```bash
cpgcli dailyscan
```

Enable the daily scan:

```bash
cpgcli dailyscan --enable
```

Disable the daily scan:

```bash
cpgcli dailyscan --disable
```

### Weekly Scan

The weekly scan runs every Sunday at midnight and re-checks all files in the watchlist that have been modified within the last 7 days.

To view the current configuration:

```bash
cpgcli weeklyscan
```

Enable the weekly scan:

```bash
cpgcli weeklyscan --enable
```

Disable the weekly scan:

```bash
cpgcli weeklyscan --disable
```

### Scheduled Full Scan

In addition to the daily and weekly incremental scans, cPGuard provides a built-in scheduled malware scan feature that allows you to configure and manage scheduled full scans for all monitored website files on the system.

To view the current configuration:

```bash
cpgcli schedulescan
```

Enable the scheduled scan:

```bash
cpgcli schedulescan --enable
```

Disable the scheduled scan:

```bash
cpgcli schedulescan --disable
```

You can also customize the scan schedule and resource usage with the following options:

- `--day <DAY>` – Sets the day of the month on which the full system scan will run. The default is **1** (the first day of every month).
- `--nice <VALUE>` – Sets the CPU priority (nice value) for the scan process. The default is **0**.

For example, to change the CPU priority:

```bash
cpgcli schedulescan --nice 10
```

A higher nice value lowers the scan process priority, allowing other applications and services to receive more CPU time and reducing the impact of the scheduled scan on busy servers.

To change the day of the month when the scheduled scan runs:

```bash
cpgcli schedulescan --day 15
```

The above example schedules the full malware scan to run on the 15th day of every month.

## AI Scan

The AI scan feature enables AI-based analysis for suspicious files to improve malware detection.

To view the current configuration:

```bash
cpgcli ai-scan
```

Enable AI scanning:

```bash
cpgcli ai-scan --enable
```

Disable AI scanning:

```bash
cpgcli ai-scan --disable
```