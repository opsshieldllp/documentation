---
slug: cpguard-license-abuse-detected
title: "cPGuard License Abuse Is Detected"
tags: [cpguard, license, faq, troubleshooting, opsshield, error-fix]
description: Learn what the "cPGuard License abuse is detected" error means, why it appears, and the exact steps to resolve it so you can activate your legitimate cPGuard license.
---


If you've tried to apply a cPGuard license on a server and received the following error, this guide explains exactly what it means and how to resolve it:

:::danger[License abuse error] 
**cPGuard License abuse is detected and Trial license is not allowed. Please contact support for further assistance.**
:::

{/* comment */}

---

## What Does This Error Mean?

This error is triggered when cPGuard detects that the server was previously running a **cracked or nulled version of cPGuard** , commonly distributed as a "shared license" by unofficial third-party vendors.

Cracked cPGuard packages typically work by redirecting the license verification requests away from the legitimate OPSSHIELD licensing servers. They do this by adding fraudulent entries for `opsshield.com` and `malware.expert` into the server's `/etc/hosts` file, effectively hijacking the license check.

When you attempt to activate a genuine license on such a server, cPGuard identifies the presence of these redirects and blocks activation to protect the integrity of the licensing system and to protect your server from the security risks that come with running unknown, modified software.

:::warning
Running a cracked version of cPGuard on your server is a significant security risk. The source and content of unofficial packages are unknown. They may contain backdoors, modified malware definitions, or other malicious code. It also puts your server in violation of the cPGuard license terms.
:::

---

## How to Fix the License Abuse Error

Resolving this error requires two things: removing the fraudulent `/etc/hosts` entries and uninstalling any packages that were installed by the cracked license vendor.

### Step 1 : Remove Fraudulent `/etc/hosts` Entries

Open the `/etc/hosts` file on your server:

```bash
nano /etc/hosts
```

Look for any lines containing `opsshield.com` or `malware.expert` and **delete them**. These entries are not part of a legitimate cPGuard installation and should not exist in your hosts file.

Save and close the file once the entries are removed.

:::tip
You can quickly check whether these entries exist before editing by running:

```bash
grep -E "opsshield.com|malware.expert" /etc/hosts
```

If the command returns any results, those lines need to be removed.
:::

### Step 2 : Uninstall the Cracked License Packages

Identify and uninstall any packages that were provided by the third-party vendor to activate the cracked license. These packages are responsible for injecting the `/etc/hosts` entries in the first place. If you only remove the hosts entries without uninstalling the packages, they will be re-added automatically and the error will return.

:::warning
If you are unsure which packages were installed by the cracked license vendor, contact OPSSHIELD support for assistance. They can help identify and safely remove the offending packages without disrupting your server.
:::

### Step 3 : Apply Your Legitimate License Key

Once the hosts entries are cleared and the cracked packages are uninstalled, apply your genuine cPGuard license using:

```bash
cpgcli license --key LICENSE-KEY
```

Replace `LICENSE-KEY` with your actual license key purchased from OPSSHIELD. The license will be verified against the real licensing server and your server will be activated successfully.


---

## Still Having Issues?

If you have followed all three steps and the error persists, or if you need help identifying which packages to remove, contact the OPSSHIELD support team directly. You can raise a ticket from within the client portal:

[https://manage.opsshield.com](https://manage.opsshield.com/index.php/client/plugin/support_manager/client_tickets/departments/)

---
