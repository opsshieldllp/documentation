---
title: Suspend Hook
sidebar_position: 3
---

# Suspend Hook

Use `suspend_hook.php` to run custom automation when cPGuard triggers a user or domain suspension event.

## Script Location

```text
/opt/cpguard/app/scripts/suspend_hook.php
```

## Input Payload

The hook receives one JSON argument (`$argv[1]`) with:

- `emails` (array): Primary and secondary notification emails
- `user` (string): User to suspend
- `reason` (string): Reason for suspension

## Sample `suspend_hook.php`

```php
#!/opt/cpguard/cpg-php-fpm/bin/php
<?php

## DO NOT CUSTOMISE THIS FILE
## This file may be updated during software update
## Please make a copy of the file (as suspend_hook.php) and customize for using it

sleep(2);
$input = json_decode($argv[1], true);
/*
    $input['emails']    - (array) Primary and secondary notification emails
    $input['user']      - (string) The user to be suspended
    $input['reason']    - (string) Reason for suspension
*/

if (!isUserSuspended($input['user'])) {

    // Suspend the user
    suspendUser($input['user'], $input['reason']);

    // Send a notification in Slack
    slack_notification($input);

    // Send an email
    send_email_notification($input);
}

function isUserSuspended($user)
{

    // code to check if user is already suspended

    // Your code here...

}


function suspendUser($user, $reason)
{

    // code to suspend user

    // Your code here...

}

/* -------------------------------------------------------------------------
 *      SLACK WEBHOOKS
 *      REFER https://api.slack.com/messaging/webhooks
 * ---------------------------------------------------------------------- */

function slack_notification($input)
{

    $server = gethostname();

    // Update the webhook URL below
    $webhook_url = "https://hooks.slack.com/services/xxxxxxxxxxxxxxx";

    $data = array(
        "text" => $input['user'] . " account suspended! on $server",
        "blocks" => array(
            array(
                "type" => "section",
                "text" => array(
                    "type" => "mrkdwn",
                    "text" => "*" . $input['user'] . " account suspended! on $server*\nReason : " . $input['reason']
                )
            )
        )
    );

    $data_string = json_encode($data);
    $ch = curl_init($webhook_url);
    curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "POST");
    curl_setopt($ch, CURLOPT_POSTFIELDS, $data_string);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
    curl_setopt(
        $ch,
        CURLOPT_HTTPHEADER,
        array(
            'Content-Type: application/json',
            'Content-Length: ' . strlen($data_string)
        )
    );

    $result = curl_exec($ch);
}

/* -------------------------------------------------------------------------
 *      SENDING EMAILS TO END USERS
 * ---------------------------------------------------------------------- */

function send_email_notification($input)
{
    if (empty($input['emails'])) {
        echo "User email IDs are not available. Email not sent";
        return false;
    }
    $server = gethostname();
    $to_address = implode(',', $input['emails']);
    $subject = $input['user'] . " account suspended on $server";

    $message = "
    <html>
        <head>
            <title>" . $input['user'] . " account suspended on $server</title>
        </head>
        <body>
            <h2>" . $input['user'] . " account suspended on $server</h2>
            <p>Reason : " . $input['reason'] . "</p>
        </body>
    </html>
    ";

    // Always set content-type when sending HTML email
    $headers = "MIME-Version: 1.0" . "\r\n";
    $headers .= "Content-type:text/html;charset=UTF-8" . "\r\n";

    // More headers
    $headers .= "From: cpguard@$server" . "\r\n";
    //$headers .= 'Cc: myboss@example.com' . "\r\n";

    mail($to_address, $subject, $message, $headers);
}
```

## Make Script Executable

```bash
chmod +x /opt/cpguard/app/scripts/suspend_hook.php
```

## Best Practices

- Keep custom hooks idempotent to avoid duplicate actions.
- Validate payload values before calling external services.
- Add timeout and error handling around webhook calls.
