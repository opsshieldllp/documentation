---
title: Scanner Whitelist
sidebar_position: 1
---

# Scanner Exclusions and Whitelisting

To reduce server overhead and prevent false positives, cPGuard provides granular control over which files, users, and database signatures are bypassed by the scanner. These settings can be managed via the **App Portal > Settings > Scanner**.

:::info
User-level whitelisting is primarily designed for the **Automatic (Real-time) Scanner**. Files owned by whitelisted users may still be flagged during manual full-server scans if not specifically excluded by path.
:::

---

## Whitelist Users

Whitelisting a user instructs the real-time scanner to skip all files owned by that specific system user. This is useful for accounts running specialized development tools or administrative scripts that frequently trigger heuristic alerts.

### How to Whitelist a User
1. Navigate to **Settings > Scanner**.
2. Locate the **Whitelist Users** section.
3. Select the desired user from the dropdown menu.
4. Click **Add**.

---

## Whitelist Files

You can exclude specific files or directory paths from being flagged by the virus scanner. This is common for excluding legitimate socket files, large log files, or specific application assets that trigger false positives.

### Configuration
- **File Name/Path**: You can enter an absolute path (e.g., `/home/username/public_html/safe_script.php`) or a specific filename pattern.
- **Examples from the interface**:
    - `mysql.sock` (Excludes specific socket files)
    - `plugins/getastra/astra/libraries/plugins/astra-gk/var/db` (Excludes specific plugin directories)

---

## Whitelist DB Scan Signatures

The Database Scanner identifies malicious injections based on specific signature IDs. If a legitimate database entry is being flagged, you can exclude that specific signature.

* **Signature ID**: The unique identifier of the signature provided in the scan report.
* **Reason**: A mandatory field to document why the signature is being excluded for future audit purposes.

---


## Whitelist Logic and Best Practices

* **Use Path-Specific Whitelisting**: Whenever possible, whitelist the exact file path rather than an entire user to maintain a high security posture.
* **Document Your Exclusions**: Always provide a reason when whitelisting database signatures to help other administrators understand the exception.
* **Review Periodically**: Regularly audit your whitelist to ensure that temporary exclusions are removed once they are no longer needed.
* **System Files**: Avoid whitelisting broad system paths; cPGuard is already optimized to ignore most non-web system files by default.