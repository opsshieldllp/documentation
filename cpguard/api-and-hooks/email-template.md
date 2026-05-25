---
title: Customise Email Templates
sidebar_position: 5
---

# Customise Email Templates for Branding

You can replace default cPGuard branding in user-facing notifications with your own company identity.

## Template Directory

```text
/opt/cpguard/app/resources/email_templates/
```

## Safe Workflow

1. Copy the default template and append `_custom`.
2. Edit only the custom copy.
3. Keep Twig variables and logic blocks intact.
4. Test using `cpgcli notification --test`.

Example:

```bash
cp /opt/cpguard/app/resources/email_templates/user_infected_files.html \
   /opt/cpguard/app/resources/email_templates/user_infected_files_custom.html

nano /opt/cpguard/app/resources/email_templates/user_infected_files_custom.html
```

## Common Branding Changes

- Logo URL and footer company name
- Brand colors and support contact links
- Copy tone and call-to-action text

## Important

- Do not edit original template files directly.
- `_custom` files are not overwritten during software updates.
- Email clients support limited CSS; prefer table-based layout and inline styles.

## Test Emails

Use test notifications while iterating templates:

```bash
cpgcli notification --test scanner_detection_user --template test_template.html --to name@example.com --count 1
cpgcli notification --test cms_threats_user --template test_template.html --to name@example.com
cpgcli notification --test user_disabled --template test_template.html --to name@example.com
cpgcli notification --test account_suspension_user --template test_template.html --to name@example.com --type domain
```

For full variable-level template details, see the legacy deep-dive page under Troubleshooting.
