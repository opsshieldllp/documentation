---
title: File Actions
sidebar_position: 6
---



Using below options you can change the actions perfomed by the scanner when an infected file is detected


:::note
Allowed options are **email**, **disable**, or **quarantine**.
:::

| Command | Description |
|--------|-------------|
| `cpgcli file-action --virus` | Set virus file action |
| `cpgcli file-action --suspicious` | Set suspicious file action |
| `cpgcli file-action --binary` | Set binary file action |

> **Note:** The default action is **Email Only**. The scanner will not take any further action unless explicitly changed.

## Configuration via UI

![Firewall](../../assets/img/cpguard/firewall/vs3.png)

1. Navigate to **Settings → Virus Scanner**
2. Under **What to do with**, select the action for each file type:
   - **Virus Files**
   - **Suspicious Files**
   - **Binary Files**
3. Choose one of the following options:

| Option | Behaviour |
|---|---|
| **Email Only** | Sends an alert. Use this to handle files manually. |
| **Quarantine** *(Recommended)* | Moves the file to a secure quarantine directory. |
| **Disable File** | Sets file permissions to `000`, rendering it inaccessible. |

---