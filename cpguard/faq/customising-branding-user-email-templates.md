---
slug: cpguard-customising-branding-email-templates
title: How to Customise and Brand cPGuard User Email Templates
authors: [your-name]
tags: [cpguard, email-templates, branding, twig, customisation, opsshield, hosting-provider, faq]
date: 2024-10-03
description: A complete guide to customising and branding the cPGuard user-facing email notifications — covering the Twig templating engine, all four email templates, their variables, and how to safely create custom versions without overwriting originals.
---

# How to Customise and Brand cPGuard User Email Templates

When cPGuard detects a threat or takes action on a hosting account, it automatically sends notification emails to the end user. By default, these emails carry the cPGuard branding. But as a **hosting provider**, your customers likely don't know what cPGuard is. They only know your brand.

cPGuard's email template system lets you **fully customise every user-facing notification email** replacing logos, brand names, colours, and content. So every automated email your customers receive looks like it came directly from your hosting company.

{/* comment */}

---

## How to Create a Custom Template

All email templates are stored on your server at:

```
/opt/cpguard/app/resources/email_templates/
```

The customisation process follows a consistent pattern for every template:

**Step 1 : Copy the original file**

Never edit the original template directly. Instead, create a copy with `_custom` appended to the filename:

```bash
# Example for the infected files template
cp /opt/cpguard/app/resources/email_templates/user_infected_files.html \
   /opt/cpguard/app/resources/email_templates/user_infected_files_custom.html
```

**Step 2 : Edit the custom copy**

Open the copied file in any text editor and make your changes:

```bash
nano /opt/cpguard/app/resources/email_templates/user_infected_files_custom.html
```

Modify the logo, brand name, colours, content, and any other elements to match your brand identity.

**Step 3 : Save and deploy**

Once saved, cPGuard automatically detects the `_custom` version and uses it in place of the default template. No restart or additional configuration is required.

:::note
Custom templates survive cPGuard software updates without being overwritten, since the update process only replaces the original filenames — not the `_custom` variants.
:::

:::tip
For the best results, start by changing the logo URL, the company name, support email, and primary brand colour. These four changes alone transform the email from a generic cPGuard notification into a branded communication from your company.
:::

---

## Twig Templating Engine

cPGuard's email templates use **[Twig](https://twig.symfony.com/doc/3.x/templates.html)**, a widely-used, secure PHP templating engine that powers dynamic content in the emails. Understanding the basic Twig syntax helps you customise templates confidently without breaking the logic.

### Variables

Variables are rendered using double curly brace syntax:

```twig
{{ user }}       {# Renders the username #}
{{ domain }}     {# Renders the primary domain #}
{{ hostname }}   {# Renders the server hostname #}
```

### Conditionals

Use `{% if %}` blocks for conditional content:

```twig
{% if logs|length > 0 %}
  {/* comment */}
{% else %}
  {/* comment */}
{% endif %}
```

### Loops

Use `{% for %}` to iterate over arrays like the logs list:

```twig
{% for log in logs %}
  {/* comment */}
{% endfor %}
```

### Available Filters

| Filter | Example | Description |
|---|---|---|
| `e` | `{{ log.path\|e }}` | Escape special HTML characters |
| `date` | `{{ log_time\|date('F j, Y, g:i a') }}` | Format a timestamp |
| `first` | `{% set log = logs\|first %}` | Get the first item of an array |
| `lower` | `{{ log.reason\|lower }}` | Convert text to lowercase |
| `length` | `{{ logs\|length }}` | Get the count of items in an array |

:::warning
Keep all Twig **logic structures intact** loops (`{% for %}`), conditionals (`{% if %}`), and variable references. Removing or breaking these will cause the email to render incorrectly or fail to send. Only modify the surrounding HTML, CSS, and static text content.

Also note that **email clients have limited HTML and CSS support** compared to web browsers. Avoid using modern CSS features like flexbox, grid, or CSS variables. Stick to inline styles and table-based layouts for consistent rendering across Gmail, Outlook, Apple Mail, and other clients.
:::

---

## Available Email Templates

### 1. Infected Files Notification

**Filename:** `user_infected_files.html`

Sent when the malware scanner detects infected files on a user's hosting account. Informs the user of the specific files flagged and the action taken.

**Template Variables:**

| Variable | Description |
|---|---|
| `user` | Username of the hosting account |
| `domain` | Primary domain of the account |
| `logs` | Array of detected infected file log entries |
| `hostname` | Server hostname |
| `__year` | Current year (used in copyright footer) |

**`logs` Array Items** (accessed via `{% for log in logs %}`):

| Variable | Description |
|---|---|
| `log.path` | Full path of the infected file |
| `log.definition` | Virus or malware signature name |
| `log.reason` | Type or category of the detection |
| `log.action` | Action taken (e.g., quarantined, deleted) |
| `log_time` | Timestamp of when the infection was detected |

