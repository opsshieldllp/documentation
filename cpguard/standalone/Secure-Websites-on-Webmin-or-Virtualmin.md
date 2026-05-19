---
title: How to Secure Websites on a Webmin/Virtualmin Server Using cPGuard
description: A comprehensive guide to installing and configuring cPGuard on Webmin/Virtualmin servers for enhanced website security and WAF protection.
authors: [admin]

 
---

# How to Secure Websites on a Webmin/Virtualmin Server Using cPGuard

## Overview

cPGuard Standalone is a powerful security solution that can run on any server with or without control panels. If you're using Webmin/Virtualmin, cPGuard offers built-in profile scripts that simplify the installation and configuration process, allowing you to secure your websites with minimal manual setup.

This guide will walk you through the process of installing and configuring cPGuard on your Webmin/Virtualmin server to enhance your website security and implement Web Application Firewall (WAF) protection.

## What is cPGuard?

cPGuard is a comprehensive security solution designed to protect web applications from various threats. It works as a Web Application Firewall (WAF) that can be integrated with popular web servers and control panels, including Webmin/Virtualmin.

### Key Benefits

- **Platform Agnostic**: Works on servers with or without control panels
- **Easy Integration**: Built-in profile scripts for Webmin/Virtualmin
- **Minimal Configuration**: Automated setup with unattended installation options
- **Multi-Server Support**: Compatible with Apache, Nginx, and Litespeed
- **Rule-Based Protection**: Uses ModSecurity for advanced threat detection

## Prerequisites

Before you begin, ensure you meet the following requirements:

- **Server**: A Webmin/Virtualmin-based server
- **ModSecurity Version**: 2.9.4 or higher installed with Apache, Nginx, or Litespeed
- **Root Access**: Administrative privileges on your server
- **Configuration File**: Access to `/opt/cpguard/cpguard.ini`

:::info
ModSecurity is essential for enabling the WAF (Web Application Firewall) module on your server. If you haven't installed it yet, you'll need to do so before proceeding.
:::

## How cPGuard Works on Webmin/Virtualmin

cPGuard Standalone uses a configuration-driven approach with profile hook scripts that can be plugged in or out as needed. The Webmin/Virtualmin profile scripts are built-in, which means:

1. **Automatic Detection**: The installer recognizes your Webmin/Virtualmin setup
2. **Pre-configured Settings**: Default settings are automatically configured for your control panel
3. **Minimal Manual Work**: You can start with basic setup and customize as needed
4. **Unattended Installation**: If you know the configuration values, you can preload them for a fully automated installation

The primary configuration file is located at:

```
/opt/cpguard/cpguard.ini
```

## Installation Steps

### Step 1: Review the Installation Documentation

Before starting, familiarize yourself with the complete installation process. Refer to the [cPGuard Standalone Installation Guide](https://opsshield.com/help/cpguard/how-to-install-cpguard-standalone/) for detailed instructions.

### Step 2: Prepare Your Configuration

If you plan to use unattended installation, prepare your configuration values in advance. You'll typically need:

- Web server type (Apache/Nginx/Litespeed)
- ModSecurity installation path
- Virtual host configuration details
- Log file locations
- Rule sets to enable

### Step 3: Run the Installation Script

Execute the installation script according to the documentation. The Webmin/Virtualmin profile will be automatically loaded, and the installer will:

- Detect your Webmin/Virtualmin control panel
- Configure ModSecurity integration points
- Set up virtual host protection
- Create necessary log directories
- Initialize the cPGuard daemon

### Step 4: Verify the Installation

After installation completes:

1. Check that the cPGuard service is running
2. Verify ModSecurity is active on your web server
3. Test that virtual hosts are being protected
4. Review the configuration file for any custom adjustments needed

## Advanced Configuration

### Customizing cpguard.ini

The `/opt/cpguard/cpguard.ini` configuration file controls cPGuard's behavior. For detailed customization options, refer to the [cPGuard Configuration Guide](https://opsshield.com/help/cpguard/how-to-modify-standalone-configuration-file-cpguard-ini/).

Common customization areas include:

- **Rule Sets**: Enable/disable specific security rules
- **Log Levels**: Configure logging verbosity
- **Virtual Hosts**: Add or modify protected domains
- **Performance**: Tune caching and performance parameters
- **Alerting**: Configure notifications for detected threats

### ModSecurity Versions

Ensure you're using ModSecurity version 2.9.4 or higher. If you need to upgrade:

- For Apache, see the [ModSecurity 2.9.7 Installation Guide](https://opsshield.com/help/cpguard/how-to-install-latest-modsecurity-2-9-7-in-apache-install-modsecurity-2-9-7-in-apache/)
- For Nginx, see the [ModSecurity with Nginx on Debian/Ubuntu Guide](https://opsshield.com/help/cpguard/install-modsecurity-with-nginx-on-debian-ubuntu/)

## Unattended Installation

If you want to automate the installation process without interactive prompts:

1. Prepare all configuration values in advance
2. Preload them into the configuration file before running the installer
3. Run the installer in unattended mode

This is particularly useful for:

- Deploying cPGuard across multiple servers
- Integrating with infrastructure-as-code tools
- Automating server provisioning workflows
- Reducing human error in configuration

Refer to the [installation documentation](https://opsshield.com/help/cpguard/how-to-install-cpguard-standalone/) for the specific syntax and options.

## Supported Platforms

While this guide focuses on Webmin/Virtualmin, cPGuard Standalone also supports:

- **DirectAdmin**: Dedicated integration available
- **Plesk**: Full support and integration
- **Control Web Panel (CWP)**: Standalone configuration
- **Interworx**: Compatible deployment
- **Enhance Control Panel**: Dedicated installation guide
- **Other Platforms**: Generic standalone mode with manual configuration

## Best Practices

When securing your Webmin/Virtualmin server with cPGuard:

1. **Keep ModSecurity Updated**: Regularly update to the latest ModSecurity version
2. **Monitor Logs**: Regularly review cPGuard logs for attack patterns
3. **Test Rules**: Enable rules gradually and test to avoid false positives
4. **Backup Configuration**: Always backup your `cpguard.ini` before making changes
5. **Document Changes**: Keep notes on any customizations made
6. **Update Rule Sets**: Periodically update security rule sets
7. **Monitor Performance**: Track server performance with WAF enabled

## Troubleshooting

### cPGuard Service Not Starting

- Check that ModSecurity is properly installed
- Verify `/opt/cpguard/cpguard.ini` exists and is readable
- Review system logs for error messages
- Ensure Apache/Nginx is running and configured with ModSecurity

### ModSecurity Not Detecting Threats

- Verify ModSecurity rules are enabled in the configuration
- Check that rule files are present and properly loaded
- Review ModSecurity audit logs
- Ensure you're using ModSecurity version 2.9.4 or higher

### Virtual Hosts Not Protected

- Confirm virtual hosts are configured in cPGuard
- Check Webmin/Virtualmin virtual host configuration
- Verify ModSecurity is enabled per virtual host
- Review configuration syntax for errors



## Conclusion

Securing your Webmin/Virtualmin server with cPGuard is a straightforward process thanks to the built-in profile scripts and automated configuration. By following this guide and the official installation documentation, you can deploy a robust Web Application Firewall in minutes, protecting your websites from common web-based attacks.

The combination of Webmin/Virtualmin's control panel capabilities with cPGuard's WAF protection provides a comprehensive security solution for your hosted applications.