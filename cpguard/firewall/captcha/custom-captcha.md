---
title: Custom CAPTCHA Page
sidebar_position: 7
---


You can customize the firewall CAPTCHA page, including branding, by creating a custom template based on the default design.

### 1. Use the existing template as your starting point

```
/opt/cpguard/app/resources/captcha/design.html
```

### 2. Create a copy with the exact name

```
/opt/cpguard/app/resources/captcha/design_custom.html
```

### 3. Set the correct ownership

```bash
chown cpguard:cpguard /opt/cpguard/app/resources/captcha/design_custom.html
```

### 4. Modify the file as needed

Customize branding elements such as logo, text, colors, and styling.

### 5. Apply the changes

Disable and re-enable CAPTCHA after making changes:

```bash
cpgcli fw --captcha disable
cpgcli fw --captcha enable
```

## Important Notes

- The filename must be exactly `design_custom.html` for the custom template to be recognized.
- Avoid making major structural changes. The CAPTCHA widget is injected dynamically, and the template contains placeholders and required variables that must remain intact.
- Limit modifications to branding elements such as text, images, and styling to ensure compatibility.
- In rare cases, if there are changes to the CAPTCHA structure in future updates, you may need to review and adjust your custom template accordingly.