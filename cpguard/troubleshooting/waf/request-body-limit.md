---
sidebar_label: Request body limit error
title: Fix ModSecurity Request Body Limit Errors
description: How to resolve 413 errors when request body size exceeds ModSecurity limits.
sidebar_position: 4
---

Use this article when uploads or form submissions fail with a 413 response and ModSecurity logs show either of these messages:

- `Request body excluding files is bigger than the maximum`
- `Request body no files data length is larger than the configured limit`

These errors mean the request is larger than the limits set in ModSecurity:

- `SecRequestBodyLimit`
- `SecRequestBodyNoFilesLimit`

cPGuard does not set these values. They come from the control panel's ModSecurity configuration or from ModSecurity defaults.

## Fix

Increase `SecRequestBodyLimit` and `SecRequestBodyNoFilesLimit` in your ModSecurity configuration, then restart the web server.

### cPanel
---

Edit:

```bash
/etc/apache2/conf.d/modsec/modsec2.user.conf
```

Add or update the directives near the top of the file:

```apache
SecRequestBodyLimit 10485760
SecRequestBodyNoFilesLimit 1048576
```

Restart Apache after saving the file.

### DirectAdmin
---

Edit:

```bash
/etc/httpd/conf/extra/httpd-modsecurity.conf
```

Increase `SecRequestBodyLimit` and `SecRequestBodyNoFilesLimit`, then restart the web server. For a permanent change, update the DirectAdmin custombuild configuration as well.

### Enhance
---

Edit:

```bash
/etc/modsecurity.d/modsec.customisations.conf
```

Increase `SecRequestBodyLimit` and `SecRequestBodyNoFilesLimit`, then restart the web server.

### Other Panels or Standalone Servers
---

Locate the active ModSecurity configuration file, update the same two directives, and restart Apache, Nginx, LiteSpeed, or OpenLiteSpeed.

Common locations include:

- `/etc/modsecurity/modsec.conf`
- `/etc/apache2/mods-enabled/security.conf`
- `/etc/httpd/conf.d/mod_security.conf`

## Recommended Values

Start with values that match your application's real upload and form requirements. Avoid setting unnecessarily high limits.

| Use Case | SecRequestBodyLimit | SecRequestBodyNoFilesLimit |
|---|---:|---:|
| Small sites | 2097152 (2MB) | 262144 (256KB) |
| Medium sites | 10485760 (10MB) | 1048576 (1MB) |
| Large media uploads | 52428800 (50MB) | 5242880 (5MB) |

## Verify

1. Restart the web server.
2. Reproduce the failed upload or form submission.
3. Check the ModSecurity log to confirm the same request body limit error no longer appears.
