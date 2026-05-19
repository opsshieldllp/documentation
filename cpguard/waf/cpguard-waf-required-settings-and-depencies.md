---
title: cPGuard WAF Required Settings and Dependencies
sidebar_position: 1
description: Complete guide to setting up cPGuard WAF with required dependencies, ModSecurity configuration, and control panel-specific settings.
slug: cpguard-waf-required-settings-and-depencies
authors: [opsshield]

---

# cPGuard WAF Required Settings and Dependencies

## Overview

cPGuard WAF is a powerful ModSecurity ruleset designed to protect your web applications from various cyber threats. It works seamlessly with Apache, Nginx, and Litespeed web servers. Since cPGuard WAF is built on top of the ModSecurity module, there are specific dependencies and configurations required for optimal operation.

This guide covers the essential requirements and setup instructions for different hosting control panels and environments.

## Core Requirements

Before enabling cPGuard WAF, your server must meet these baseline requirements:

| Requirement | Status | Details |
|---|---|---|
| **ModSecurity Version** | Required | 2.9.4 or higher must be installed and enabled |
| **SecRuleEngine** | Required | Must be turned ON to process security rules |
| **OWASP Rules** | Disabled | Must be disabled as it's incompatible with cPGuard WAF |
| **SecAuditLog** | Recommended | Enable for WAF logging and monitoring (not required for blocking) |

### Key Points

- **ModSecurity 2.9.4+**: The foundation for cPGuard WAF functionality
- **Rule Engine**: Must be active to enforce security policies
- **Rule Compatibility**: OWASP and cPGuard rules cannot run simultaneously
- **Logging**: While optional for protection, audit logs are essential for security monitoring and debugging

---

## cPanel Configuration

Navigate to **Home >> Security Center >> ModSecurity™ Configuration >> Configure Global Directives**

Set these values:

**Audit Log Level (SecAuditEngine)**
- Select: "Only log noteworthy transactions" (recommended)
- This optimizes logging by capturing only important security events

**Rules Engine (SecRuleEngine)**
- Select: "Process the rules"
- This activates the rule processing engine

![Logo](../../assets/img/cpguard/waf/whm_modsec_conf.png)


:::warning
Do not enable additional ModSecurity vendor rules from WHM >> Security Center >> ModSecurity™ Vendors » Manage Vendors to avoid conflict with cPGuard WAF
:::

### Configuration Reference

Here's what your ModSecurity configuration should look like:

```
SecRuleEngine On
SecAuditEngine On
SecAuditLogRelevantStatus "^(?:2|3|4|5)"
SecAuditLogParts ABDEFHIJZ
```

---

## ConfigServer ModSecurity Control (CMC)

If you're using ConfigServer ModSecurity Control (CMC), ensure ModSecurity remains enabled:

**Do Not Disable ModSecurity in CMC**

- CMC should have ModSecurity turned ON
- This maintains the security layer for cPGuard WAF
- Check CMC regularly to ensure ModSecurity status remains active

![Logo](../../assets/img/cpguard/waf/cmc_modsec_on.png)

---

## DirectAdmin Configuration

For DirectAdmin users, enable ModSecurity through the CustomBuild system.

### Installation Steps

```bash
cd /usr/local/directadmin/custombuild
./build update
./build set modsecurity yes
./build set modsecurity_ruleset "no"
./build modsecurity
./build modsecurity_rules
./build rewrite_confs
```

You should enable ModSecurity in your Web Server without any additional rules. The CWAF rules are compatible and you can enable that optionally if you would like.



### Verify SecRuleEngine Status

Navigate to **DirectAdmin >> Server Manager >> ModSecurity**

Ensure **SecRuleEngine is enabled** (turned ON)

![Logo](../../assets/img/cpguard/waf/da_secengine_on.png)


---

## Plesk ModSecurity Setup

Plesk requires platform-specific commands to enable ModSecurity with the correct ruleset.

### Prerequisites

