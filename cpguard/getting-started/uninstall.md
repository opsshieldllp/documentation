---
sidebar_position: 4
title: Uninstalling cPGuard
sidebar_label: Uninstall
description: A complete guide to uninstalling cPGuard from your server including the one-line uninstall command, what it removes, pre-uninstall steps for license migration, and how to get help if something goes wrong.
---


Uninstalling cPGuard is a quick and straightforward process. Whether you are decommissioning a server, migrating to a new one, or simply switching security solutions, this guide walks you through the complete uninstallation process.

---

## Before You Uninstall — License Considerations

If your intention is to **move cPGuard to a new server** rather than permanently remove it, you should handle the license before uninstalling. Once cPGuard is uninstalled, the license is released from the server and the server is removed from the App Portal.

:::tip
If you plan to reuse your cPGuard license on a different server or after an IP change, follow the [Migrate License to a New Server or IP Address](../../shared/license/migrate-or-reissue.md) guide before proceeding with uninstallation.
:::

---

## Uninstall cPGuard

Run the following single command on your server as **root** via SSH to download and execute the cPGuard uninstaller:

```bash
cd /usr/local/src && rm -f cpguard_uninstall.sh && curl -o cpguard_uninstall.sh -L https://downloads.opsshield.com/cpguard/cpguard_uninstall.sh && bash cpguard_uninstall.sh
```

**What the uninstaller does:**

1. Downloads the latest cPGuard uninstaller script
2. Removes all cPGuard packages and installed files from the server
3. Removes the cPGuard agent service
4. Detaches the server from your cPGuard App Portal account

That's it — the uninstallation is complete.

:::note
The uninstall script **does not delete your website files, databases, or any hosted content**. It only removes cPGuard-specific packages, services, configurations, and quarantine data.
:::

---


## After Uninstalling

Once cPGuard is uninstalled:

- The server will no longer appear in the **cPGuard App Portal**
- WAF protection, malware scanning, and IP reputation checks will stop
- If you plan to reinstall cPGuard on this server in the future, simply run the [installation command](installation.md) again with your license key

:::warning
After uninstalling, your server will have **no active web application firewall or malware scanner** from cPGuard. Ensure you have an alternative security solution in place if the server remains in production.
:::

---

## Need Help?

If you encounter any issues during uninstallation or the script fails to complete, the OPSSHIELD support team is available to help. You can raise a support ticket directly from the client portal:

[Raise a Support Ticket](https://manage.opsshield.com/index.php/client/plugin/support_manager/client_tickets/departments/)

You can also [grant the support team SSH access](../../shared/support.md) to your server so they can assist directly.

---
