---
title: Install PHP Modules Using PECL
description: Learn how to install third-party PHP extensions not listed in the cPGuard X GUI using PECL — including handling the disable_functions error, configuring PHP to load the extension, and removing extensions when no longer needed.
tags: [php, cpguard-x, pecl, php-extensions, php-modules, server-management]
---
# Install Additional PHP Modules Using PECL

cPGuard X allows you to enable many PHP modules directly from the GUI, but for third-party or less common extensions not listed there, you can install them manually using **PECL** (PHP Extension Community Library). This guide walks through the full process — from installation to configuration and removal.

{/* comment */}

:::info[GUI vs PECL]
If the extension you need is already listed in the cPGuard X GUI, it is easier to enable it from **Settings → PHP Modules** without SSH. See [Enabling PHP Extensions in the GUI](/cpguard-x/php/enable-php-module) for those steps.

Use this guide for extensions **not available** in the GUI, including third-party modules.
:::

---

## What is PECL?

PECL (PHP Extension Community Library) is a repository of PHP extensions that provide additional functionality such as performance enhancements, database drivers, image processing libraries, caching backends, and more. It allows you to compile and install PHP extensions directly from source.

PECL is **pre-installed** on your cPGuard X server and no additional setup is required before following the steps below.

---

## PHP Version Path Reference

The PECL binary path and PHP-FPM service name vary depending on the PHP version you are using. Use the commands below that match the version installed on your server:

| PHP Version | PECL Binary Path | FPM Service Name |
|---|---|---|
| PHP 8.1 | `/opt/cpguard/packages/php81/bin/pecl` | `php81-fpm` |
| PHP 8.2 | `/opt/cpguard/packages/php82/bin/pecl` | `php82-fpm` |
| PHP 8.3 | `/opt/cpguard/packages/php83/bin/pecl` | `php83-fpm` |
| PHP 8.4 | `/opt/cpguard/packages/php84/bin/pecl` | `php84-fpm` |

The examples in this guide use **PHP 8.4**. Adjust the version prefix to match yours.

---

## Installing a PECL Extension

### 1. Access the Server via SSH

Log in to your server as the **root user** using an SSH client (such as PuTTY or your terminal).

### 2. Install the Compiler

PECL extensions are compiled from source, so the required build tools must be installed first. Run the following command:

```bash
export DEBIAN_FRONTEND=noninteractive
sudo apt-get -y install gcc g++ make autoconf libc-dev pkg-config
```

### 3. Install the Extension

Run the PECL install command, replacing `EXTENSION` with the name of the extension you want to install:

```bash
/opt/cpguard/packages/php84/bin/pecl install EXTENSION
```

**Example — installing `imagick`:**

```bash
/opt/cpguard/packages/php84/bin/pecl install imagick
```

---

### Handling the `popen()` Error

When running the install command, you may encounter the following error:

```
PHP Fatal error: Uncaught Error: Call to undefined function popen() in
/opt/cpguard/packages/php84/lib/php/OS/Guess.php:306
```

**Why this happens:** The `popen()` function (along with `exec()`, `system()`, `shell_exec()`, and others) is disabled in `php.ini` by default as a security measure to prevent command injection or arbitrary code execution on the server.

**To resolve this**, temporarily comment out the `disable_functions` directive:

**a)** Open the `php.ini` file in a text editor:

```bash
sudo nano /opt/cpguard/packages/php84/etc/php.ini
```

**b)** Locate the `disable_functions` line and comment it out by adding a `;` at the beginning:

```ini
;disable_functions = exec,system,shell_exec,passthru,popen,proc_open,pcntl_exec,dl,eval
```

**c)** Save the file and re-run the PECL install command.

:::danger Re-enable Security After Installation
Once the extension installation is complete, **immediately uncomment** the `disable_functions` line in `php.ini` to restore the security protections. Leaving these functions enabled is a security risk.
:::

---

### 4. Configure PHP to Load the Extension

After installation, tell PHP to load the new extension by creating a dedicated `.ini` file in the `conf.d` directory:

```bash
sudo bash -c "echo extension=EXTENSION.so > /opt/cpguard/packages/php84/etc/conf.d/EXTENSION.ini"
```

**Example — for `imagick`:**

```bash
sudo bash -c "echo extension=imagick.so > /opt/cpguard/packages/php84/etc/conf.d/imagick.ini"
```

### 5. Restart PHP-FPM

Restart the PHP-FPM service to apply the changes:

```bash
sudo service php84-fpm restart
```

The extension is now installed and active.

---

### 6. Verify Installed PECL Extensions

To list all PECL extensions currently installed for a specific PHP version:

```bash
/opt/cpguard/packages/php84/bin/pecl list
```

---

## Removing a PECL Extension

If you no longer need an extension, uninstall it with:

```bash
/opt/cpguard/packages/php84/bin/pecl uninstall EXTENSION
```

**Example — removing `imagick`:**

```bash
/opt/cpguard/packages/php84/bin/pecl uninstall imagick
```

You may see the following message during uninstall:

```
Unable to remove "extension=EXTENSION.so" from php.ini
```

:::note
This message can be **safely ignored**. It simply means PECL could not automatically update `php.ini`, but since the extension was loaded via a dedicated `conf.d` ini file, the removal will still take effect after a PHP-FPM restart.
:::

Restart PHP-FPM to apply the removal:

```bash
sudo service php84-fpm restart
```

---

## Quick Reference

| Task | Command (PHP 8.4 example) |
|---|---|
| Install an extension | `/opt/cpguard/packages/php84/bin/pecl install EXTENSION` |
| List installed extensions | `/opt/cpguard/packages/php84/bin/pecl list` |
| Load extension in PHP | `echo extension=EXTENSION.so > .../conf.d/EXTENSION.ini` |
| Restart PHP-FPM | `sudo service php84-fpm restart` |
| Remove an extension | `/opt/cpguard/packages/php84/bin/pecl uninstall EXTENSION` |

---

## Summary

PECL gives you the flexibility to install any PHP extension your application requires, even when it is not available through the cPGuard X GUI. The key steps are: install build tools → run `pecl install` → create a `conf.d` ini file → restart PHP-FPM. Remember to temporarily disable and then re-enable `disable_functions` in `php.ini` if you encounter the `popen()` error during installation.

---

## Related Guides

- [Enabling PHP Extensions in the GUI](/cpguard-x/php/enable-php-module) — toggle extensions available in the control panel without SSH
- [Managing PHP Settings in the Control Panel](/cpguard-x/php/php-settings) — per-domain `php.ini` and PHP-FPM configuration
- [Global PHP Settings via Panel Settings](/cpguard-x/php/global-php-settings) — server-wide PHP defaults
