---
title: Block PHP Uploads
sidebar_position: 4
---

Prevent uploading PHP scripts through web forms, web file managers, and web shells to enhance server security.

## Overview

The PHP upload blocker stops malicious actors from uploading backdoors, web shells, and other malicious PHP scripts via web interfaces. This adds an extra layer of protection beyond file upload validation rules.

## How It Works

When enabled, the upload scanner blocks all PHP script uploads submitted through:
- Web forms
- Web-based file managers
- Web shells
- Any HTTP/HTTPS upload interface

**Allowed upload methods:**
- FTP/SFTP
- SSH/SCP
- Control panel file managers (cPanel, DirectAdmin, etc.)
- Shell access

:::warning[Important Impact]
This option disables **all** PHP script uploads via web interfaces. You cannot edit and save PHP files using web-based editors when this is enabled.
:::

## Enable PHP Upload Blocking

### Using CLI

Enable the blocker:

```bash
cpgcli upload-scanner --block-php=enable
```

### Using App Portal

1. Log in to App Portal
2. Select your server
3. Navigate to **Settings** → **Additional Settings**
4. Turn **ON** the option **"Block PHP files upload"**

## Disable PHP Upload Blocking

If you need to allow PHP uploads via web interfaces:

### Using CLI

```bash
cpgcli upload-scanner --block-php=disable
```

### Using App Portal

1. Log in to App Portal
2. Select your server
3. Navigate to **Settings** → **Additional Settings**
4. Turn **OFF** the option **"Block PHP files upload"**

## Verify It's Working

After enabling PHP upload blocking, test that it's functioning correctly.

### Test Block Behavior

**Step 1:** Try uploading a PHP file via a web form

**Expected result:** Upload is blocked with an error message

**Step 2:** Check WAF logs for the blocked upload

1. Log in to App Portal → Select your server
2. Navigate to **Security** → **WAF Logs**
3. Look for entries like:

```
Action: Blocked PHP upload
File: test.php
Client IP: 192.168.1.100
Reason: PHP file upload blocked by policy
```

### Test Allowed Methods

**Verify FTP/SSH uploads still work:**

```bash
# Via SCP
scp script.php user@server:/path/to/webroot/

# Via SFTP
sftp user@server
put script.php /path/to/webroot/
```

**Expected result:** Upload succeeds without errors

## Use Cases

### When to Enable

Enable PHP upload blocking when:
- Server hosts multiple untrusted users
- Shared hosting environment
- High-security requirements
- Protection against compromised web applications
- Additional web shell protection layer

### When to Disable

Disable PHP upload blocking when:
- Single-tenant server with trusted administrators
- Application requires web-based PHP file editing
- Development environment with frequent code changes via web IDE
- Temporary need to upload PHP files via web interface

## Workarounds for Editing PHP Files

If you need to edit PHP files with the blocker enabled:

### Option 1: Control Panel File Manager

Use your control panel's file manager:
- cPanel: **File Manager**
- DirectAdmin: **File Manager**
- Plesk: **Files**
- CyberPanel: **File Manager**

### Option 2: FTP/SFTP Client

Configure an FTP client:
- FileZilla
- WinSCP
- Transmit
- Cyberduck

### Option 3: SSH Editor

Edit files directly via SSH:

```bash
ssh user@server
nano /path/to/file.php
# or
vim /path/to/file.php
```

### Option 4: Git Deployment

Use Git for code deployment:
- Push changes to repository
- Deploy via Git integration

## Impact on Applications

### Web-Based Code Editors

Applications with web-based PHP editors may be affected:
- WordPress theme/plugin editors
- Online IDE tools
- CMS template editors
- File manager plugins

**Solution:** Use control panel file managers or disable the blocker temporarily when needed.

### Plugin/Theme Upload

CMS plugin/theme uploads via admin panel:
- WordPress: Dashboard → Plugins/Themes → Upload
- Joomla: Extension installation
- Drupal: Module/theme installation

**Impact:** Installation via admin panel may fail if files contain `.php` extensions.

**Solution:** 
- Install via control panel file manager
- Use SFTP upload
- Temporarily disable PHP upload blocking

### Backup Restoration

Web-based backup plugins that restore PHP files:
- UpdraftPlus (WordPress)
- Akeeba Backup (Joomla)
- BackupBuddy

**Impact:** Restoration via web interface may fail.

**Solution:**
- Use control panel backup tools
- Restore via SSH/SFTP
- Temporarily disable blocker during restoration

## Security Best Practices

**Recommended configuration:**
```bash
# Enable PHP upload blocking
cpgcli upload-scanner --block-php=enable

# Enable WEBSHELL protection rule set
cpgcli waf --enable=webshell

# Monitor WAF logs regularly
cpgcli waf --watch
```

**Additional hardening:**
- Restrict file upload directories
- Implement file type validation
- Use proper file permissions (644 for files, 755 for directories)
- Enable PHP disable_functions in php.ini
- Monitor file integrity

## Troubleshooting

### Legitimate Uploads Blocked

**Symptoms:** Cannot upload necessary PHP files

**Solutions:**
1. Verify blocker is actually causing the issue:
   ```bash
   cpgcli upload-scanner --block-php=disable
   ```
2. Test upload again
3. If successful, use alternative upload methods or keep disabled
4. Check WAF logs to confirm block reason

### False Positive Blocks

**Symptoms:** Non-PHP files blocked (e.g., .phps, .php.txt)

**Solutions:**
1. Check exact file extension being uploaded
2. Rename file to safe extension before upload
3. Upload via FTP/SSH instead
4. Contact support if false positives persist

### Control Panel Editor Not Working

**Symptoms:** Cannot save PHP files in control panel file editor

**Solutions:**
1. Check if control panel editor uses web upload mechanism
2. Most control panel editors should work (not using HTTP upload)
3. If blocked, temporarily disable PHP blocker
4. Save file, then re-enable blocker

## Related Features

This feature works alongside other security measures:

- **WAF WEBSHELL Rules** - Blocks web shell execution attempts
- **Malware Scanner** - Detects uploaded malicious files
- **File Integrity Monitoring** - Alerts on unexpected PHP file changes

See [WAF Overview](overview.md) for related protection features.

## Product Availability

Available in: **cPGuard** and **cPGuard X**