- Do NOT use OWASP rules as they conflict with cPGuard WAF
- Choose either Apache or Nginx (not both simultaneously)

### For Apache Web Server

Run this command via SSH or Plesk terminal:

```bash
plesk bin server_pref --update-web-app-firewall \
  -waf-rule-engine on \
  -waf-web-server apache \
  -waf-rule-set custom \
  -waf-archive-path /opt/cpguard/app/resources/cpg_modsec_enable.conf.zip
```

### For Nginx Web Server

Run this command via SSH or Plesk terminal:

```bash
plesk bin server_pref --update-web-app-firewall \
  -waf-rule-engine on \
  -waf-web-server nginx \
  -waf-rule-set custom \
  -waf-archive-path /opt/cpguard/app/resources/cpg_modsec_enable.conf.zip
```

Once ModSecurity is enabled using one of the above commands, you can turn on cPGuard WAF from Settings. 

As mentioned above, you can optionally enable Comodo WAF rules instead of our custom ModSecurity rules set if you prefer to use them as well.

---

## CyberPanel Configuration

CyberPanel requires ModSecurity settings to be configured through the web interface.

### Step 1: Configure ModSecurity Settings

Navigate to **CyberPanel >> Server >> Security >> ModSecurity Conf**

Apply these settings:

| Setting | Value | Notes |
|---|---|---|
| **Enable ModSecurity** | ON | Activates the security module |
| **SecRuleEngine** | ON | Processes security rules |
| **SecAuditEngine** | ON | Enables audit logging |
| **SecAuditLogParts** | ABIJDEFHZ | Captures comprehensive log data |

![Logo](../../assets/img/cpguard/waf/cyberpanel_modsec.png)


### Step 2: Disable OWASP Rules

Navigate to **ModSecurity Rule Packs**

Ensure **"OWASP ModSecurity Core Rules" is NOT enabled** (⚠️ incompatible with cPGuard WAF)

### Step 3: Configure Server Hostname

This is critical for WAF health verification:

1. Set a **fully qualified hostname** (example: `server.yourdomain.com`)
2. Ensure it **resolves to your server's IP address**
3. This allows test attacks to verify WAF functionality

**Example hostname configuration:**
```
server.yourdomain.com → 192.0.2.1
```

---

## Control Web Panel (CWP) Configuration

CWP requires both UI configuration and direct file editing.

### Step 1: Enable ModSecurity in CWP

Navigate to **CWP >> Security >> Mod Security**

Enable ModSecurity through the control panel interface.

![Logo](../../assets/img/cpguard/waf/cpg_cwp_modsec1.png)


### Step 2: Edit Main ModSecurity Configuration

Now edit the Main ModSec Configuration file (/usr/local/apache/conf.d/mod_security.conf ) from UI or command line and match it like the following


![Logo](../../assets/img/cpguard/waf/cwp_cpg_modsecconf.png)


### Step 3: Complete Standalone Configuration

Once ModSecurity is configured as above, refer to the [CWP Standalone Configuration guide](../getting-started/cwp.md) to finalize the setup.

---

## Litespeed with Enhance Control Panel

Litespeed requires configuration through both the control panel and the LSWS Web Admin Console.

### Step 1: Control Panel Configuration (if applicable)

Navigate to **Configuration >> Server >> Security**

Set the ModSecurity ruleset with:

```
Enable ModSecurity: Yes
```

### Step 2: LSWS Web Admin Console Configuration

Access the LSWS Web Admin Console and configure WAF settings.

**Enable Web Application Firewall (WAF)**

Navigate to **Configuration >> Server >> Security**

Configure these WAF settings:

