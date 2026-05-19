---
title: Secure Websites on Interworx Control Panel with cPGuard
description: Complete guide to installing and configuring cPGuard on your Interworx server for comprehensive web security protection
keywords: [Interworx, cPGuard, web security, server security, WAF, ModSecurity]
authors: [opsshield]
tags: [security, server-management, interworx, cpguard]
date: 2023-10-02
---

# Securing Your Interworx Server with cPGuard

Protecting your web hosting infrastructure is critical in today's threat landscape. If you're running an Interworx Control Panel, cPGuard provides comprehensive security across multiple layers. This guide walks you through the installation and configuration process.

## What is Interworx Control Panel?

InterWorx is a robust web hosting control panel designed to simplify server management for hosting providers. It features a dual-interface architecture:

- **NodeWorx**: Server administration interface for managing the entire server and its configurations
- **SiteWorx**: Individual website management interface for account holders
- **Reseller Support**: NodeWorx also enables web hosting resellers to manage multiple SiteWorx accounts while maintaining security boundaries

Whether you're managing a dedicated server or reselling hosting services, Interworx provides the control you need—and cPGuard adds the security layer you require.

## Why cPGuard for Interworx?

cPGuard is a comprehensive web security suite that operates across multiple layers to protect your infrastructure:

- **Multi-layer Protection**: Defense at application, network, and file system levels
- **ModSecurity Integration**: Industry-standard WAF (Web Application Firewall) technology
- **Automated Configuration**: Intelligent setup that detects your Interworx environment automatically
- **Standalone Flexibility**: Convert to standalone mode for advanced customization if needed

## Installation Requirements

Before proceeding, ensure you have:

- Root or sudo access to your Interworx server
- A valid cPGuard license key
- Internet connectivity for downloading installation packages
- Basic command-line familiarity

## Step-by-Step Installation Guide

### 1. Download and Execute the Installation Script

Run the following command on your Interworx server:

```bash
cd /usr/local/src && rm -f cpguard_install.sh && curl -o cpguard_install.sh -L https://downloads.opsshield.com/cpguard/cpguard_install.sh && bash cpguard_install.sh LICENCE-KEY
```

**Replace `LICENCE-KEY`** with your actual purchased license key. This is mandatory to complete the installation and bind your server to the [OPSSHIELD App Portal](https://app.opsshield.com/).

### 2. Automatic Dependency Installation

The installer script will automatically:

- Detect your operating system (Debian/Ubuntu, CentOS, etc.)
- Install required dependency packages
- Configure the security modules
- Verify the installation

This process typically completes within 5-10 minutes depending on your system.

### 3. Automatic Configuration

The installation script performs automatic configuration for:

- **Web Server Configuration**: Detects and configures your web server (Apache, Nginx, etc.)
- **WAF Configuration**: Sets up ModSecurity rules and policies

In most cases, **no manual configuration is required**—the installer handles everything intelligently.

## Post-Installation Verification

Once installation completes successfully:

1. **Access the App Portal**: Visit [https://app.opsshield.com/](https://app.opsshield.com/)
2. **Verify Server Listing**: Your Interworx server should appear in the connected servers list
3. **Monitor Protection**: Begin monitoring security events and configuring advanced settings

## Advanced Configuration (Optional)

If you need to customize cPGuard settings or convert to standalone mode, use the configuration wizard:

```bash
cpgcli standalone-conf --wizard
```

This is only necessary if you want to:

- Customize security policies
- Fine-tune ModSecurity rules
- Convert to standalone configuration mode
- Manually specify Interworx server details

### When to Use Standalone Mode

Consider standalone configuration when:

- You need granular control over security rules
- You want to customize WAF behavior
- You're managing multiple control panel environments
- You require environment-specific security policies

## Security Configuration Best Practices

### 1. Regular Rule Updates

Keep your ModSecurity rules current by:

- Enabling automatic rule updates in the App Portal
- Monitoring security bulletins
- Testing new rules in detection-only mode first

### 2. Whitelisting and Tuning

After initial deployment:

- Monitor logs for false positives
- Create whitelist rules for legitimate traffic
- Adjust sensitivity levels based on your applications

### 3. Monitoring and Alerting

- Set up notifications for security events
- Review logs regularly
- Track blocked attacks and trends

### 4. Keep Systems Updated

- Update Interworx regularly
- Maintain your operating system patches
- Update cPGuard when new versions are released

## Troubleshooting Common Issues

### Installation Fails

**Problem**: Installation script exits with errors

**Solution**:
- Ensure you have root/sudo privileges
- Check internet connectivity
- Verify your license key is correct
- Review system requirements (supported OS and versions)

### Server Not Appearing in App Portal

**Problem**: Server doesn't show in your OPSSHIELD dashboard after installation

**Solution**:
- Wait 5-10 minutes for synchronization
- Verify your license key was applied correctly
- Check server connectivity to OPSSHIELD infrastructure
- Review `/var/log/cpguard/` for error messages

### False Positives/Blocked Legitimate Traffic

**Problem**: Legitimate website traffic is being blocked

**Solution**:
- Review blocked requests in the App Portal
- Create whitelist rules for specific IPs or patterns
- Adjust ModSecurity paranoia level
- Test rules in detection mode before enforcement

## Next Steps

After securing your Interworx server with cPGuard:

1. **Configure Security Policies**: Customize rules based on your website requirements
2. **Set Up Monitoring**: Enable alerts and log monitoring
3. **Regular Reviews**: Schedule monthly security audits
4. **Stay Informed**: Follow OPSSHIELD blog for security best practices

## Additional Resources

- [cPGuard Standalone Configuration](https://opsshield.com/help/cpguard/cpguard-standalone-configuration/)
- [ModSecurity Configuration Guide](https://opsshield.com/help/cpguard/how-to-modify-standalone-configuration-file-cpguard-ini/)
- [OPSSHIELD Knowledge Base](https://opsshield.com/help/)
- [OPSSHIELD Blog](https://www.opsshield.com/blog)

## Getting Support

If you encounter issues or need assistance:

- **Knowledge Base**: Browse [OPSSHIELD Help Documentation](https://www.opsshield.com/help)
- **Support Tickets**: [Submit a support request](https://manage.opsshield.com/index.php/client/plugin/support_manager/client_tickets/departments/)
- **Contact**: Reach out to the OPSSHIELD team directly

## Conclusion

Securing your Interworx server with cPGuard provides enterprise-grade protection across multiple security layers. The straightforward installation process and automatic configuration mean you can have comprehensive security running within minutes, not hours.

By following this guide and implementing the security best practices outlined, you'll significantly reduce your attack surface and protect your hosted websites from common threats.

---
