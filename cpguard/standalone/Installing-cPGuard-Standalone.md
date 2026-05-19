---
id: cpguard-standalone-installation
title: How to Install cPGuard Standalone
description: Complete guide to installing and configuring cPGuard on standalone servers without control panels
keywords: [cpguard, installation, standalone, modsecurity, web security]
authors: [ops-team]
tags: [cpguard, security, installation, standalone-servers]
sidebar_position: 2
---

# How to Install cPGuard Standalone

## Overview

cPGuard provides default support for three major control panels: **cPanel**, **DirectAdmin**, and **Plesk**. However, if you're running a server without one of these control panels or using an alternative control panel, you'll need to follow the cPGuard standalone installation procedure.

The standalone installation process is very similar to the standard installation method, with one key difference: you'll need to provide additional parameter values that cPGuard requires to identify and secure your server resources.

:::info
**Who needs standalone installation?**
- Servers running Webmin/Virtualmin
- Servers without any control panel
- Custom server environments
:::

## Installation Command

The cPGuard standalone installation uses the same base command as the standard installation, with optional parameters for unattended deployment.

### Basic Command

```bash
cd /usr/local/src && rm -f cpguard_install.sh && curl -o cpguard_install.sh -L https://downloads.opsshield.com/cpguard/cpguard_install.sh && bash cpguard_install.sh LICENCE-KEY INI-PATH
```

### Command Parameters

| Parameter | Description | Required | Example |
|-----------|-------------|----------|---------|
| **LICENCE-KEY** | Your purchased cPGuard license key that binds your server to the App Portal | Yes | `ABC123XYZ789` |
| **INI-PATH** | Path to your preconfigured `cpguard.ini` configuration file for unattended installation | No | `/root/cpguard.ini` |

:::note
- The `LICENCE-KEY` is mandatory and must be valid
- The `INI-PATH` parameter is optional and used only for automated deployments
- If providing an INI-PATH, it must be a complete file path on the same server
- The configuration file will be copied to `/opt/cpguard/cpguard.ini` during installation
:::

## Installation Methods

### Method 1: Interactive Installation

For manual setup where you'll provide configuration details during installation:

```bash
cd /usr/local/src && rm -f cpguard_install.sh && curl -o cpguard_install.sh -L https://downloads.opsshield.com/cpguard/cpguard_install.sh && bash cpguard_install.sh YOUR_LICENSE_KEY
```

Replace `YOUR_LICENSE_KEY` with your actual license key.

**Steps:**
1. The installer will download and initialize
2. You'll be prompted to enter server configuration details
3. Provide paths and parameters as requested
4. Wait for the installation to complete
5. Verify the installation was successful

### Method 2: Unattended Installation

For automated deployment using a pre-configured `cpguard.ini` file:

```bash
cd /usr/local/src && rm -f cpguard_install.sh && curl -o cpguard_install.sh -L https://downloads.opsshield.com/cpguard/cpguard_install.sh && bash cpguard_install.sh YOUR_LICENSE_KEY /path/to/cpguard.ini
```

**When to use:**
- Automated server deployments
- When managing multiple servers
- In CI/CD pipelines
- For scripted infrastructure setup

:::tip
Before running unattended installation, prepare your `cpguard.ini` file with all required configuration parameters. See [How to Modify Standalone Configuration File cpguard.ini](./cpguard-standalone-config) for detailed configuration instructions.
:::

## Step-by-Step Installation Guide

### Prerequisites

- Root or sudo access to your server
- Valid cPGuard license key
- Internet connectivity for downloading the installer
- curl or wget installed on your server
- Linux-based operating system (Debian, Ubuntu, CentOS, etc.)

### Installation Steps

**Step 1: Navigate to Source Directory**

```bash
cd /usr/local/src
```

**Step 2: Download the Installer**

```bash
rm -f cpguard_install.sh
curl -o cpguard_install.sh -L https://downloads.opsshield.com/cpguard/cpguard_install.sh
```

**Step 3: Execute the Installer**

For interactive installation:
```bash
bash cpguard_install.sh YOUR_LICENSE_KEY
```

For unattended installation:
```bash
bash cpguard_install.sh YOUR_LICENSE_KEY /path/to/cpguard.ini
```

**Step 4: Follow Prompts (Interactive Mode Only)**

The installer will prompt you for:
- Document root paths for your websites
- Virtual host configurations
- Apache/Nginx web server type
- Log file locations
- Other server-specific parameters

**Step 5: Verify Installation**

After installation completes, verify that cPGuard is running:

```bash
ps aux | grep cpguard
```

Check the cPGuard status:
```bash
systemctl status cpguard
```

Or if using init.d:
```bash
/etc/init.d/cpguard status
```

## Configuration After Installation

Once installed, you may need to:

1. **Modify the configuration file**: Edit `/opt/cpguard/cpguard.ini` to fine-tune settings
2. **Enable ModSecurity rules**: Configure your desired security rule sets
3. **Set up logging**: Configure where cPGuard logs are stored
4. **Test your installation**: Verify cPGuard is protecting your websites

 

## Common Installation Scenarios

### Installing on Webmin/Virtualmin

For Webmin/Virtualmin servers, follow the [How to Secure the Websites on a Webmin/Virtualmin Server Using cPGuard](./webmin-virtualmin-cpguard) guide.

### Installing with Nginx

If you're using Nginx instead of Apache, you'll need ModSecurity configured for Nginx. See [Install ModSecurity with Nginx on Debian/Ubuntu](./modsecurity-nginx-debian-ubuntu).

### Installing on CWP (Control Web Panel)

For CWP servers, follow the [Secure Websites on Control Web Panel/CWP Server](./cwp-cpguard) guide.

### Installing on Enhance Panel

For Enhance control panel servers, see [Installing cPGuard on Enhance Panel](./enhance-cpguard).

### Installing on Interworx

For Interworx servers, refer to [Secure Websites on Interworx Control Panel](./interworx-cpguard).

## Troubleshooting

### License Key Issues

**Error: Invalid license key**
- Verify your license key is correct
- Ensure your license hasn't expired
- Check that the key matches your server

### Installation Fails

**Error: curl: command not found**
- Install curl: `sudo apt-get install curl` (Debian/Ubuntu) or `sudo yum install curl` (CentOS)

**Error: Permission denied**
- Ensure you're running the installer as root or with sudo
- Check file permissions in `/usr/local/src`

### Configuration Not Applied

- Verify the INI file path is correct and readable
- Ensure the INI file is properly formatted
- Check file permissions on the INI file

## Next Steps

After successful installation:

1. **Verify protection**: Test that cPGuard is blocking malicious requests
2. **Configure rules**: Customize ModSecurity rules for your needs
3. **Set up monitoring**: Configure log monitoring and alerts
4. **Register in App Portal**: Link your installation to your [OPSSHIELD App Portal](https://app.opsshield.com)
5. **Review logs**: Monitor `/opt/cpguard/logs/` for security events

 
---
 