---
title: "Fixing ModSecurity Request Body Limit Errors"
description: "How to resolve 413 errors when your request body exceeds ModSecurity's configured limits"
---

## Overview

When working with ModSecurity, you may encounter errors that prevent file uploads or form submissions. This guide explains the root cause and provides solutions for different control panels and server setups.

## The Problem

When submitting forms or uploading files on your server, you might see one of these errors in your ModSecurity logs, typically accompanied by a **413 HTTP response code**:

- `Request body excluding files is bigger than the maximum`
- `Request body no files data length is larger than the configured limit`

These errors occur when your request body size exceeds the limits defined in your ModSecurity configuration.

## Root Cause

The issue is caused by insufficient values set in two ModSecurity parameters:

- **`SecRequestBodyLimit`** - Maximum size of the entire request body
- **`SecRequestBodyNoFilesLimit`** - Maximum size of the request body excluding file uploads

Since cPGuard does not enforce these values from its configuration, they're either inherited from your control panel defaults or left with their default settings, which may be too restrictive.

## Solutions by Control Panel

### cPanel

**Steps:**

1. Edit the file `/etc/apache2/conf.d/modsec/modsec2.user.conf`
2. Add entries for the configuration parameters at the top of the file
3. Restart your web server to apply the changes

Example configuration:

```apache
SecRequestBodyLimit 10485760
SecRequestBodyNoFilesLimit 1048576
```

:::tip
Refer to the [cPanel ModSecurity Documentation](https://support.cpanel.net/hc/en-us/articles/11596999975447--ModSecurity-Request-body-Content-Length-is-larger-than-the-configured-limit) for additional guidance.
:::

### Plesk

Plesk provides comprehensive documentation for this issue. Follow the instructions in their official support article:

[Plesk: Unable to upload file when Imunify is present](https://www.plesk.com/kb/support/unable-to-upload-file-to-the-website-hosted-in-plesk-when-imunify-is-present-on-the-server-request-body-no-files-data-length-is-larger-than-the-configured-limit/)

### DirectAdmin

**Steps:**

1. Edit the file `/etc/httpd/conf/extra/httpd-modsecurity.conf`
2. Increase the values for `SecRequestBodyLimit` and `SecRequestBodyNoFilesLimit`
3. Restart your web server

**For permanent changes**, update your custombuild configuration. See the [DirectAdmin ModSecurity Documentation](https://docs.directadmin.com/webservices/apache/modsecurity.html#customizing-the-modsecurity-configuration) for detailed instructions.

### Enhance/cDeploy

**Steps:**

1. Edit the file `/etc/modsecurity.d/modsec.customisations.conf`
2. Increase the values for `SecRequestBodyLimit` and `SecRequestBodyNoFilesLimit`
3. Restart your web server

## Generic Configuration for Standalone Servers

If you're running a standalone server or using a different control panel:

1. **Locate your ModSecurity configuration file** - typically found in:
   - `/etc/modsecurity/modsec.conf`
   - `/etc/apache2/mods-enabled/security.conf`
   - `/etc/httpd/conf.d/mod_security.conf`

2. **Add or modify these parameters:**

```apache
SecRequestBodyLimit 10485760
SecRequestBodyNoFilesLimit 1048576
```

3. **Restart your web server:**

For Apache:
```bash
sudo systemctl restart apache2
# or
sudo service apache2 restart
```

For Nginx with ModSecurity:
```bash
sudo systemctl restart nginx
```

## Recommended Values

Here are suggested values for different use cases:

| Use Case | SecRequestBodyLimit | SecRequestBodyNoFilesLimit |
|----------|---------------------|---------------------------|
| Small sites/low uploads | 2097152 (2MB) | 262144 (256KB) |
| Medium sites | 10485760 (10MB) | 1048576 (1MB) |
| Large sites/media | 52428800 (50MB) | 5242880 (5MB) |
| Very large uploads | 104857600 (100MB) | 10485760 (10MB) |

:::warning
Set these values based on your actual requirements. Excessively large values may impact security posture and server performance.
:::

## Best Practices

1. **Monitor your logs** - Check ModSecurity logs to see what sizes your legitimate traffic uses
2. **Set appropriately** - Use values that accommodate your needs without being unnecessarily large
3. **Document changes** - Keep track of why you modified these values
4. **Test after changes** - Verify that uploads and form submissions work as expected
5. **Regular review** - Periodically review and adjust limits as your application needs evolve

## Troubleshooting

If errors persist after increasing the limits:

1. **Verify configuration syntax** - Check for typos in the `.conf` file
2. **Confirm changes took effect** - Use `apache2ctl -M | grep security` to verify ModSecurity is loaded
3. **Check error logs** - Review `/var/log/apache2/error.log` or `/var/log/modsec_audit.log` for more details
4. **Restart completely** - Ensure the web server fully restarts after configuration changes

## Summary

The "Request body exceeding maximum" error is straightforward to fix by adjusting ModSecurity parameters. Identify your control panel, edit the appropriate configuration file, increase the size limits, and restart your web server. Always set values that match your actual requirements while maintaining security best practices.

For additional support, consult your control panel's documentation or contact your hosting provider's support team.