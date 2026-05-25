---
title: Outdated CMS notifications
tags: [cpguard, cms, wordpress, notifications, email, branding, opsshield, faq, security]
description: Learn how to configure cPGuard to automatically send email notifications to hosting users when their CMS installations are outdated — including scheduling options and how to brand the notification emails.
---

# How to Notify Users About Outdated CMS Installations in cPGuard

Outdated CMS installations whether WordPress, Joomla, Drupal, or others are one of the most common entry points for attacks on shared hosting servers. Keeping users informed about outdated software on their own accounts shifts responsibility appropriately and reduces the risk of unpatched vulnerabilities being exploited across the server.

cPGuard makes this easy with a built-in **automated CMS version notification** feature that emails end users directly when outdated installations are detected under their accounts.

{/* comment */}

---

## What the Feature Does

When enabled, cPGuard scans for CMS installations under each user's account, checks the installed versions against current release information, and sends an **automated email alert** to the account owner when outdated versions are found. The email prompts the user to update their CMS to reduce security risk.

This is a proactive security measure that empowers hosting users to take action on their own sites without requiring server administrators to intervene manually.

---

## How to Enable CMS Outdated Version Notifications

To configure the notification schedule:

1. Log in to the **cPGuard App Portal** and open your server.
2. Navigate to **Settings → Notifications**.
3. Under the **User Notifications** section, find the **"Outdated CMS"** option.
4. Select your preferred notification frequency from the available options.

---

## Notification Schedule Options

| Option | Behaviour |
|---|---|
| **Never** | Completely disables CMS outdated version notifications |
| **Daily** | Sends alerts every day after fetching version info from installed CMS |
| **Weekly** | Sends alerts every **Monday** after fetching CMS version info |
| **Monthly** | Sends alerts on the **1st day of each month** after fetching CMS version info |

:::tip
**Weekly** notifications are a good default for most shared hosting environments. Frequent enough to keep users informed without generating alert fatigue from daily emails. Use **Daily** for servers where you want the fastest possible response to outdated software.
:::

---

## Why This Matters for Hosting Providers

Outdated CMS versions are a leading cause of server-wide compromises on shared hosting. A single user running an unpatched WordPress installation can become an entry point for malware that spreads across the entire server.

By enabling automated CMS notifications, hosting providers can:

- **Reduce server-wide infection risk** by prompting users to update before vulnerabilities are exploited
- **Shift responsibility** to users for the security of their own applications
- **Reduce support overhead** by preventing avoidable malware infections upstream
- **Demonstrate proactive security** as a value-add for their hosting service

---

## Branding and Customising the Email Template

By default, the CMS outdated version notification emails use the standard cPGuard template. Hosting providers and administrators can **fully customise and brand** these emails to match their own brand replacing the logo, brand name, colours, and any other styling elements.


For full details on all customisable email templates in cPGuard including notification emails for malware detections and other user-facing alerts. See the [Customising/Branding Email Templates guide](https://opsshield.com/help/cpguard/customising-branding-user-email-templates/).


---
