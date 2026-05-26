---
sidebar_position: 2
sidebar_label: Configuration Reference
title: Standalone Configuration Reference
description: Full reference for standalone cpguard.ini settings, JSON formats, priorities, and update commands.
keywords: [cpguard, standalone, configuration, cpguard.ini, modsecurity]

---

# Standalone Configuration Reference

This page explains every standalone setting in `cpguard.ini`, when to use it, and how the values relate to each other.

:::info
Standalone configuration is stored at `/opt/cpguard/cpguard.ini`.
:::

---

## How standalone data is supplied

For standalone installations, cPGuard needs:

1. **Website data**: domain, document root, and user
2. **User contact data**: user and email
3. **WAF data**: ModSecurity integration points

The website-data requirement can be satisfied in either of these ways:

- `web_server` + `web_server_conf`
- `domain_list`

If `domain_list` is set, it takes priority.

---

## Website data source

### `web_server`

The web server whose virtual host configuration cPGuard should parse.

**Allowed values:**

- `apache`
- `nginx`
- `litespeed`
- `openlitespeed`

```ini
web_server = nginx
```

### `web_server_conf`

The file path or glob pattern containing the domain and document-root declarations for the web server.

Examples:

```ini
web_server_conf = /etc/apache2/sites-enabled/*.conf
web_server_conf = /home/*/apache.conf
web_server_conf = /etc/apache2/apache2.conf
web_server_conf = /etc/nginx/nginx.conf
```

Use `web_server` and `web_server_conf` together when your virtual host configuration reliably exposes the domains and document roots cPGuard needs to parse.

---

### `domain_list`

An explicit source for **domain + document root + user** data.

Use this when:

- your server layout is non-standard
- the web server config does not cleanly expose all values
- you already have a panel/API/script that can generate the data
- you want full control over what cPGuard imports

`domain_list` can point to:

- a JSON file
- an executable script that outputs JSON

If `domain_list` is set, it overrides `web_server` and `web_server_conf` for website data discovery.

#### JSON format

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

#### Examples

```ini
domain_list = /opt/cpguard/domains.json
domain_list = /usr/local/bin/get_domains.sh
```

:::note
`domain_list = auto` is only meaningful when cPGuard detects a supported control panel with a premade script. On a true standalone server, `auto` should be treated as effectively empty.
:::

---

## User and email source

### `user_list`

The source for **user + email** mappings. cPGuard uses this for user-facing features such as notifications and account mapping in the UI.

Like `domain_list`, this can point to:

- a JSON file
- an executable script that outputs JSON

#### JSON format

```json
[
  {
    "user": "username1",
    "email": "mail1@example.com"
  },
  {
    "user": "username2",
    "email": "mail2@example.com"
  }
]
```

#### Examples

```ini
user_list = /opt/cpguard/users.json
user_list = /usr/local/bin/get_users.sh
```

:::warning
If `user_list` is not provided, user-email-based notifications and related features are disabled.
:::

:::note
`user_list = auto` only works when a supported control panel is detected. On a true standalone server, use an explicit JSON file or script path.
:::

---

## Account suspension hook

### `suspend_hook`

An optional PHP script triggered on cPGuard user suspension events.

Recommended workflow:

1. Copy the sample hook:

   ```bash
   cp /opt/cpguard/app/scripts/suspend_hook_sample.php /root/suspend_hook.php
   ```

2. Modify the copied file to implement your own suspension logic.
3. Set the path in `cpguard.ini`:

   ```ini
   suspend_hook = /root/suspend_hook.php
   ```

When configured, cPGuard copies the provided file to:

```text
/opt/cpguard/app/script/suspend_hook.php
```

You can later edit that copied file directly if needed.

:::caution
Only enable this after testing your hook carefully. A faulty hook can suspend the wrong accounts or fail to suspend the intended ones.
:::

---

## WAF configuration

These settings are used only when enabling the cPGuard WAF with ModSecurity.

:::info
Treat WAF settings as a complete block. If you intend to use WAF, provide all WAF-related values together instead of only filling some of them.
:::

