---
slug: standalone-configuration
id: cpguard-standalone-configuration
title: cPGuard Standalone Configuration Guide
sidebar_position: 3
---

# cPGuard Standalone Configuration Guide

## What is cPGuard Standalone Configuration?

cPGuard provides native support for three major control panels: **cPanel**, **DirectAdmin**, and **Plesk**. For these platforms, integration is straightforward with dedicated setup scripts that automatically configure the scanner, WAF, and other modules.

However, if you're running a custom control panel or a standalone web server without any control panel management system, you'll need to manually configure cPGuard with your specific environment details. This custom configuration is stored in the **`cpguard.ini`** file located at:

```
/opt/cpguard/cpguard.ini
```
## Information You May Be Asked

During standalone installation, cPGuard may prompt for:

- Web server type (Apache / Nginx / OpenLiteSpeed / LiteSpeed)
- Virtual host or main configuration file path
- Which web server runs ModSecurity
- ModSecurity include file or directory
- Custom document-root layout (if auto-detection is insufficient)

:::note
This allows cPGuard to build watchlist for malware sacanner and correctly load WAF rules and apply protections.
:::

During installation, cPGuard attempts to detect these automatically and asks for your confirmation.
If detected values look correct, you can **accept them** and proceed.

---

The standalone can be configured using **Intsallation Wizard** or by editing **ini** file.

### The Installation Wizard

When prompted, you have two options:

#### Option 1: Configure Interactively (Recommended)

Provide values directly during installation:

```bash
cpgcli standalone-conf --wizard
```

- **Web Server Name**
  `apache`, `nginx`, or `litespeed`

- **Web Server Configuration Path**
  Example: `/etc/nginx/nginx.conf`

- **WAF Server**
  Where ModSecurity is installed

---

### Option 2: Skip and Configure Later using cpguard.ini

If you are unsure, you can safely skip configuration:

- Accept defaults or leave values empty
- Installation completes successfully
- Edit configuration later at:

```
/opt/cpguard/cpguard.ini
```

Apply changes after editing:

```bash
cpgcli standalone-conf --update
```

Or re-run the full wizard anytime:

```bash
cpgcli standalone-conf --wizard
```

---

## Unattended Installation Using `cpguard.ini`

For bulk deployments or automated provisioning, you can skip all interactive prompts by providing a pre-configured `cpguard.ini` file during installation.

This is useful when:

- Installing cPGuard on multiple servers
- Using configuration management tools
- Deploying identical environments

---

### Step 1: Prepare a `cpguard.ini` File

Create a configuration file on the server (for example: `/root/cpguard.ini`).
You can content the file below to prepare the file


## Major Configuration Sections in cpguard.ini

To deploy cPGuard on a standalone server, you need to provide several key pieces of information:

- Web server type and configuration path
- Website directories for watchlist scanning
- WAF (ModSecurity) server details
- User and email information
- Optional account suspension hooks

Let's explore each configuration section in detail.

---

## Web Server Name – `web_server`

This parameter specifies the **Web Application Server** running your websites and virtual hosts.

### Supported Web Servers
- Apache
- Nginx
- Litespeed
- OpenliteSpeed

### Configuration
```ini
web_server = apache
```

---

## Web Server Configuration Path – `web_server_conf`

This is the file or directory path where your websites/virtual hosts are configured.

cPGuard reads this configuration to:
- Detect all websites configured on your server
- Identify the document root for each website
- Build the **watchlist** (directories to monitor)
- Associate domains with their respective folders

### Common Examples

```ini
# Apache with site-enabled directory
web_server_conf = /etc/apache2/sites-enabled/*.conf

# Apache with per-user configuration
web_server_conf = /home/*/apache.conf

# Apache main configuration
web_server_conf = /etc/apache2/apache2.conf

# Nginx configuration
web_server_conf = /etc/nginx/nginx.conf
```

### Important Note
By specifying the correct configuration path, cPGuard minimizes overhead by scanning only website files and excluding unnecessary system directories.

---

## Domain, Document Root & User Source – `domain_list`

:::info 
**Optional but Recommended**

This parameter is optional, but we strongly recommend setting it for custom environments.
:::

The `domain_list` parameter provides the source for building your directory watchlist. While cPGuard can auto-detect virtual hosts from `web_server_conf`, the `domain_list` setting takes precedence and ensures comprehensive scanning in custom setups.

### Input Format

You can provide either:
1. **Path to a JSON file** containing domain information
2. **Path to a script** that generates JSON output

### JSON Structure

```json
[
  {
    "domain": "example.com",
    "docroot": "/home/username/public_html/",
    "user": "username"
  },
  {
    "domain": "subdomain.example.com",
    "docroot": "/home/username/subdomain/",
    "user": "username"
  }
]
```

### Configuration Example

```ini
domain_list = /opt/cpguard/domains.json
# OR
domain_list = /usr/local/bin/get_domains.php
```

---

## WAF Server Name – `waf_server`

Specifies the web server on which **ModSecurity** is installed and where cPGuard WAF rules will be loaded.

