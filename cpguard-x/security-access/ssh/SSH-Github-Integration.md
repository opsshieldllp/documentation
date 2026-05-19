---
title: SSH Authentication & GitHub Integration
sidebar_position: 6
description: Configure SSH key authentication and integrate your server with GitHub repositories using cPGuard X.
---

# SSH Authentication & GitHub Integration

To manage your server securely, it is recommended to use **SSH key-based authentication** instead of passwords. SSH keys improve security and allow passwordless login to the server. 

The cPGuard X control panel provides an interface to add SSH keys and configure outgoing SSH connections for services such as **GitHub repositories**.

---

## Accessing SSH Settings

Follow these steps to access SSH configuration in the control panel:

1. Log in to the **cPGuard X control panel**.
2. Navigate to **Websites**.
![cPGuard X websites](../../../assets/img/ssh/image1-19.png)

3. Select the website you want to manage.
![cPGuard X websites](../../../assets/img/ssh/image2-8.png)

4. Open the **Advanced** dropdown menu.
![cPGuard X websites advanced](../../../assets/img/ssh/image3-7.png)

5. Click **SSH** to open the SSH configuration page for the website.
![cPGuard X ssh](../../../assets/img/ssh/ssh.png)

6. On the SSH page, click **+ Add Public Key** to add a new key.
![cPGuard X ssh section](../../../assets/img/ssh/ssh3.png)

7. Paste your **public SSH key** in the provided field.
![cPGuard X ssh section](../../../assets/img/ssh/ssh5.png)

8. Click **Create**.

Once the key is added, you can connect to the server via SSH using the corresponding **private key** from your local machine.

---

## Connecting to External Services (GitHub)

In some situations, the server must connect to external systems such as:

- GitHub
- Git repositories
- Deployment systems

To allow this, the server needs an **SSH key pair**.

### Steps to Configure

1. Go to the **SSH** section in the control panel.
2. Click **+ Add SSH Key**.
3. Either:
   - Import an existing SSH key, or
   ![cPGuard X ssh section](../../../assets/img/ssh/ssh7.png)

   - Generate a new SSH key pair.
   ![cPGuard X ssh section](../../../assets/img/ssh/ssh8.png)


This key pair can then be used for **outgoing SSH connections from the server to external platforms like GitHub**.

---

## Example: Using SSH with GitHub

After generating a key pair:

1. Copy the **public key**.
2. Add it to your GitHub account under:

```
GitHub → Settings → SSH and GPG Keys → New SSH Key
```
![cPGuard X ssh section](../../../assets/img/ssh/ssh-and-gpg-key.png)

Once added, the server will be able to authenticate with GitHub repositories securely.

---

## Benefits of SSH Authentication

Using SSH keys provides several advantages:

- Eliminates password-based login
- Stronger authentication mechanism
- Secure Git repository access
- Suitable for automated deployments

---

## Summary

With cPGuard X you can:

- Add **SSH public keys for server login**
- Generate **SSH keys for outbound connections**
- Integrate servers with **GitHub repositories**
- Improve server security using **key-based authentication**

Using SSH keys is the recommended method for secure server access and automated repository integration.