---

### 2. Outdated CMS and Vulnerability Report

**Filename:** `user_cms_threats.html`

Sent when cPGuard detects outdated CMS versions or known vulnerabilities on a user's account. Encourages the user to update their CMS or address security issues.

**Template Variables:**

| Variable | Description |
|---|---|
| `title` | Subject/title of the email |
| `user` | Username of the account |
| `domain` | Primary domain of the account |
| `critical_count` | Number of critical CMS issues detected |
| `outdated_count` | Number of outdated CMS installations found |
| `threats_count` | Total number of CMS threats found |
| `cms_list` | Array of CMS installations and their status |
| `threats` | Array of identified threat details |
| `hostname` | Server hostname |
| `__year` | Current year (used in copyright footer) |

**`cms_list` Array Items** (accessed via `{% for cms in cms_list %}`):

| Variable | Description |
|---|---|
| `cms.domain` | Domain of the CMS installation |
| `cms.path` | Server path of the CMS |
| `cms.version` | Currently installed CMS version |
| `cms.version_status` | Version status (e.g., `latest`, `outdated`) |
| `cms.threat_rating` | Highest CVE rating (e.g., `critical`, `high`, `medium`) |
| `cms.threat_score` | Highest CVE score |

**`threats` Array Items** (accessed via `{% for threat in threats %}`):

| Variable | Description |
|---|---|
| `threat.path` | File path of the detected threat |
| `threat.version` | Reason for identifying the file as a threat |
| `threat.version_status` | Severity level (e.g., `Low`, `Moderate`, `Severe`, `Critical`) |

---

### 3. User Account Suspension Notification

**Filename:** `user_account_suspension.html`

Sent when suspicious activity triggers an automatic account suspension. Informs the user that their account has been suspended for security reasons and prompts them to take action.

**Template Variables:**

| Variable | Description |
|---|---|
| `title` | Subject/title of the email |
| `domain` | Primary domain of the suspended account |
| `suspended_user` | Username of the suspended account (if applicable) |
| `suspended_domain` | Domain that has been suspended (if no user is specified) |
| `reason` | Specific reason for the suspension |
| `count` | Number of infected files detected (conditional) |
| `interval` | Time range in hours during which infections were detected (conditional) |
| `hostname` | Server hostname |
| `__year` | Current year (used in copyright footer) |

:::note
`suspended_user` and `suspended_domain` are used conditionally — the template renders user-based or domain-based suspension messages depending on which variable is populated. Keep both conditional blocks in your custom template to handle both cases correctly.
:::

---

### 4. User Account Login Disabled Notification *(cPanel Only)*

**Filename:** `user_account_disable.html`

Sent when suspicious activity causes cPGuard to automatically disable a user's account login. Informs the user that their login has been disabled as a security precaution.

**Template Variables:**

| Variable | Description |
|---|---|
| `title` | Subject/title of the email |
| `domain` | Primary domain of the affected account |
| `user` | Username of the account |
| `hostname` | Server hostname |
| `__year` | Current year (used in copyright footer) |

:::note
This email template applies to **cPanel only**. DirectAdmin, Plesk, and Standalone configurations do not use this template.
:::

---


# Email Template Testing


The `cpgcli notification --test` command allows administrators to send test/dummy emails while customising email templates, without triggering a real scan or security event.

---

## Syntax

```bash
cpgcli notification --test <email_type> --template <template_file> --to <recipient> [options]
```

---

## Options

| Option | Description |
|---|---|
| `--template` | Specify the email template file to use |
| `--to` | Specify the recipient email address (or domain for some types) |
| `--count` | Number of infections to simulate *(used with `scanner_detection_user`)* |
| `--type` | Suspension type: `virus` or `domain` *(used with `account_suspension_user`)* |

---

## Examples

### Scanner Detection Email
```bash
# Test with 1 infection
cpgcli notification --test scanner_detection_user --template test_template.html --to name@example.com --count 1

# Test with 5 infections
cpgcli notification --test scanner_detection_user --template test_template.html --to name@example.com --count 5
```

### CMS Threats Email
```bash
cpgcli notification --test cms_threats_user --template test_template.html --to name@example.com
```

### User Disabled Email *(cPanel only)*
```bash
cpgcli notification --test user_disabled --template test_template.html --to name@example.com
```

### Account Suspension Email
```bash
# Suspend on virus detection
cpgcli notification --test account_suspension_user --template test_template.html --to example.com --type virus

# Suspend on domain blacklist
cpgcli notification --test account_suspension_user --template test_template.html --to name@example.com --type domain
```

---

## ⚠️ Important Notes

> - The `--template` option expects the template file to be located inside:
>   ```
>   /opt/cpguard/app/resources/email_templates/
>   ```
> - After testing, the template file should be **renamed or overwritten** to replace the actual template file in use.