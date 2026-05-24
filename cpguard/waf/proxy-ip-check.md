---
title: Proxy IP Check
description: Use Proxy IP Check to detect the real client IP behind CDN and reverse proxy layers.
authors: [technical-team]
---

# Proxy IP Check

Proxy IP Check is a WAF extension that identifies the real visitor IP when traffic comes through Cloudflare, reverse proxies, or load balancers.

Without this module, Layer 3 controls (such as IPDB) often see only the proxy IP. With Proxy IP Check enabled, cPGuard evaluates the extracted client IP and applies block/allow decisions to the actual source.

## When to Enable

Enable this module if your server is behind:

- Cloudflare or another CDN
- Reverse proxies (Nginx, HAProxy, Traefik)
- A load balancer in front of web servers

## Supported Headers

The module checks common real-IP headers:

- `CF-Connecting-IP`
- `True-Client-IP`
- `X-Real-IP`
- `X-Forwarded-For`
- `Fastly-Client-IP`
- `X-Client-IP`
- `X-Cluster-Client-IP`
- `Forwarded`
- `REMOTE_ADDR` (fallback)

## Enable or Disable

Enable:

```bash
cpgcli waf --enable=proxycheck
```

Disable:

```bash
cpgcli waf --disable=proxycheck
```

## Verification

1. Enable the module.
2. Send traffic through your proxy/CDN.
3. Check WAF logs and confirm the client IP is the real visitor IP, not the proxy edge IP.

## Notes

- Use this together with WAF and IPDB for better source attribution.
- Keep proxy/CDN configuration correct so trusted headers are passed reliably.
- If IPs still look incorrect, review [Troubleshooting](troubleshooting.md).
