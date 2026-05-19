---
title: Request body excluding files is bigger than the maximum - Request body no files data length is larger than the configured limit
sidebar_position: 4
---

You may notice either of the following errors in ModSecurity log with 413 response code while submitting your changes or uploading your files

- Request body excluding files is bigger than the maximum
- Request body no files data length is larger than the configured limit

This happens specifically because the values set are not enough for the following parameters in your ModSecurity configuration.

- `SecRequestBodyLimit`
- `SecRequestBodyNoFilesLimit`

Increasing the values for the above parameters in the ModSecurity configuration should fix this issue. Since we do not enforce the values from cPGuard configuration, the values are set either from your control panel or they are left with the default values. Given below are the steps to modify the values with some popular control panels…for other panels or Standalone servers you need to locate the configuration file and make amendments.

## 1. cPanel

You can edit the file `/etc/apache2/conf.d/modsec/modsec2.user.conf` and add the entries for the specific configuration parameters into the top of the file. Then restart your web server to reflect the changes.

Refer to [this cPanel Documentation](https://support.cpanel.net/hc/en-us/articles/11596999975447--ModSecurity-Request-body-Content-Length-is-larger-than-the-configured-limit) for additional help.

## 2. Plesk

Plesk has an official documentation about this issue and [this help article](https://www.plesk.com/kb/support/unable-to-upload-file-to-the-website-hosted-in-plesk-when-imunify-is-present-on-the-server-request-body-no-files-data-length-is-larger-than-the-configured-limit/) has the instructions to fix this error with Plesk.

## 3. DirectAdmin

On DirectAdmin servers, you can find the values set in `/etc/httpd/conf/extra/httpd-modsecurity.conf`. So you need to

1. Edit the file `/etc/httpd/conf/extra/httpd-modsecurity.conf`
2. Increase the values for `SecRequestBodyLimit` and `SecRequestBodyNoFilesLimit`
3. Restart Web Server

That will help to fix the issue. To permanently retain this change, you need to edit the custombuild configuration. Please refer to [this instruction document](https://docs.directadmin.com/webservices/apache/modsecurity.html#customizing-the-modsecurity-configuration) for details.

## 4. Enhance

On Enhance servers, you can modify the value by

1. Edit file `/etc/modsecurity.d/modsec.customisations.conf`
2. Increase the values for `SecRequestBodyLimit` and `SecRequestBodyNoFilesLimit`
3. Restart Web Server

> **Note:** You can also manage the mentioned configurations for other panels or on standalone servers. You just need to find an appropriate `.conf` file to modify the value and then restart your web server.