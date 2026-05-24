---
title: Virus Hook
sidebar_position: 2
---

# Virus Hook

Use `virus_hook.php` to run custom actions whenever cPGuard detects an infected, suspicious, binary, or symbolic-link file event.

## Script Location

```text
/opt/cpguard/app/scripts/virus_hook.php
```

## Sample Template Location

```text
/opt/cpguard/app/scripts/virus_hook_sample.php
```

## Arguments Passed to the Hook

The following values are passed as CLI arguments:

1. Original path of infected file
2. Quarantine path (`not-quarantined` if not quarantined)
3. Virus definition
4. Category (`Virus File`, `Suspicious File`, `Binary File`, `Symbolic Link`)
5. Username affected
6. User email

## Sample `virus_hook.php`

```php
#!/opt/cpguard/cpg-php-fpm/bin/php
<?php

## DO NOT CUSTOMISE THIS FILE
## This file may be updated during software update
## Please make a copy of the file (as virus_hook.php) and customize it

## Remember to make this file executable

$original_path = $argv[1];      // Original path of infected file
$quarantine_path = $argv[2];    // Path to quarantined file or "not-quarantined" if file was not quarantined
$virus_definition = $argv[3];   // Virus description
$category = $argv[4];           // Virus File | Suspicious File | Binary File | Symbolic Link
$username = $argv[5];           // Username affected
$user_email = $argv[6];         // User email

/* -------------------------------------------------------------------------
 *      SLACK WEBHOOKS
 *      REFER https://api.slack.com/messaging/webhooks
 * ---------------------------------------------------------------------- */

$server = gethostname();

$webhook_url = "https://hooks.slack.com/xxxxxxxxxxxx";

$data = array(
    "text" => "$category found on $server",
    "blocks" => array(
        array(
            "type" => "section",
            "text" => array(
                "type" => "mrkdwn",
                "text" => "*$category* found on $server"
            )
        ),
        array(
            "type" => "context",
            "elements" => array(array(
                    "type" => "mrkdwn",
                    "text" => "*Original path* : $original_path\n"
                    . "*Quarantine path* : $quarantine_path\n"
                    . "*Definition* : $virus_definition\n"
                    . "*User* : $username\n"
                )
            )
        ),
    )
);

$data_string = json_encode($data);
$ch = curl_init($webhook_url);
curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "POST");
curl_setopt($ch, CURLOPT_POSTFIELDS, $data_string);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_HTTPHEADER, array(
    'Content-Type: application/json',
    'Content-Length: ' . strlen($data_string))
);

$result = curl_exec($ch);

/* -------------------------------------------------------------------------
 *      SENDING EMAILS TO END USERS
 * ---------------------------------------------------------------------- */

$subject = "$category found";

$message = "
<html>
    <head>
        <title>$category found</title>
    </head>
    <body>
        <p>$category found</p>
        <table>
            <tr>
                <th style=\"text-align:left\">Type</th>
                <td>$category</td>
            </tr>
            <tr>
                <th style=\"text-align:left\">Original Path</th>
                <td>$original_path</td>
            </tr>
            <tr>
                <th style=\"text-align:left\">Quarantine path</th>
                <td>$quarantine_path</td>
            </tr>
            <tr>
                <th style=\"text-align:left\">Description</th>
                <td>$virus_definition</td>
            </tr>
            <tr>
                <th style=\"text-align:left\">User</th>
                <td>$username</td>
            </tr>
        </table>
    </body>
</html>
";

// Always set content-type when sending HTML email
$headers = "MIME-Version: 1.0" . "\r\n";
$headers .= "Content-type:text/html;charset=UTF-8" . "\r\n";

// More headers
$headers .= 'From: <webmaster@example.com>' . "\r\n";
$headers .= 'Cc: myboss@example.com' . "\r\n";

mail($user_email, $subject, $message, $headers);
```

## Make Script Executable

```bash
chmod +x /opt/cpguard/app/scripts/virus_hook.php
```

## Notes

- Keep runtime short to avoid delaying scanner workflows.
- If you call external APIs, add retries and timeout handling.
- Test with a safe sample file before using in production.
