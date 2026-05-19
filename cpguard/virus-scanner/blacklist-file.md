---
title: Scanner Blacklist
sidebar_position: 2
---


## Blacklist Files

The Blacklist is the inverse of whitelisting. It allows you to specify filenames that should always be treated as threats, regardless of their content. If the scanner encounters a blacklisted file, it will be immediately diffused or quarantined.

This is highly effective for blocking common attack scripts or persistent malware filenames like:
- `1.sh`
- `libworker.so`
- `cmd.php`

---

There are two methods to blacklist a file. Each method behaves differently once a match is detected, so choose the approach that best fits your use case.

| Method | Trigger | Action on Detection |
|---|---|---|
| **File Name** | Matches by filename | Always quarantines the file immediately |
| **Checksum (MD5)** | Matches by file content | Follows your configured "Action for virus files" preference |

---

## Method 1 : Blacklist by File Name

File name-based blacklisting quarantines any file matching the specified name as soon as it is detected, regardless of its location on the server.

> **Note:** Enter the file name only — do not include the full file path.


**Via UI:**
1. Navigate to **Settings →Virus Scanner**
2. Locate the **Blacklisted Files** field
3. Enter the file name (e.g., `malware.php`, `shell.py`)
4. Add — the rule becomes active instantly

![Firewall](../../assets/img/cpguard/firewall/vs1.png)

---

## Method 2 : Blacklist by Checksum (MD5)

Checksum-based blacklisting adds the file's MD5 hash to the virus database, allowing the scanner to detect the file regardless of its name or location. When detected, the configured **Action for virus files** preference is applied rather than an automatic quarantine.

> **Note:** Checksum-based entries are not applied instantly for manual scans. However, the automatic scanner loads the changes instantly.

![Firewall](../../assets/img/cpguard/firewall/vs2.png)

**Via UI:**
1. Navigate to **cPGuard → Settings → Virus Scanner**
2. Locate the **Custom Virus File Definitions** field
3. Enter the definition.
4. Click **Add**

---

## Blacklisting Files via CLI (During a Scan)

You can also apply a blacklist directly when running a manual scan by passing a reference file containing blacklisted file names or paths.

```bash
cpgcli scan --blacklist path
```

The `--blacklist` flag accepts a path to a file containing the blacklisted file names or paths to be applied during that scan session.

**Example: Scan a directory with a custom blacklist file:**
```bash
cpgcli scan --blacklist  /etc/cpguard/my-blacklist.txt
```

