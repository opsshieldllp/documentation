---
title: Run a hook script after file detection
sidebar_position: 2
---

cPGuard allows you to run a script after detecting a bad file, in which you can perform necessary actions on the affected file.

## Arguments Passed to the Hook Script

The following details are passed as arguments to the script:

1. Original path of the infected file
2. Path to the quarantined file, or `not-quarantined` if the file is not quarantined
3. Virus description
4. Category of the detected file
5. Username affected
6. User email

## Script Location

Place your hook script at:

```
/opt/cpguard/app/scripts/virus_hook.php
```

A sample hook script is available at:

```
/opt/cpguard/app/scripts/virus_hook_sample.php
```

## Sample Hook Script

The sample script contains code to push notifications to a Slack channel or send a notification to the end user.

```php
#!/opt/cpguard/cpg-php-fpm/bin/php
<?php
## Remember to make this file executable
$original_path = $argv[1];      // Original path of infected file
$quarantine_path = $argv[2];    // Path to Quarantined file or "not-quarantined" if file was not quarantined
$virus_definition = $argv[3];   // Virus Description
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
$headers = "MIME-Version: 1.0" . "\r\n";
$headers .= "Content-type:text/html;charset=UTF-8" . "\r\n";
$headers .= 'From: <webmaster@example.com>' . "\r\n";
$headers .= 'Cc: myboss@example.com' . "\r\n";
mail($user_email, $subject, $message, $headers);
```

:::note
Please contact our support if you need any additional details or want additional features added to the hook script.
:::