| Setting | Value | Description |
|---|---|---|
| **Enable WAF** | Yes | Activates the firewall module |
| **Log Level** | 0 | Captures all log levels |
| **Default Action** | deny,log,status:403 | Denies requests and logs with 403 status |
| **Scan Request Body** | Yes | Scans POST/PUT request bodies |
| **Temporary File Path** | /tmp | Location for temporary files |
| **Enable Security Audit Log** | Native Audit Log | Uses native Litespeed logging |
| **Security Audit Log** | $SERVER_ROOT/logs/security_audit.log | Log file path |
| **Disable .htaccess Override** | Not Set | Allows .htaccess processing |

![Logo](../../assets/img/cpguard/waf/Enhance_lsws_modsec_enable.png)


### Step 3: Add cPGuard WAF Ruleset

Navigate to **Configuration >> Server >> Security >> WAF Rule Set**

Click **"Add"** and configure:

| Field | Value |
|---|---|
| **Name** | cPGuard |
| **Action** | deny,log,status:403 |
| **Enabled** | Yes |
| **Rules Definition** | Include $SERVER_ROOT/conf/cpguard.conf |


![Logo](../../assets/img/cpguard/waf/Enhance_lsws_modsec_addruleset.png)


**Complete Rules Definition:**
```
Include $SERVER_ROOT/conf/cpguard.conf
```

### Verification

After configuration:

1. Restart Litespeed
2. Monitor security audit logs at `/usr/local/lsws/logs/security_audit.log`
3. Verify WAF is processing requests

---

## Troubleshooting Common Issues

### WAF Settings Errors

If you encounter WAF settings errors:

1. **Verify ModSecurity version**: Ensure 2.9.4 or higher is installed
2. **Check SecRuleEngine**: Confirm it's set to ON
3. **Disable conflicting rules**: Remove OWASP rules if enabled
4. **Review logs**: Check ModSecurity audit logs for configuration issues

### OWASP Rules Conflict

If OWASP rules are enabled alongside cPGuard:

- The two rulesets are incompatible
- Disable OWASP rules immediately
- Restart the web server after changes

### Audit Log Issues

If audit logs aren't appearing:

1. Verify `SecAuditEngine` is ON
2. Check log file permissions
3. Ensure log directory exists and is writable
4. Review log file path in configuration

### Performance Degradation

If you notice performance issues after enabling WAF:

1. Review audit log level settings
2. Consider adjusting `SecAuditLogRelevantStatus`
3. Monitor resource usage
4. Check for rule false positives

---

## Best Practices

### Security Monitoring

- **Enable audit logging**: Always capture WAF events for analysis
- **Review logs regularly**: Monitor security_audit.log for patterns
- **Whitelist false positives**: Use rule exclusion mechanisms carefully
- **Keep ModSecurity updated**: Apply security patches promptly

### Configuration Management

- **Document your setup**: Maintain records of all WAF configurations
- **Test before production**: Verify WAF rules in test environment first
- **Back up configurations**: Keep copies of all ModSecurity configs
- **Version control**: Track configuration changes over time

### Performance Optimization

- **Adjust log levels appropriately**: Balance logging detail with server load
- **Monitor request body limits**: Configure based on your application needs
- **Use rule whitelisting carefully**: Only exempt rules when necessary
- **Regular health checks**: Verify WAF functionality periodically

---

## Summary Checklist

Use this checklist to ensure your cPGuard WAF setup is complete:

- [ ] ModSecurity 2.9.4 or higher installed and enabled
- [ ] SecRuleEngine set to ON
- [ ] OWASP rules disabled
- [ ] SecAuditLog enabled (recommended)
- [ ] Control panel-specific settings applied
- [ ] Web server restarted
- [ ] Audit logs verified
- [ ] Hostname properly configured (if applicable)
- [ ] Initial WAF rules loaded
- [ ] Test rules executed successfully

---

## Next Steps

After completing the requirements and configuration:

1. **Enable cPGuard WAF** through your control panel or settings
2. **Monitor initial deployment** for any blocking of legitimate traffic
3. **Review WAF logs** regularly for security events
4. **Whitelist rules** if legitimate traffic is blocked
5. **Keep documentation** of any customizations

---

 
 