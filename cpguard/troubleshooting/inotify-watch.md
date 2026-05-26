---
title: What Is Inotify Watch and How Does It Affect Server Performance?

description: A complete explanation of what inotify and inotify watch is, how it powers cPGuard's real-time file scanning, whether increasing the watch limit affects server performance, and what OpenVZ/Virtuozzo users need to know before installing cPGuard.
---

# What Is Inotify Watch and How Does It Affect Server Performance?

If you've read through cPGuard's system requirements or seen a warning about inotify watch limits, you might be wondering what exactly inotify is, why cPGuard depends on it, and whether increasing its limit could cause any problems on your server. This guide answers all of those questions in plain language.

{/* comment */}

---

## What Is Inotify?

**Inotify** (short for *inode notify*) is a **Linux kernel subsystem** that allows the kernel to monitor the filesystem and notify applications when changes occur such as a file being created, modified, moved, or deleted. Instead of applications repeatedly polling the filesystem to check for changes (which is slow and resource-intensive), inotify pushes change events directly to the application via an API as they happen.

Inotify support was added to **glibc in 2006** and requires **Linux kernel 2.6.13 or later**. All modern Linux distributions that cPGuard supports include inotify by default.

---

## What Is an Inotify Watch?

An **inotify watch** is a single monitored directory  essentially a "subscription" that tells the kernel to report file events from that directory to the watching application.

Key characteristics of inotify watches:

- Applications can set watches on multiple directories simultaneously
- When a file event (create, modify, delete, move) occurs inside a watched directory, the kernel immediately notifies the watching application via the inotify API
- The Linux kernel enforces a **maximum limit** on the total number of inotify watches that can be active across the entire system at any one time

This limit is controlled by the kernel parameter:

```bash
fs.inotify.max_user_watches
```

You can check the current limit on your server with:

```bash
cat /proc/sys/fs/inotify/max_user_watches
```

---

## How cPGuard Uses Inotify

cPGuard's **real-time scanner engine** relies on inotify to monitor the directories in the watchlist for newly created or modified files. When a new or changed file appears in a watched directory, the inotify API immediately notifies the cPGuard scanner, which then scans that file for threats without any polling delay.

This is what enables cPGuard to catch malware the moment it is uploaded or written to disk, rather than waiting for the next scheduled scan.

For this to work correctly, the number of active inotify watches needs to be sufficient to cover all the directories in cPGuard's watchlist. On servers hosting many websites with deep directory structures, this can require a large number of watches which is why the default system limit often needs to be increased.

:::warning
If the inotify watch limit is hit, cPGuard's real-time scanning will be interrupted and you may see the following message in your system logs:

```
upper limit on inotify watches reached
```

If this occurs, the `max_user_watches` limit must be increased to restore normal scanner operation.
:::

---

## How to Check and Increase the Watch Limit

### Check the Current Limit

```bash
cat /proc/sys/fs/inotify/max_user_watches
```

### Temporarily Increase the Limit

```bash
sysctl -w fs.inotify.max_user_watches=1000000
```

### Permanently Increase the Limit

Add the following line to `/etc/sysctl.conf` (or a file in `/etc/sysctl.d/`):

```bash
echo "fs.inotify.max_user_watches=1000000" >> /etc/sysctl.conf
sysctl -p
```

:::tip
For servers hosting multiple accounts with many websites and deep directory structures, a value of **1,000,000** is a safe and commonly recommended starting point. The exact number you need depends on the total directory count across all watchlist paths on your server.
:::

---

## Does Increasing the Watch Limit Affect Server Performance?

This is a common concern — and the short answer is: **no, not meaningfully**.

On a 64-bit Linux system, each inotify watch consumes approximately **1 KB of kernel memory**. This means:

| Watch Count | Approximate Memory Usage |
|---|---|
| 100,000 watches | ~100 MB |
| 500,000 watches | ~500 MB |
| 1,000,000 watches | ~1 GB |

For a modern server with sufficient RAM, this is a manageable overhead. cPGuard is also designed to use inotify watches as efficiently as possible — it only sets watches on directories that are relevant to the web file watchlist, not the entire filesystem.

There is **no CPU overhead** from simply having watches set — the kernel only does work when a file event actually occurs within a watched directory.

:::note
The memory consumption above represents the theoretical maximum. In practice, cPGuard only sets watches on the directories it actively monitors — and not all 1,000,000 slots would typically be used simultaneously on a single server.
:::

---

## OpenVZ / Virtuozzo Users — What You Need to Know

OpenVZ and Virtuozzo containers have a fundamental restriction that affects inotify: **kernel parameters cannot be changed from within the container itself**. The `fs.inotify.max_user_watches` value is set at the host node level by the hosting provider — not by the VPS owner.

This means:

- You **cannot increase the watch limit** on an OpenVZ/Virtuozzo VPS yourself
- You must contact your **hosting provider** and ask them to increase the `max_user_watches` value for your container
- Some providers may be reluctant to do this even though increasing this limit is **completely safe**

:::warning
**Before purchasing or installing cPGuard on an OpenVZ or Virtuozzo VPS**, confirm with your hosting provider that they are willing and able to increase the inotify watch limit for your container, and that their kernel supports inotify.

If they use a custom kernel, verify that **inotify support is included**. Without this, cPGuard's real-time scanner will not function correctly.
:::

### Recommended Value to Request

When contacting your provider, request a `max_user_watches` value of at least:

```
1000000
```

This is sufficient for servers hosting multiple accounts. Your provider may set a lower value appropriate for a single-account VPS — that is acceptable as long as the number of watched directories stays within the limit.

---

