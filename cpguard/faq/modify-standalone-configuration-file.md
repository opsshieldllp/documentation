---
title: How to modify standalone configuration file cpguard.ini
---

You can modify the standalone configuration file if you wish to change any of the value sets to match with the updated server configuration. You have 2 options to modify the values in the standalone configuration file.

---

### Option 1: By Directly Editing the Configuration File

1. Open file `/opt/cpguard/cpguard.ini` in your favorite text editor and change the value of the configuration you wish to modify
2. Run the following command to update the configuration

```bash
cpgcli standalone-conf --update
```

---

### Option 2: Using the Reconfigure Wizard

Run the following command from SSH as the root user, which will run the entire standalone configuration again and you can modify the whole configuration values

```bash
cpgcli standalone-conf --wizard
```