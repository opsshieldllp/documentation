---
title: Slack or Zulip Notifications
sidebar_position: 4
---

# Configure Slack or Zulip Notifications

You can send cPGuard incident notifications directly to Slack or Zulip from the App Portal.

## UI Path

1. Open **Settings**.
2. Click **Notifications**.
3. Select the **Slack** tab.
4. Enable the top-right toggle.
5. Paste your incoming webhook URL.
6. Save and test by triggering a known notification event.

## Supported Format

Use an incoming webhook URL, for example:

```text
https://hooks.slack.com/services/T00000000/B00000000/XXXXXXXXXXXXXXXXXXXXXXXX
```

## What Gets Sent

The notification channel receives events such as:

- License notifications
- Scanner status changes
- Virus file detections
- Binary file detections

## Notes

- Keep the webhook URL private.
- If no messages arrive, confirm outbound HTTPS access from the server.
- For custom payloads or routing logic, use `virus_hook.php` or `suspend_hook.php`.