`web_server` and `waf_server` are independent:

- `web_server` is used to discover websites
- `waf_server` is used to load ModSecurity rules

They can be the same server or different ones.

---

### `waf_server`

The web server where ModSecurity is installed and where cPGuard WAF rules should be loaded.

**Allowed values:**

- `apache`
- `nginx`
- `litespeed`
- `openlitespeed`

ModSecurity version **2.9.4 or higher** is required.

```ini
waf_server = apache
```

### `waf_server_conf`

The file path where cPGuard should create or append its ModSecurity include configuration.

Examples:

```ini
waf_server_conf = /etc/modsecurity/cpguard.conf
waf_server_conf = /etc/nginx/modsec/cpguard.conf
waf_server_conf = /usr/local/lsws/conf/cpguard.conf
```

:::danger
This path must be valid for your web server's include mechanism. A wrong value can break the web server configuration.
:::

### `waf_server_restart_cmd`

The full restart command cPGuard should run after updating WAF-related configuration.

Examples:

```ini
waf_server_restart_cmd = /usr/sbin/service httpd restart
waf_server_restart_cmd = /usr/sbin/service apache2 restart
waf_server_restart_cmd = /bin/systemctl restart nginx
waf_server_restart_cmd = /usr/local/lsws/bin/lsws_ctl restart
```

### `waf_audit_log`

The ModSecurity audit log file. cPGuard reads this to collect WAF events.

If left empty, cPGuard tries common default locations.

Examples:

```ini
waf_audit_log = /var/log/modsec_audit.log
waf_audit_log = /var/log/nginx/modsec_audit.log
waf_audit_log = /var/log/httpd/modsec_audit.log
waf_audit_log = /etc/apache2/logs/modsec_audit.log
```

### `waf_server_error_log`

The web server error log. Some WAF/ModSecurity events are written here instead of the audit log, and cPGuard also uses it for certain brute-force and bot-related event collection.

If left empty, cPGuard tries common default locations.

Examples:

```ini
waf_server_error_log = /var/log/httpd/error_log
waf_server_error_log = /var/log/nginx/error_log
waf_server_error_log = /etc/apache2/logs/error_log
waf_server_error_log = /usr/local/lsws/logs/error.log
```

---

## Complete examples

### Example 1: parse web server config directly

```ini
web_server = apache
web_server_conf = /etc/apache2/sites-enabled/*.conf

user_list = /opt/cpguard/users.json

waf_server = apache
waf_server_conf = /etc/modsecurity/cpguard.conf
waf_server_restart_cmd = /usr/sbin/service apache2 restart
waf_audit_log = /var/log/apache2/modsec_audit.log
waf_server_error_log = /var/log/apache2/error.log
```

### Example 2: explicit JSON/script sources

```ini
domain_list = /usr/local/bin/get_domains.sh
user_list = /usr/local/bin/get_users.sh

waf_server = nginx
waf_server_conf = /etc/nginx/modsec/cpguard.conf
waf_server_restart_cmd = /bin/systemctl restart nginx
waf_audit_log = /var/log/nginx/modsec_audit.log
waf_server_error_log = /var/log/nginx/error_log
```

### Example 3: split web and WAF servers

```ini
web_server = nginx
web_server_conf = /etc/nginx/nginx.conf

user_list = /opt/cpguard/users.json

waf_server = apache
waf_server_conf = /etc/modsecurity/cpguard.conf
waf_server_restart_cmd = /usr/sbin/service httpd restart
waf_audit_log = /var/log/httpd/modsec_audit.log
waf_server_error_log = /var/log/httpd/error_log
```

---

## Updating the configuration later

### Edit the file directly

```bash
nano /opt/cpguard/cpguard.ini
```

Then apply the changes:

```bash
cpgcli standalone-conf --update
```

### Re-run the wizard

```bash
cpgcli standalone-conf --wizard
```

Use the wizard when you want cPGuard to walk you through the standalone settings again interactively.
 
