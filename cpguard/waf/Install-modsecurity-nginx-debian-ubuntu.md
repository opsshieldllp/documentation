---
id: install-modsecurity-nginx-debian-ubuntu
title: Install ModSecurity with Nginx on Debian/Ubuntu
slug: install-modsecurity-nginx-debian-ubuntu
sidebar_position: 2
tags:
  - Security
  - Web Application Firewall
  - ModSecurity
  - Nginx
  - Debian
  - Ubuntu
  - WAF
keywords:
  - ModSecurity
  - Nginx
  - Web Application Firewall
  - Security
  - Debian
  - Ubuntu
  - Installation Guide
---

# Install ModSecurity with Nginx on Debian/Ubuntu

## Overview

ModSecurity is a powerful open-source Web Application Firewall (WAF) that provides comprehensive protection for web applications against Layer 7 (HTTP) attacks. This guide walks you through installing ModSecurity 3.0 with Nginx on Debian and Ubuntu-based systems.

### What is ModSecurity?

ModSecurity is the most well-known open-source web application firewall originally built for Apache Web Server. It provides comprehensive protection for web applications like WordPress, Joomla, and OpenCart against a wide range of Layer 7 (HTTP) attacks.

ModSecurity can work as a Web Server module and can filter out attacks such as:
- SQL injection
- Cross-site scripting (XSS)
- Local file inclusion (LFI)
- Remote file inclusion (RFI)
- And many other common web-based attacks

:::info
ModSecurity 3.0 is under active development and lacks some features from 2.9.x versions, but it's the only version compatible with Nginx.
:::

### cPGuard WAF

