---
title: Change Document Root

---

# Change Website Document Root in cPGuard X

## Overview

The **Document Root** is the directory from which your web server serves website files. By default, it is usually set to a directory such as:

```
/home/username/public_html
```

There may be situations where you need to change the document root to point to a different directory.

## Common Use Cases

You may want to change the document root when:

- Deploying a new version of your website from a different directory.
- Hosting applications that use a **public** or **web** folder as the web root (such as Laravel or Symfony).
- Separating application files from publicly accessible files for better security.
- Moving your website to a new location without changing the domain.
- Testing a new website version before replacing the existing one.

---

# How to Change the Document Root

Follow these steps to update the document root for a website.

## Step 1: Open the Websites Section

From the **cPGuard X Control Panel**, navigate to **Websites** using the sidebar.

![cPGuard X websites](../../assets/img/websites/image1-11.png)


---

## Step 2: Select the Website

Locate the website whose document root you want to modify and click on it.

![cPGuard X websites](../../assets/img/websites/addon.png)

---

## Step 3: Open the Domains Tab

Inside the website settings, click the **Domains** tab.

![cPGuard X websites](../../assets/img/websites/addon1.png)



---

## Step 4: Edit the Document Root

In the domain list:

1. Locate the domain.
2. Click the **Edit** (✏️) icon next to the **Document Root** field.
3. Enter the new document root path.

Example:

```
/home/user/public_html
```

or

```
/home/user/myapp/public
```

![websites document root change](../../assets/img/websites/change-docroot1.png)


---

## Step 5: Update the Changes

After entering the new document root, click **Update** to save the changes.

The web server configuration will be updated automatically.

![websites document root change](../../assets/img/websites/change-docroot2.png)

---

# Verify the Change

After updating the document root:

1. Visit your website in a browser.
2. Ensure the correct content is being served.
3. Verify that the specified directory exists and contains the website files.
4. Confirm that the web server user has appropriate permissions to access the directory.

---

:::note

- Ensure the new document root path exists before updating.
- The directory should contain the website's entry file (such as `index.php` or `index.html`).
- Incorrect document root paths may result in **404 Not Found**, **403 Forbidden**, or **500 Internal Server Error** responses.
- If your application uses a framework (for example, Laravel), ensure the document root points to the framework's **public** directory rather than the project root.
:::