### Requirements
- ModSecurity version **2.9.4 or higher**
- Leave empty if ModSecurity is not installed (WAF module will be disabled)
- You can later install ModSecurity and update this value

### Supported Options
- `apache`
- `nginx`
- `litespeed`

### Configuration

```ini
waf_server = nginx
```

---

## WAF Server Configuration – `waf_server_conf`

The file path where cPGuard will include its ModSecurity configuration.

:::danger[Critical] 
Use extreme care with this setting. An incorrect value can cause web server crashes.
:::

### Guidelines
- The path must be suitable for an `Include` directive in your web server configuration
- cPGuard will create the file if it doesn't exist
- If the file already exists, cPGuard will append the configuration

### Examples

```ini
# Nginx ModSecurity configuration
waf_server_conf = /etc/nginx/modsec/cpguard.conf

# Apache configuration
waf_server_conf = /etc/apache2/mods-enabled/cpguard.conf

# Litespeed configuration
waf_server_conf = /usr/local/lsws/conf/cpguard.conf
```

---

## WAF Webserver Restart Command – `waf_server_restart_cmd`

The command to restart your WAF web server after configuration changes.

:::tip[Best Practices]
Always provide the complete path to avoid execution errors.
:::

### Common Examples

```ini
# Nginx
waf_server_restart_cmd = /usr/sbin/service nginx restart

# Apache
waf_server_restart_cmd = /usr/sbin/service apache2 restart

# Systemd (modern Linux)
waf_server_restart_cmd = /bin/systemctl restart nginx

# Litespeed
waf_server_restart_cmd = /usr/local/lsws/bin/lsws_ctl restart
```

---

## WAF Audit Log Location – `waf_audit_log`

The file path where ModSecurity logs all WAF activities and rule violations.

### Purpose
- cPGuard's log collection script monitors this file
- Logs are saved to the local WAF database
- Used for display in the WAF logs section of the UI

### Auto-Detection
Leave this field empty if you want cPGuard to search standard locations automatically.

### Common Paths

```ini
# ModSecurity default
waf_audit_log = /var/log/modsec_audit.log

# Nginx with ModSecurity
waf_audit_log = /var/log/nginx/modsec_audit.log

# Nginx error log
waf_audit_log = /var/log/nginx/error_log
```

---

## User & Email Data – `user_list`

Specifies the source for user names and contact email addresses.

:::info[Optional but Important]
Leaving this field empty will disable modules that rely on user data and email notifications.
:::

### Input Format

Provide either:
1. **Path to a JSON file** with user information
2. **Path to a script** that generates JSON output

### JSON Structure

```json
[
  {
    "user": "exampleuser1",
    "email": "user1@example.com"
  },
  {
    "user": "exampleuser2",
    "email": "user2@example.com"
  }
]
```

### Use Cases
- Building user lists in the UI
- Sending notification emails about security events
- Reporting suspicious activity to account owners

### Configuration Example

```ini
user_list = /opt/cpguard/users.json
# OR
user_list = /usr/local/bin/generate_users.php
```

---

## Automatic Account Suspension Script – `suspend_hook`

An optional PHP script triggered when a user suspension event occurs in cPGuard.

### Purpose
Enable automatic account suspension on your server based on security events.

### Setup

1. **Reference the sample file:**
   ```
   /opt/cpguard/app/scripts/suspend_hook_sample.php
   ```

2. **Configure in cpguard.ini:**
   ```ini
   suspend_hook = /opt/cpguard/app/scripts/suspend_hook.php
   ```

3. **Customize the script** based on your specific suspension requirements

:::caution
Only enable this feature if you have thoroughly tested it in your environment. Incorrect configuration could lead to unintended account suspensions.
:::

---

## Complete Configuration Example

Here's a sample `cpguard.ini` file for a standalone Nginx server:

```ini
; Web Server Configuration
web_server = nginx
web_server_conf = /etc/nginx/nginx.conf

; Domain & Directory Watchlist
domain_list = /opt/cpguard/domains.json

; WAF (ModSecurity) Configuration
waf_server = nginx
waf_server_conf = /etc/nginx/modsec/cpguard.conf
waf_server_restart_cmd = /bin/systemctl restart nginx
waf_audit_log = /var/log/nginx/modsec_audit.log

; User & Email Configuration
user_list = /opt/cpguard/users.json

; Optional: Automatic Suspension
; suspend_hook = /opt/cpguard/app/scripts/suspend_hook.php
```

---


---

## Summary

| Parameter | Purpose | Required |
|-----------|---------|----------|
| `web_server` | Web server type (Apache/Nginx/Litespeed) | Yes |
| `web_server_conf` | Path to web server configuration | Yes |
| `domain_list` | List of domains and document roots | Recommended |
| `waf_server` | WAF server type | If using WAF |
| `waf_server_conf` | WAF configuration file path | If using WAF |
| `waf_server_restart_cmd` | Command to restart WAF | If using WAF |
| `waf_audit_log` | ModSecurity audit log path | Optional |
| `user_list` | User and email information | Optional |
| `suspend_hook` | Account suspension script | Optional |

---
 
