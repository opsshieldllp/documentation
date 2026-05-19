---
title: Overriding Wordpress cron job
sidebar_position: 2
---

# What is wp-cron.php

WordPress comes with a default job scheduler which is actually a virtual cron job using a file called `wp-cron.php`. This is used to schedule tasks in order to automate things like publishing scheduled posts, checking for plugin or theme updates, sending email notifications, and more. So every time the website gets a hit and you have any scheduled task present on that website, the task is to basically ask "is it time to do anything yet?". This can be anything like

- Publishing of the scheduled posts
- Plugins or Theme updates check
- Sending notifications
- Taking backups
- WordPress automatic updates etc

## How it can be a problem for your server

Of course, you need to run the `wp-cron.php` scheduler to do some important jobs on your websites like updates checks and backup. But if you leave the default virtual cron job enabled, this check will happen with every hit. On a server with some low-traffic websites, this is perfectly fine. But when your site visits start to increase or you have so many WordPress websites with moderate hits such a scheduled tasks check for each website can be very inefficient and lead to resource usage problems for the server, this will in turn make your website load slower. We have had a situation where the CPU load on the server has doubled only due to `wp-cron.php` execution on all websites.

## How cPGuard manage this issue?

We have certain jobs which are collecting all information about the WordPress websites on your server and it keeps updating the information. So we use this information and run some logic to disable the cron job in each WordPress website, add a cron job under the user for each WordPress installation and configure each job to run multiple times intermittently a day and the run time for the users are synchronously distributed. We carefully calculate the run time for each WordPress wp-cron ( by assigning random hours and minutes ) and thus avoid the load due to running them all together. It is perfectly fine to run the job a few times a day for moderate-traffic websites and if any of the users want to change the scheduled time, they can update it from their control panel. This approach will help to reduce the server load due to `wp-cron.php` on a large scale and efficiently run them without any overhead to your website and server.

## Is it useful for everyone?

Not really, this is helpful for people only running too many WordPress websites and suffering from the `wp-cron.php` load. Do not enable this feature without knowing what you are doing. You may not find it useful if you have a low number of WordPress websites on your server or the websites get rare traffic. But if you have a handful of WordPress websites with moderate traffic, this will be a cool feature and it can save a lot of your server resources.

## How to enable this feature?

You can enable this simply by going to **cPGuard >> Settings >> Additional Settings >> Enable "Override WordPress wp-cron.php"** and select the **"Interval for running wp-cron.php"**.

## Is it possible to revert this anytime?

Of course, you can …cPGuard provides options to revert the configuration updates. To revert all the changes done by cPGuard to manage wp-cron jobs, go to **cPGuard >> Settings >> Additional Settings >> Disable "Override WordPress wp-cron.php"**. Please note that it will take up to a few minutes to complete the changes.

## Still need clarification?

Please feel free to contact our support team anytime and we are happy to answer your questions.