cPGuard WAF is a set of ModSecurity rules designed to block most generic web attacks. It is powered by  [commercial ModSecurity rules from Malware.Expert](https://malware.expert/modsecurity-rules/) and provides protection against targeted and automated attacks with explicit rules for popular CMS platforms like WordPress and Joomla.

## Installation Steps


### Step 1: Install Nginx

If you don't already have Nginx Web Server installed, install it using the following command:

```bash
sudo apt install nginx
```

If you already have Nginx installed, you can skip this step.

### Step 2: Download and Compile ModSecurity

#### Install Build Dependencies

Install all required dependencies for compiling ModSecurity:

```bash
apt-get install libtool autoconf build-essential libpcre3-dev zlib1g-dev libssl-dev libxml2-dev libgeoip-dev liblmdb-dev libyajl-dev libcurl4-openssl-dev libpcre++-dev pkgconf libxslt1-dev libgd-dev automake
```


:::tip[Alternative: Using DigitalWave Repository]
If your server policy allows adding third-party repositories, you can use the DigitalWave package instead of compiling. This is recommended by OWASP and allows you to skip Steps 2, 3, and 4. Install packages from the DigitalWave repository and proceed to "Step 5: Install Nginx Configuration." 
:::

#### Download ModSecurity Source Code

Download the ModSecurity source code from GitHub:

```bash
cd /usr/local/src
git clone --depth 100 -b v3/master --single-branch https://github.com/SpiderLabs/ModSecurity
cd ModSecurity
git submodule init
git submodule update
```

#### Compile and Install ModSecurity

Build and install ModSecurity on your server:

```bash
# Generate configure file
sh build.sh

# Pre-compilation step - checks for dependencies
./configure

# Compile the source code
make

# Install libmodsecurity to /usr/local/modsecurity/lib/libmodsecurity.so
make install
```

### Step 3: Download and Compile ModSecurity Nginx Connector

#### Check Your Nginx Version

First, check your Nginx version to ensure you download the correct source:

```bash
nginx -V
```

#### Download Nginx Source and Connector

Create a working directory and download the Nginx source code and ModSecurity-nginx connector:

```bash
mkdir /usr/local/src/cpg
cd /usr/local/src/cpg

# Download Nginx source (replace version number with your actual Nginx version)
wget http://nginx.org/download/nginx-1.21.4.tar.gz

# Extract the source code
tar -xvzf nginx-1.21.4.tar.gz

# Download the ModSecurity-nginx connector
git clone https://github.com/SpiderLabs/ModSecurity-nginx
```

### Step 4: Compile Nginx with ModSecurity Module

Compile Nginx with the ModSecurity module. The module will be compiled as a dynamic module that is binary-compatible with your existing Nginx installation.

#### Option A: Standard Compilation (Recommended)

If your Nginx package is compiled with the `--with-compat` flag:

```bash
cd nginx-1.21.4
./configure --with-compat --with-openssl=/usr/include/openssl/ --add-dynamic-module=/usr/local/src/cpg/ModSecurity-nginx
```

#### Option B: Using Existing Compile Flags

If your Nginx package is not compatible with `--with-compat`, check your existing compile flags and use them to build the package. Here's an example for CloudPanel:

```bash
cd nginx-1.21.4
./configure \
  --with-cc-opt='-g -O2 -fdebug-prefix-map=/home/clp/packaging/nginx/tmp/nginx-1.21.4=. -fstack-protector-strong -Wformat -Werror=format-security -fPIC -Wdate-time -D_FORTIFY_SOURCE=2' \
  --with-ld-opt='-Wl,-z,relro -Wl,-z,now -fPIC' \
  --prefix=/usr/share/nginx \
  --conf-path=/etc/nginx/nginx.conf \
  --http-log-path=/var/log/nginx/access.log \
  --error-log-path=/var/log/nginx/error.log \
  --lock-path=/var/lock/nginx.lock \
  --pid-path=/run/nginx.pid \
  --modules-path=/usr/lib/nginx/modules \
  --http-client-body-temp-path=/var/lib/nginx/body \
  --http-fastcgi-temp-path=/var/lib/nginx/fastcgi \
  --http-proxy-temp-path=/var/lib/nginx/proxy \
  --http-scgi-temp-path=/var/lib/nginx/scgi \
  --http-uwsgi-temp-path=/var/lib/nginx/uwsgi \
  --with-debug \
  --with-pcre-jit \
  --with-http_ssl_module \
  --with-http_stub_status_module \
  --with-http_realip_module \
  --with-http_auth_request_module \
  --with-http_v2_module \
  --with-http_dav_module \
  --with-http_slice_module \
  --with-threads \
  --with-http_addition_module \
  --with-http_geoip_module=dynamic \
  --with-http_gunzip_module \
  --with-http_gzip_static_module \
  --with-http_image_filter_module=dynamic \
  --with-http_sub_module \
  --with-stream_ssl_module \
  --with-stream_ssl_preread_module \
  --with-mail=dynamic \
  --with-mail_ssl_module \
  --add-dynamic-module=/usr/local/src/cpg/ModSecurity-nginx
```

#### Build the Module

Build the Nginx module and copy it to the Nginx modules directory:

```bash
# Build the module
make modules

# Copy the module to Nginx modules directory
cp objs/ngx_http_modsecurity_module.so /usr/share/nginx/modules/
```

### Step 5: Load ModSecurity Module into Nginx

Create or edit the ModSecurity module configuration file:

```bash
nano /etc/nginx/modules-enabled/50-mod-http-modsecurity.conf
```

Add the following content:

```nginx
load_module modules/ngx_http_modsecurity_module.so;
```

### Step 6: Install Nginx Configuration

#### Update Main Nginx Configuration

Open the main Nginx configuration file:

```bash
nano /etc/nginx/nginx.conf
```

Add the following line after the existing `include "/etc/nginx/sites-enabled/*.conf"` line:

```nginx
include /etc/nginx/cpguard_waf_load.conf;
```

#### Create WAF Load Configuration

Create the WAF load configuration file:

```bash
nano /etc/nginx/cpguard_waf_load.conf
```

Add the following content:

```nginx
modsecurity on;
modsecurity_rules_file /etc/nginx/nginx-modsecurity.conf;
```

#### Create ModSecurity Configuration

Create the ModSecurity configuration file:

```bash
nano /etc/nginx/nginx-modsecurity.conf
```

Add the following content:

```
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

SecDebugLog /var/log/nginx/modsec_debug.log
SecDebugLogLevel 0
SecAuditEngine RelevantOnly
SecAuditLog /var/log/nginx/modsec_audit.log
SecUploadDir /tmp
SecTmpDir /tmp
SecDataDir /tmp
SecTmpSaveUploadedFiles on

# Include file for cPGuard WAF
Include /etc/nginx/cpguard_waf.conf
```

### Step 7: Configure cPGuard WAF Parameters

Once all the above steps are completed successfully, configure the following parameters in your [cPGuard Standalone configuration](../standalone/Modify-standalone-configuration.md) file:

```ini
waf_server = nginx

waf_server_conf = /etc/nginx/cpguard_waf.conf

waf_server_restart_cmd = /usr/sbin/service nginx restart

waf_audit_log = /var/log/nginx/modsec_audit.log
```

## Configuration Explanations

### Key ModSecurity Directives

| Directive | Description |
|-----------|-------------|
| `SecRuleEngine On` | Enables ModSecurity rule processing |
| `SecRequestBodyAccess On` | Allows ModSecurity to access request bodies |
| `SecDefaultAction` | Sets default action for triggered rules (deny with 406 status) |
| `SecRequestBodyLimit` | Maximum request body size (13MB in this example) |
| `SecAuditEngine RelevantOnly` | Logs only relevant transactions |
| `SecAuditLog` | Path to audit log file |
| `SecDebugLog` | Path to debug log file |

## Verification and Testing

After installation, test your configuration:

### Test Nginx Configuration

```bash
sudo nginx -t
```

### Reload Nginx

```bash
sudo systemctl reload nginx
```

### Check ModSecurity Logs

Monitor the ModSecurity audit log:

```bash
tail -f /var/log/nginx/modsec_audit.log
```

## Conclusion

You should now have ModSecurity enabled and protecting your Nginx web server. Once cPGuard WAF is enabled through your configuration, your server will be protected against common web-based attacks including SQL injection, cross-site scripting, and many other Layer 7 threats.

 

 