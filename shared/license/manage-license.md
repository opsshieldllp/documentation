---
sidebar_position: 2
sidebar_label: Apply/Manage License
title: Manage Your cPGuard License
---

# How to Manage Your cPGuard License

Managing your cPGuard license is a straightforward process handled entirely through the `cpgcli` command-line utility. Whether you're applying a new license key, checking its current status, troubleshooting an issue, or decommissioning a server, every license operation is covered by a single command.

---

## When You Might Need These Commands

License management commands are most commonly used when:

- **Activating cPGuard for the first time** on a new server
- **Upgrading from a trial** to a paid license
- **Troubleshooting** license verification or activation errors
- **Moving a license** to a different server after a migration or IP change
- **Decommissioning** a server and releasing the license

:::note
If your server's **IP address changes** or you want to use an existing license on a different server, you must first **reissue the license** from the OPSSHIELD client area before applying it again. See the [Reissue License guide](migrate-or-reissue) for details.
:::

---

## Adding a License

To apply a new license key to your server, use the following command, replacing `LICENSE-KEY` with your actual key:

```bash
cpgcli license --key LICENSE-KEY
```

**What happens when you run this:**

1. The license key is sent to the OPSSHIELD billing and licensing server for verification.
2. Upon successful verification, the license is saved on the server.
3. The server is automatically attached to your OPSSHIELD user account and becomes visible in the App Portal.

:::tip
This same command is used whether you're activating a brand-new license or applying a freshly reissued license key to a server.
:::

---

## Checking License Status

To view the current status of the license applied on your server:

```bash
cpgcli license --status
```

This is useful for quickly confirming whether the license is active, suspended, or expired especially when diagnosing issues with cPGuard features not behaving as expected.

---

## Renewing License Data

If your license was previously suspended and has since been unsuspended, or if there was a temporary synchronisation issue between the server and the licensing system, use this command to pull the latest license data:

```bash
cpgcli license --renew
```

This forces a fresh license check against the OPSSHIELD licensing server and updates the local license state accordingly.

---

## Removing a License

To fully remove the license from a server:

```bash
cpgcli license --remove
```

:::warning
This command does more than remove the license key locally. It also **removes the server and all its data from the cPGuard App Portal**. Use this command only when you intend to fully decommission the server from cPGuard.
:::

---

## Command Summary

| Command | Purpose |
|---|---|
| `cpgcli license --key LICENSE-KEY` | Apply a new license key to the server |
| `cpgcli license --status` | Check the current license status |
| `cpgcli license --renew` | Refresh license data after unsuspension or sync issues |
| `cpgcli license --remove` | Remove license and detach server from App Portal |
