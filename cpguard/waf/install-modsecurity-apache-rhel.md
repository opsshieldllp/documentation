---
title: How to Install ModSecurity 2.9.7 in Apache
sidebar_position: 2
---

This guide covers manual installation of ModSecurity 2.9.7 with Apache on RHEL-based distributions (CentOS 7 or higher, AlmaLinux, Rocky Linux).

---

## What is ModSecurity?

ModSecurity is an open-source web application firewall (WAF) originally built for Apache that provides Layer 7 (HTTP) protection against web attacks like SQL injection, cross-site scripting, and local file inclusion.

cPGuard WAF is a ModSecurity rule set powered by Malware.Expert commercial rules, providing protection against targeted and automated attacks with explicit rules for WordPress, Joomla, and other CMS platforms.

:::info
cPGuard WAF requires **ModSecurity 2.9.4 or higher** because earlier versions lack proper `SecRemoteRules` support.
:::

---

## Prerequisites

- RHEL-based OS (CentOS 7+, AlmaLinux, Rocky Linux)
- Apache web server installed
- Root or sudo access
- Internet connectivity for downloading packages

---

## Step 1: Update OS and Install Dependencies

```bash
yum clean all
yum -y update
yum install gcc make httpd-devel libxml2 pcre-devel libxml2-devel curl-devel libtool
```

Remove any existing ModSecurity package:

```bash
yum remove mod_security
```

---

## Step 2: Download and Install ModSecurity

Download ModSecurity 2.9.7 source code:

```bash
wget https://github.com/SpiderLabs/ModSecurity/archive/refs/tags/v2.9.7.zip
unzip v2.9.7.zip
cd ModSecurity-2.9.7/
```

Compile and install:

```bash
./autogen.sh
./configure
make
make install
```

This installs ModSecurity to `/usr/local/modsecurity/lib/mod_security2.so`.

---

## Step 3: Enable ModSecurity in Apache

Create Apache module configuration:

```bash
cat > /etc/httpd/conf.modules.d/10-mod_security.conf << 'EOF'
LoadModule security2_module /usr/local/modsecurity/lib/mod_security2.so
EOF
```

---

## Step 4: Configure ModSecurity

Create the main ModSecurity configuration file:

```bash
cat > /etc/httpd/conf.d/mod_security.conf << 'EOF'
<IfModule mod_security2.c>
    SecRuleEngine On
    SecRequestBodyAccess On
    SecDefaultAction "phase:2,deny,log,status:406"
    SecRequestBodyLimitAction ProcessPartial
    SecResponseBodyLimitAction ProcessPartial
    SecRequestBodyLimit 13107200
    SecRequestBodyNoFilesLimit 131072
    SecPcreMatchLimit 250000
    SecPcreMatchLimitRecursion 250000
    SecCollectionTimeout 600
    SecDebugLog /var/log/httpd/modsec_debug.log
    SecDebugLogLevel 0
    SecAuditEngine RelevantOnly
    SecAuditLogRelevantStatus "^(?:5|4(?!04))"
    SecAuditLogParts ABIJDEFHZ
    SecAuditLogType Serial
    SecAuditLog /var/log/httpd/modsec_audit.log
    SecUploadDir /tmp
    SecTmpDir /tmp
    SecDataDir /tmp
    SecTmpSaveUploadedFiles on
    # Include file for cPGuard WAF
    IncludeOptional /etc/httpd/cpguard.conf
</IfModule>
EOF
```

### Configuration Directives Explained

| Directive | Purpose |
|-----------|---------|
| `SecRuleEngine On` | Enables ModSecurity rule processing |
| `SecRequestBodyAccess On` | Allows inspection of POST request bodies |
| `SecDefaultAction` | Default action when a rule triggers (deny with 406 status) |
| `SecRequestBodyLimit` | Maximum request body size (13 MB) |
| `SecAuditEngine RelevantOnly` | Logs only blocked requests (not all traffic) |
| `SecAuditLogParts ABIJDEFHZ` | Specifies which request/response parts to log |
| `SecAuditLog` | Location of the audit log file |

---

## Step 5: Restart Apache

Create the cPGuard configuration file and restart Apache:

```bash
touch /etc/httpd/cpguard.conf
service httpd restart
```

Verify Apache starts without errors:

```bash
apachectl configtest
systemctl status httpd
```

---

## Step 6: Configure cPGuard Standalone Parameters

If you're installing cPGuard in standalone mode (without a control panel), add these parameters to your cPGuard configuration file:

```bash
waf_server = apache
waf_server_conf = /etc/httpd/cpguard.conf
waf_server_restart_cmd = /bin/systemctl restart httpd.service
waf_audit_log = /var/log/httpd/modsec_audit.log
```

---

## Verification

Test that ModSecurity is loaded:

```bash
httpd -M | grep security
```

Expected output:

```
security2_module (shared)
```

Check the audit log permissions:

```bash
ls -la /var/log/httpd/modsec_audit.log
```

If the file doesn't exist yet, it will be created when the first request is blocked.

---


## Troubleshooting

### Apache Fails to Start

Check Apache error log:

```bash
tail -f /var/log/httpd/error_log
```

Common issues:

- **Missing libmodsecurity**: Verify installation path matches the LoadModule directive
- **Syntax errors**: Run `apachectl configtest` to identify configuration issues
- **Permission denied**: Ensure Apache has read access to ModSecurity library

### ModSecurity Not Blocking Requests

Verify `SecRuleEngine` is set to `On`:

```bash
grep SecRuleEngine /etc/httpd/conf.d/mod_security.conf
```

Check if rules are loaded:

```bash
grep -r "SecRule" /etc/httpd/cpguard.conf
```

### Logs Not Appearing

Ensure log directory exists and is writable:

```bash
mkdir -p /var/log/httpd
chown apache:apache /var/log/httpd
chmod 755 /var/log/httpd
```

## Additional Resources

- [OWASP Core Rule Set](https://github.com/coreruleset/coreruleset)
- [ModSecurity GitHub Repository](https://github.com/SpiderLabs/ModSecurity)
- [Apache Module Documentation](https://httpd.apache.org/docs/current/mod/index.html)
