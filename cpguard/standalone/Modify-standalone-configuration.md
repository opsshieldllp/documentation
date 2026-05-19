---
slug: modify-cpguard-standalone-config
title: How to Modify cPGuard Standalone Configuration File (cpguard.ini)
 
---

## Overview

The cPGuard standalone configuration file (`cpguard.ini`) contains all the settings for your cPGuard installation. If your server configuration changes or you need to adjust cPGuard settings, you can modify this file using one of two methods.

:::info
The configuration file is located at `/opt/cpguard/cpguard.ini` on your server.
:::

## Method 1: Direct File Editing

This method allows you to manually edit specific configuration values.

### Steps

1. **Open the configuration file** using your preferred text editor:

```bash
nano /opt/cpguard/cpguard.ini
```

Or use any other text editor you prefer (vim, vi, etc.):

```bash
vim /opt/cpguard/cpguard.ini
```

2. **Edit the configuration values** you want to change according to your requirements.

3. **Save the file** and exit the editor.

4. **Apply the changes** by running the update command:

```bash
cpgcli standalone-conf --update
```

This command processes your configuration changes and applies them to the running cPGuard installation.

:::tip
Make a backup of your original `cpguard.ini` file before making changes:
```bash
cp /opt/cpguard/cpguard.ini /opt/cpguard/cpguard.ini.bak
```
:::

## Method 2: Using the Reconfigure Wizard

This method allows you to reconfigure all settings interactively through a guided wizard.

### Steps

Run the following command from SSH as the **root user**:

```bash
cpgcli standalone-conf --wizard
```

The wizard will:
- Guide you through the entire standalone configuration process
- Allow you to modify all configuration values interactively
- Display prompts for each configuration option
- Accept your input and update settings in real-time

:::note
This method is ideal if you need to reconfigure multiple settings at once or if you're unsure about specific configuration options, as the wizard provides guidance for each setting.
:::

## When to Use Each Method

| Scenario | Recommended Method |
|----------|-------------------|
| Updating a single configuration value | Method 1: Direct Editing |
| Changing multiple configuration settings | Method 2: Reconfigure Wizard |
| Need guidance on configuration options | Method 2: Reconfigure Wizard |
| Quick configuration updates | Method 1: Direct Editing |
| Fresh reconfiguration of the entire setup | Method 2: Reconfigure Wizard |

## Common Configuration Changes

Some common values you might modify in `cpguard.ini`:

- **Server paths and directories** - Update when moving server files or changing directory structure
- **Port settings** - Modify if you change the ports your services run on
- **Security rules** - Adjust security policies and protection levels
- **Logging preferences** - Change log file locations or verbosity levels
- **Module settings** - Enable or disable specific cPGuard modules

## Verification

After making changes, verify your configuration was applied correctly:

```bash
# Check the status of cPGuard
cpgcli status

# View the current configuration
cat /opt/cpguard/cpguard.ini
```

## Troubleshooting

If you encounter issues after modifying your configuration:

1. **Restore from backup** (if you created one):
```bash
cp /opt/cpguard/cpguard.ini.bak /opt/cpguard/cpguard.ini
cpgcli standalone-conf --update
```

2. **Check for syntax errors** in the configuration file

3. **Review cPGuard logs** for error messages:
```bash
tail -f /var/log/cpguard.log
```

4. **Run the reconfigure wizard** to reset configuration values:
```bash
cpgcli standalone-conf --wizard
```

## Related Articles

- [cPGuard Standalone Configuration](https://opsshield.com/help/cpguard/cpguard-standalone-configuration/)
- [How to Install cPGuard Standalone](https://opsshield.com/help/cpguard/how-to-install-cpguard-standalone/)
- [Secure Websites on Webmin/Virtualmin Server](https://opsshield.com/help/cpguard/how-to-secure-the-websites-on-a-webmin-virtualmin-server-using-cpguard/)

## Need Help?

If you experience any issues modifying your cPGuard configuration:

- Check the [cPGuard Knowledge Base](https://www.opsshield.com/help)
- [Raise a support ticket](https://manage.opsshield.com/index.php/client/plugin/support_manager/client_tickets/departments/)
- Review the [cPGuard Change Log](https://opsshield.com/help/cpguard/version-4/)