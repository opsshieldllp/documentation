---
title: cPGuard X Overview
sidebar_position: 1
---
# cPGuard X Overview

cPGuard X is a modern hosting platform with security at its core. It brings essential tools for hosting and website management into a single, easy-to-use interface while integrating the powerful security capabilities of cPGuard.

Whether you are managing websites, deploying applications, optimizing performance, protecting your server, or managing backups, cPGuard X provides the tools you need in one platform.

---

## More Than a Hosting Control Panel

Managing a website today involves much more than simply creating a domain and uploading files.

You may need to manage different PHP versions, deploy applications, configure databases, optimize website performance, maintain backups, handle web server configuration, and ensure security.

cPGuard X brings these capabilities together, making it easier to manage your hosting environment from a single interface.

---

## Run the Applications You Use

Modern websites are not limited to traditional PHP applications. cPGuard X provides application management capabilities for popular platforms and frameworks.

### WordPress

WordPress websites can be installed and managed directly through cPGuard X.

Administrators can manage common WordPress tasks without needing to handle every operation manually from the command line.

### Laravel

cPGuard X provides dedicated Laravel application support, making it easier to deploy Laravel-based websites and applications.

Laravel can be selected when creating a website or installed on an existing website through the application management interface.

This gives developers a convenient way to get their Laravel applications running within their hosting environment.

[Learn more about Laravel Applications](./Applications/laravel.md)

### Node.js

cPGuard X also supports Node.js applications, allowing you to run modern JavaScript applications on your server.

You can select the Node.js runtime required by your application and manage the application process from the control panel.

Node.js applications can run on their own local port and be exposed through a website using reverse proxy configuration.

[Learn more about Node.js Applications](./Applications/node.md)

---

## Built for Performance

A well-managed website is not only about availability, performance matters too.

cPGuard X includes performance features that allow administrators to optimize how websites are served.

### Nginx Server-Side Caching

The **Nginx Proxy Cache** feature in cPGuard X improves website performance and reduces backend load by storing generated responses and serving them directly from memory or disk for subsequent requests.

Instead of every visitor triggering PHP execution and database queries, frequently accessed content can be delivered directly from cache.

This can help reduce backend processing and improve response times, particularly for websites receiving repeated requests for the same content.

Administrators can also control cache behavior by configuring exclusions and clearing cached content when necessary.

[Learn more about Nginx Server-Side Caching](./cache/nginx-proxy-cache.md)

---

## Backup and Recovery

Your website data is one of your most important assets. A hosting platform should therefore provide more than just tools for managing websites. It should also help protect your data.

cPGuard X includes an integrated backup management system that makes it easier to create, manage, and restore backups.

### Backup Management

Configure backup schedules and retention according to your requirements.

You can manage backup settings directly from cPGuard X and view the available backup copies for your websites.

[Learn more about Backup Management](./backups/overview.md)

### Remote Backup Destinations

Keeping backups on the same server as your websites can leave your data exposed if the server itself experiences a serious failure.

cPGuard X allows backups to be stored on remote destinations, providing an additional copy outside the primary server.

Remote destinations can be configured for supported storage services such as:

- SFTP
- Amazon S3
- Google Drive
- OneDrive

The remote destination can be validated from cPGuard X before it is used for backup storage.

[Learn more about Remote Backup Destinations](./backups/remote-destinations.md)

---

## Security Built Into the Platform

cPGuard X integrates the cPGuard Security Suite directly into the hosting environment, giving administrators access to multiple layers of protection from the same interface.

All cPGuard security features are available within cPGuard X, such as Firewall protection, Web Application Firewall, Malware protection, Intrusion Defense, etc.

This combination allows administrators to manage their websites and apply security controls without having to build a separate security stack around their hosting panel.

---

## Start Exploring cPGuard X

cPGuard X is built to give developers and server administrators a simpler way to manage modern hosting infrastructure while keeping performance, backups, and security close at hand.

Explore the platform and see how its website management, application support, performance tools, backup capabilities, and integrated security can fit into your hosting environment.

Ready to explore cPGuard X?

[Visit cPGuard X](https://cpguardx.com/)
