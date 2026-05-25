---
sidebar_label: Webmin/Virtualmin
sidebar_position: 3
title: Webmin/Virtualmin setup
description: How cPGuard behaves on Webmin/Virtualmin, what is auto-detected, and what still needs manual standalone WAF configuration.
---

# Webmin/Virtualmin setup

Webmin/Virtualmin is a **partial automatic** case for cPGuard.

cPGuard can use built-in logic to obtain the **domain + document root + user** side of the configuration, but **WAF details still need to be supplied manually** just like a standalone setup.

---

## What is automatic on Webmin/Virtualmin

On Webmin/Virtualmin, cPGuard can usually obtain:

- domains
- document roots
- system users

That means the website-data side of standalone configuration often does not need manual JSON or custom parsing on Webmin.

---

## What is still manual

For WAF, Webmin behaves like standalone.

You still need to provide the WAF-related values in `cpguard.ini`:

- `waf_server`
- `waf_server_conf`
- `waf_server_restart_cmd`
- `waf_audit_log`
- `waf_server_error_log`

If you want WAF enabled, provide the whole WAF block together.

---

## Typical workflow

1. Install cPGuard normally.
2. Let cPGuard use the built-in Webmin/Virtualmin handling for website discovery.
3. Edit `/opt/cpguard/cpguard.ini` and fill in the WAF values.
4. Apply the updated configuration:

   ```bash
   cpgcli standalone-conf --update
   ```

You can also reopen the standalone wizard:

```bash
cpgcli standalone-conf --wizard
```

---

## Example: Apache with ModSecurity on Webmin/Virtualmin

```ini
; Website data is usually auto-resolved for Webmin/Virtualmin
domain_list = auto
user_list = auto

; WAF values still need to be provided
waf_server = apache
waf_server_conf = /etc/modsecurity/cpguard.conf
waf_server_restart_cmd = /usr/sbin/service apache2 restart
waf_audit_log = /var/log/apache2/modsec_audit.log
waf_server_error_log = /var/log/apache2/error.log
```

:::note
If your Webmin/Virtualmin setup is customised and the automatic website-data detection does not match your layout, you can still override it with explicit `domain_list` and `user_list` values just like any other standalone server.
:::

---

## Related pages

- [Standalone Overview & Installation](./overview)
- [Standalone Configuration Reference](./configuration)


 

 
