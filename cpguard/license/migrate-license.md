---
slug: cpguard-migrate-license-new-server-ip
title: How to Migrate a cPGuard License to a New Server or IP Address
tags: [cpguard, license, migration, ip-address, faq, troubleshooting, opsshield]
date: 2022-07-16
description: Step-by-step guide to migrating a cPGuard license to a new server or a changed IP address — including when to uninstall from the old server, how to reissue the license, and how to reactivate it on the new server.
---

# How to Migrate a cPGuard License to a New Server or IP Address

A cPGuard license key is **bound to your server's IP address** at the time of activation. This means that if you move to a new server or your server's main IP address changes, you need to follow a specific migration process to transfer the license correctly. Skipping any step will result in cPGuard stopping with a license error.

This guide covers both scenarios, migrating to a completely new server, and simply changing the IP address of an existing server.

{/* comment */}

---

## When Do You Need to Migrate?

| Scenario | Migration Required? |
|---|---|
| Moving cPGuard to a new server | Yes, follow all 3 steps |
| Old server's main IP address has changed | Yes, follow steps 2 and 3 only |
| Old server has already been decommissioned | Yes, skip step 1, follow steps 2 and 3 |
| Adding a secondary IP (main IP unchanged) | No, migration not needed |

:::warning
If you change your server's main IP address **without** following the migration steps below, cPGuard will stop working and display a license error. Do not skip the reissue step even if the server itself hasn't changed.
:::

---

## Migration Steps

### Step 1 : Uninstall cPGuard from the Old Server

If you are migrating the license to a **new server**, uninstall cPGuard from the old server before proceeding.

:::note
You can **skip this step** in either of the following cases:
- The **old server has already been decommissioned** or is no longer accessible
- You are **only changing the main IP address** of the same server (not moving to a new machine)
:::

To uninstall cPGuard from the old server, refer to the [cPGuard Uninstall guide](../getting-started/uninstall-cpguard) for the appropriate steps for your control panel or standalone setup.

---

### Step 2 : Reissue the License Key

Before the license can be activated on a new server or IP address, it must be **reissued** from the OPSSHIELD client portal. Reissuing releases the license from its current IP binding so it can be applied to a new server or updated IP.

Follow the complete reissue process in the dedicated guide:

[How to Reissue a cPGuard License](../license/reissue-license/)

In summary, the reissue steps are:
1. Log in to the [OPSSHIELD Client Portal](https://manage.opsshield.com/index.php/client/login/)
2. Find your license under **Services** and click the **gear icon**
3. Navigate to **Manage License → check "Reissue" → Save**

---

### Step 3 : Apply the License on the New Server or IP

Once the license has been reissued, SSH into your new (or updated) server and apply the license key:

```bash
cpgcli license --key LICENSE-KEY
```

Replace `LICENSE-KEY` with your actual cPGuard license key. The key will be verified against the OPSSHIELD licensing server and bound to your new server's IP address.

:::tip
If you are activating on a brand-new server, make sure cPGuard is fully installed before running this command. Refer to the [Installation guide](../getting-started/installation/) if needed.
:::


---

## What Happens If You Skip the Reissue Step?

If you change your server's main IP address or move to a new server without reissuing the license first, cPGuard will detect an IP mismatch between the stored license and the current server. This results in:

- cPGuard entering a **license error state**
- Security modules (scanner, WAF) potentially stopping
- The server appearing as unlicensed in the App Portal

The fix is always the same. Reissue the license and reapply it. There is no workaround.

---


