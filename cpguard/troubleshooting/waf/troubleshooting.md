---
title: Find the WAF Rule Blocking a Request
sidebar_position: 3
---

Use this page when a valid request is blocked with a 403 or 406 response and you need the WAF Rule ID before deciding whether to whitelist it.

## Use Watch Mode

Run the WAF log watcher:

```bash
cpgcli waf --watch
```

When prompted, enter a filter that matches the incident:

- Client IP address
- Domain name
- URI or path

Reproduce the blocked request after the watcher is running. The output shows the matched rule, including the Rule ID.

If the request is safe, whitelist only that Rule ID. See [Whitelist Rules](../../waf/whitelist-rules.md).

## Use App Portal Logs

1. Log in to App Portal and select the server.
2. Go to **Security** >> **WAF Logs** or **Bot Attacks**.
3. Filter by time, domain, client IP, or Rule ID.
4. Open the blocked event and note the Rule ID.

## Confirm the Fix

After whitelisting a rule:

1. Wait 1-2 minutes for the configuration to apply.
2. Reproduce the same request.
3. Confirm the request succeeds and the Rule ID no longer appears for that request.

## Related Issues

- WAF does not enable: see [Panel-Specific Steps](../../waf/panel-specific-steps.md).
- Uploads or forms fail with a 413 response: see [Request Body Limit Error](request-body-limit.md).
- PHP uploads are blocked: see [Block PHP Uploads](../../waf/block-php-uploads.md).
- Logs show proxy IPs instead of visitor IPs: see [Proxy IP Check](../../waf/proxy-ip-check.md).

If the issue continues, contact support with the Rule ID, affected URL, expected behavior, server details, and control panel details.
