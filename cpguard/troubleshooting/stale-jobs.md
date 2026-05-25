---
title: "Too Many cpguard-job-logs::fetchlogs Processes Running – Cause and Fix"
tags: [cpguard, ipdb, troubleshooting, performance, disk-io, fetchlogs, opsshield, faq]
description: Learn why multiple cpguard-job-logs::fetchlogs processes pile up on busy servers, what causes the disk IO spike, and how to fix it by disabling IPDB logging without disabling IPDB protection.
---

# Too Many cpguard-job-logs::fetchlogs Processes Running – Cause and Fix

If you've spotted a large number of `cpguard-job-logs::fetchlogs` processes in your process list and noticed elevated server load or disk IO, this article explains exactly what's happening and how to resolve it quickly — without compromising your server's protection.

{/* comment */}

---

## What Is the `cpguard-job-logs::fetchlogs` Process?

cPGuard has multiple internal components that generate logs. One of these is **IPDB** (IP Database) — cPGuard's IP-based firewall that silently blocks requests from known malicious IP addresses and entire country IP ranges.

When **IPDB logging is enabled**, cPGuard uses the **syslog facility of iptables/ipset** to write every blocked IP event to the system log file in a structured format. These logs are valuable for monitoring:

- The **rate of incoming attacks** being blocked
- Which **IP addresses** are being blocked most frequently
- Which **countries or IP blocks** are the source of most attack traffic

To collect these entries from the system log and store and display them in the cPGuard App Portal, cPGuard runs an internal background process called:

```
cpguard-job-logs::fetchlogs
```

This process collects several types of logs, but on high-traffic or frequently-attacked servers, **IPDB log entries dominate** — and this is where the problem arises.

---

## Why Do So Many Processes Queue Up?

On busy servers or servers that receive a large volume of attack blocks through IPDB, the system log can accumulate **thousands — or even tens of thousands — of IPDB block entries** in a short period of time.

Each `fetchlogs` run must read these entries from the system log file and write them to cPGuard's internal storage. When the **disk IO cannot keep pace** with the volume of reads and writes required:

1. Individual `fetchlogs` processes take longer to complete than expected
2. New `fetchlogs` processes are triggered before previous ones finish
3. Processes begin to queue up — with each new run stacking behind unfinished ones
4. This queue causes **elevated server load** and a **disk IO spike** that can noticeably impact overall server performance

---

## The Fix — Two Steps

The solution is straightforward: **disable IPDB logging** to stop writing IPDB block events to the system log, then kill the stuck processes that have already queued up.

:::warning
**Do not disable IPDB itself** — only disable IPDB logging. IPDB is an essential cPGuard protection module that silently blocks known malicious IPs and IP ranges. Disabling it entirely would leave your server unprotected against these threats. Disabling logging simply means attacks continue to be blocked silently, without being written to disk.
:::

---

### Step 1 : Disable IPDB Logging

**Via CLI:**

```bash
cpgcli fw --ipdb-log disable
```

**Via App Portal:**

1. Log in to the **cPGuard App Portal** and open your server.
2. Navigate to **Settings → Security Tools**.
3. Switch off **"Enable IPDB Logging"**.

Once IPDB logging is disabled, no new block events will be written to the system log — stopping the cycle of new `fetchlogs` processes being triggered.

---

### Step 2 : Kill All Queued `fetchlogs` Processes

With logging disabled, you can now safely kill all the `fetchlogs` processes that are currently stuck or queued:

```bash
kill -9 `ps aux | grep 'cpguard-job-logs::fetchlogs' | awk -F" " {'print $2'}`
```

This terminates all running `fetchlogs` processes immediately. Since IPDB logging is now disabled, no new processes will pile up again.

---

## Verifying the Fix

After running both commands, verify that the processes are no longer running:

```bash
ps aux | grep 'cpguard-job-logs::fetchlogs'
```

You should see only the `grep` process itself in the output — no active `fetchlogs` processes. Server load should return to normal within a few minutes as any residual disk IO settles.

---

## Should You Re-enable IPDB Logging Later?

IPDB logging is useful for visibility into attack traffic. But it is **optional** from a protection standpoint. cPGuard's IPDB module will continue to block attacks silently regardless of whether logging is enabled.

Consider re-enabling it only if:

- Your server's disk IO has improved significantly (e.g., after upgrading storage)
- Attack traffic has reduced to a manageable level
- You specifically need the attack log data for analysis or reporting

On consistently high-traffic servers, it is generally safer to leave IPDB logging disabled and rely on the App Portal's IPDB statistics for an overview of blocked traffic.

:::tip
If you need occasional visibility into IPDB block activity without the ongoing disk IO burden, you can **temporarily enable IPDB logging**, collect the data you need, and then disable it again.
:::

---

