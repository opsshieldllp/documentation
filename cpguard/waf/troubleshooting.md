---
title: Troubleshooting
sidebar_position: 3
---

This guide helps you identify and resolve WAF blocking issues, including false positives where legitimate requests are mistakenly blocked.

## Understanding WAF Blocking

cPGuard WAF uses ModSecurity rules operating at Layer 7 to analyze request/response bodies, headers, and parameters. When a rule condition matches a request, it triggers an action that blocks the request with a 403 or 406 response code.

Each block includes a **Rule ID** - a unique identifier for the triggered rule. Use this Rule ID to whitelist the rule if you need to allow that type of traffic.

## Why Debug WAF Blocking?

While WAF rules are carefully designed to minimize false positives, certain websites have specific requirements that may trigger legitimate blocks. When this occurs, you need to identify the Rule ID causing the issue so you can whitelist it appropriately.

You can find incidents in WAF/Bot attack logs, but manual troubleshooting provides more immediate feedback.

## Identify Blocking Rules

### Using Watch Mode

Monitor WAF logs in real-time to identify which rule is blocking access.

**Step 1:** Start WAF log monitoring

```bash
cpgcli waf --watch
```

**Step 2:** Enter a filter pattern

When prompted, enter a search pattern such as:
- Your IP address
- Affected domain name
- Affected URI/path

**Step 3:** Recreate the incident

From your browser, reproduce the action that triggers the block. The console will display the triggered rule.

**Example output:**

```
Filter: 192.168.1.100
Watching WAF logs...

[2026-02-23 10:45:23] 
Domain: example.com
URI: /wp-admin/admin-ajax.php
Client IP: 192.168.1.100
Rule ID: 4500006
Message: XSS Attack Detected
Action: Blocked (403)
```

**Step 4:** Take action

You can immediately whitelist the rule from the prompt, or exit and manually review before whitelisting.

:::info

Enter the correct filter pattern **before** recreating the incident for accurate results.
:::

### Using App Portal Logs

**Step 1:** Log in to App Portal → Select your server

**Step 2:** Navigate to **Security** → **WAF Logs** or **Bot Attacks**

**Step 3:** Filter logs by:
- Time range
- Domain
- Client IP
- Rule ID

**Step 4:** Review the blocked request details to identify the Rule ID

## Common Issues

### WAF Not Enabling

**Symptoms:** Cannot enable WAF, error messages in settings

**Causes:**
- ModSecurity version below 2.9.4
- SecRuleEngine not enabled
- OWASP rules enabled (incompatible)
- SecAuditLog not configured

**Resolution:** See [setup requirements](../waf/cpguard-waf-required-settings-and-depencies.md) for your control panel.

### Legitimate Traffic Blocked

**Symptoms:** Users report 403 errors, forms don't submit, uploads fail

**Causes:**
- Rule too aggressive for your application
- Custom application behavior triggers security pattern
- Known false positive for specific use case

**Resolution:**
1. Identify the Rule ID using watch mode or logs
2. Review the rule details
3. Whitelist the rule if safe for your environment
4. See [Whitelist Rules](whitelist-rules.md) for instructions

### High False Positive Rate

**Symptoms:** Many legitimate requests blocked, frequent user complaints

**Causes:**
- WEBSHELL or RBL rule sets too aggressive for your environment
- Custom CMS or framework with unusual patterns

**Resolution:**
1. Disable optional rule sets temporarily:
   ```bash
   cpgcli waf --disable=webshell,rbl
   ```
2. Monitor if issues reduce
3. Whitelist specific Rule IDs instead of disabling entire modules
4. Contact support with Rule IDs for review

### PHP Upload Blocked

**Symptoms:** Cannot upload PHP files via web forms or file managers

**Causes:**
- Block PHP Upload option is enabled
- This is intentional security behavior

**Resolution:**
- Use FTP, SSH, or control panel file manager instead
- Disable the option if required: `cpgcli upload-scanner --block-php=disable`
- See [Block PHP Uploads](block-php-uploads.md) for details

### Request Body Size Limit Exceeded

**Symptoms:** 413 response code, cannot submit forms or upload files

**Error messages in ModSecurity log:**
- `Request body excluding files is bigger than the maximum`
- `Request body no files data length is larger than the configured limit`

**Causes:**
- `SecRequestBodyLimit` value too low
- `SecRequestBodyNoFilesLimit` value too low
- Default ModSecurity limits insufficient for your application

**Resolution:**

Increase the values for these ModSecurity parameters. cPGuard does not enforce these values. They are controlled by your control panel or ModSecurity defaults.

See [Request Body Size Limit Exceeded](Request%20Body%20Exceeds%20Limit) for details

### Changes Not Applied

**Symptoms:** WAF configuration changes don't take effect

**Causes:**
- Configuration updates have a time delay
- Web server not restarted

**Resolution:**
1. Wait 1-2 minutes after configuration change
2. Manually restart web server if needed:
   - Apache: `systemctl restart httpd` or `service httpd restart`
   - Nginx: `systemctl restart nginx` or `service nginx restart`
   - LiteSpeed: `/usr/local/lsws/bin/lswsctrl restart`

### Proxy/CDN Issues

**Symptoms:** Wrong IP addresses shown in logs, cannot block real attackers

**Causes:**
- Server behind CloudFlare or other proxy
- IPDB cannot see real visitor IP at Layer 3

**Resolution:**
- Enable Proxy IP Check rule set:
  ```bash
  cpgcli waf --enable=proxycheck
  ```
- See [Proxy IP Check](proxy-ip-check-waf.md) for details

## Verification Steps

### Test WAF is Working

**Step 1:** Ensure you have a fully qualified hostname pointing to your server IP

**Step 2:** Test with a known attack pattern:

```bash
curl -I "http://yourdomain.com/?test=<script>alert('xss')</script>"
```

**Expected result:** 403 Forbidden response

**Step 3:** Check WAF logs for the blocked request

### Verify Rule Whitelist

After whitelisting a rule:

**Step 1:** Wait 1-2 minutes for configuration to apply

**Step 2:** Recreate the previously blocked request

**Expected result:** Request succeeds without 403 error

**Step 3:** Verify in WAF logs that the rule is no longer triggering

## Getting Help

If issues persist after troubleshooting:

1. Collect the Rule ID from logs
2. Document the legitimate request that's being blocked
3. Check if the request contains unusual patterns
4. Contact support with:
   - Rule ID
   - Full request details (URL, parameters, headers if needed)
   - Expected behavior
   - Server and control panel details

See [Support](../faq/grant-server-access-to-support.md) for contact information.

