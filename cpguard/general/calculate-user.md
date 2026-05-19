---
title: How do we Calculate the number of users on a server
sidebar_position: 1
---

# cPGuard License Types and User Limits

cPGuard has 2 license types based on the number of accounts you have on your server. The Standard license is valid for up to 50 users on a server, whereas the Unlimited accounts license has no user limit.

Since we support multiple web hosting control panels and platforms, we count the number of normal users on the server regardless of the number of users active in the control panel. You can check the number of users on your server using the following command.

```bash
/usr/bin/awk -F '[/:]' '{if ($3 >= 1000 && $3 < 60000 && $3 != 65534) print $1}' '/etc/passwd' | grep -v 'cpguard\|centos\|clamav\|magicspam\|cldiaguser\|ubuntu\|cyberpanel\|docker\|ftpuser\|lscpd' 2>&1 |/usr/bin/wc -l
```

If you want to know the list of users included in the user count on your server, you can use the following command.

```bash
/usr/bin/awk -F '[/:]' '{if ($3 >= 1000 && $3 < 60000 && $3 != 65534) print $1}' '/etc/passwd' | grep -v 'cpguard\|centos\|clamav\|magicspam\|cldiaguser\|ubuntu\|cyberpanel\|docker\|ftpuser\|lscpd'
```

## What should you do if you get the "Allowed accounts limit exceeded" error

This error appears typically on servers with Standard License keys when it crosses the 50 users limit. To fix this error, you can do 2 actions

1. List the users you have on your server using the above command and remove the unwanted users to limit the user count to under 50
2. Upgrade to Unlimited license. There is no direct upgrade option available…you need to purchase a new Unlimited license, apply it on your server, and then cancel the old Standard license service.

If you need any assistance to fix this issue, please feel free to reach our support team.