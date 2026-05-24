---
title: Database Scanner
sidebar_position: 3
---

# Database Scanner

While file-level security is critical, attackers frequently target the data layer of websites. The cPGuard Database Scanner is a specialized module designed to identify and alert you to malicious code injections directly within your CMS databases.

Currently, the scanner is highly optimized for **WordPress**, with support for other CMS platforms continually expanding.

---

## How It Works

The Database Scanner identifies common database-level threats, including:
- **Malicious Redirects**: Code injected into rows that sends visitors to external phishing or malware sites.
- **JavaScript Injections**: Malicious scripts added to posts or pages that execute in the visitor's browser.
- **Hidden Backdoors**: Code snippets that allow attackers to regain access even after file-level malware is removed.

### Scanning Modes
1. **Manual Integration**: When you initiate a manual file scan on a path containing a WordPress installation, cPGuard automatically triggers a database scan for that instance.
2. **Scheduled Scanning**: If enabled, the system performs a daily discovery and analysis of all databases on the server.
3. **App Portal**: View and manage all database-specific threats under **Virus Scanner > DB Scanner**.

---

## Configuration

You can manage the Database Scanner status via the **App Portal**:
1. Go to **Settings > Scanner**.
2. Locate the **Database Scanner** toggle.
3. Turn it **ON** to enable automatic daily background scans.

---

## CLI Usage

You can use the `cpgcli` utility to run targeted database audits from the terminal.

| Task | Command |
| :--- | :--- |
| **Scan a specific installation** | `cpgcli dbscan --scan /path/to/webroot` |
| **Scan all databases on server** | `cpgcli dbscan --scan --all` |
| **View help & options** | `cpgcli dbscan --help` |

:::info
 CLI-based scans display results directly in your terminal console but do not store persistent logs in the App Portal.
:::
---

## Whitelisting Signatures

If the Database Scanner flags a legitimate entry (e.g., a specific tracking script or unique site configuration), you can exclude that signature to prevent it from reappearing in future reports.

- **Signature Whitelist**: Locate the **Whitelist DB scan signatures** section in the scanner settings.
- **Signature ID**: Enter the unique ID provided in the scan result and provide a clear reason for the exclusion.

---

## Why Database Scanning is Essential

Modern malware often lives in two places: the **filesystem** and the **database**. Cleaning only the files while leaving the database infected often results in "re-infection" as the malicious database entries can sometimes be used to recreate the infected files. 

**Best Practice**: Always check the DB Scanner reports after a significant malware cleanup to ensure no malicious persistence remains.