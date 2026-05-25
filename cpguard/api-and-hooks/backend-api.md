---
sidebar_label: Backend API
title: cPGuard Backend API
sidebar_position: 10
---

# cPGuard API Documentation

The cPGuard API provides a direct interface to the cPGuard service running on individual client servers. It enables developers and administrators to interact programmatically with the firewall and security engine—allowing tasks such as information gathering, automation, initiating scans, managing CMS updates, and retrieving reports to be performed seamlessly.

This documentation covers everything you need to work with the API, including authentication, request/response formats, and practical usage examples.

---

## Authentication

To use the cPGuard API, you must authenticate using an API key. All requests must include the following header:

```
Authorization: Bearer <API_KEY>
```

### Generate API Key

Generate a new API key to access the API by running the following command on the server as root.

```bash
cpgcli api --generate
```

**Example Output:**

```
root@server:~# cpgcli api --generate
API Key : 3cf8f550827dd687xxxxxxxxxxxxxxxx206d17bd3cd4d71f8df795285b
```

You can also list and manage API keys with the `cpgcli api` command. To retrieve all API keys created so far you can run the following command.

```bash
cpgcli api --list-keys
```

**Example Output:**

```
root@server:~# cpgcli api --list-keys
Active API Keys
----------------------------------------------------------------
3cf8f5xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx3cd4d71f8df795285b
```

---

## API Endpoints

### Status

Retrieve the current server status, including license information, portal connection, and security alerts.

```
POST https://localhost:9098/api/api.php?method=status
```

**Curl example:**

```bash
curl -X POST "http://localhost:9098/api/api.php?method=status" \
     -H "Authorization: Bearer 3cf8f5xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx3cd4d71f8df795285b"
```

**PHP Implementation:**

```php
<?php
$api_key = '3cf8f5xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx3cd4d71f8df795285b';
$url = "http://localhost:9098/api/api.php?method=status";
$ch = curl_init();
curl_setopt($ch, CURLOPT_URL, $url);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_POST, true);
curl_setopt($ch, CURLOPT_HTTPHEADER, [
    "Authorization: Bearer $apiKey"
]);
$response = curl_exec($ch);
$httpCode = curl_getinfo($ch, CURLINFO_HTTP_CODE);
curl_close($ch);
if ($httpCode == 200) {
    echo $response;
} else {
    echo "Failed to fetch status! http_code: $httpCode";
}
```

**Sample Response:**

```json
{
    "status": true,
    "response": {
        "server_id": 22,
        "license_status": "active",
        "portal_connection": "active",
        "alerts": [
            "daily scan is off",
            "Rootkit is off",
            "Scanner is off"
        ]
    },
    "version": 5.36,
    "errors": null
}
```

---

### User Report

Retrieve detailed reports for a specific user.

```
POST https://localhost:9098/api/api.php?method=userReport
```

**Payload:**

| Field | Type | Description |
|-------|------|-------------|
| `user`(required) | String  | The username for fetching reports. |

**Response Sample:**

```json
{
    "blacklisted_domains": [],
    "cms": [
        {
            "id": 41,
            "type": "wordpress",
            "domain": "dev1.hosting.com",
            "version": "6.7.1 (1)",
            "path": "\/home\/dev1\/public_html\/wordpress\/mindart\/wordpress",
            "status": "Working",
            "plugins": "up to date",
            "themes": "up to date"
        },
        {
            "id": 42,
            "type": "joomla",
            "domain": "dev1.hosting.com",
            "version": "4.0.0 (0)",
            "path": "\/home\/dev1\/public_html\/data_joomla",
            "status": "Working",
            "plugins": "up to date",
            "themes": "up to date"
        }
    ]
}
```

---

### Job Status

Retrieve the status of a specific job using its job ID.

```
POST https://localhost:9098/api/api.php?method=jobStatus
```

**Payload:**

| Field | Type | Description |
|-------|------|-------------|
| `id`(required) | integer  | ID of the Job. |

**Response Format:**

```json
{
    "status": [
        {
            "id": 41948,
            "name": "cms.update-component-all",
            "status": 1,
            "meta": {
                "cms_id": "50",
                "type": "wordpress",
                "domain": "wordpress.in",
                "component": "theme"
            },
            "stages": [
                {
                    "stage": "refreshing",
                    "data": []
                },
                {
                    "stage": "closed",
                    "data": []
                }
            ],
            "error": null
        }
    ]
}
```

---

### Manual Scans

Initiate a manual malware scan for a specified target.

```
POST https://localhost:9098/api/api.php?method=manualScan
```

**Payload:**

| Field | Type | Description |
|-------|------|-------------|
| `target`(required) | string  | The target path to scan. Use `"all"` for scanning all. |
| `user`(optional) | string  | The user performing the scan. |
| `suspicious_action`(optional) | string  | Action for suspicious detection: `email`, `disable`, `quarantine`. |
| `binary_action`(optional) | string  | Action for binary detection: `email`, `disable`, `quarantine`. |
| `virus_action`(optional) | string  | Action for virus detection: `email`, `disable`, `quarantine`. |
| `whitelist_file`(optional) | string  | Path to whitelist file. |
| `blacklist_file`(optional) | string  | Path to blacklist file. |

**Response Format:**

```json
{
    "id": 2278,
    "type": "path",
    "target": "path",
    "user": "path",
    "progress": {
        "completed": 0,
        "total": 100
    },
    "total_files": 0,
    "infected_count": 0,
    "status": "path",
    "start_time": "path",
    "end_time": null,
    "actions": {
        "delete": true
    }
}
```

---

### CMS List

Retrieve a list of CMS installations on the server.

```
POST https://localhost:9098/api/api.php?method=cmsList
```

**Payload:**

| Field | Type | Description |
|-------|------|-------------|
| `page`(optional) | string  | Page number. Default: `1`. |
| `length`(optional) | string  | Items per page. Default: `10`. |
| `order`(optional) | string  | Order details. |
| `filter`(optional) | array  | Filter conditions. |


**Filter use Case**

The filter parameter supports dynamic keys such as user, path, and domain etc to narrow down CMS listing results.

# Filter Usage Example

You can use the `filter` parameter to narrow down CMS listing results based on different keys such as `user`, `path`, or `domain`etc.

## Filter by User

```bash
curl -k -s -X POST "https://localhost:9098/api/api.php?method=cmsList" \
  -H "Authorization: Bearer xxxxxxxxxxxxxxxxxxxxx" \
  --data-urlencode "filter[user]=username"
```

**Response Format:**

```json
{
    "meta": {
        "page": 1,
        "per_page": 10,
        "total": 18,
        "total_pages": 2
    },
    "records": [
        {
            "id": 50,
            "domain": "domain.in",
            "user": "domain",
            "url": null,
            "path": "\/home\/domain\/public_html",
            "type": "wordpress",
            "version": "6.6.2",
            "latest_version": "6.7.1",
            "version_status": "outdated",
            "threat_rating": "safe",
            "threat_score": null,
            "status": 0,
            "issues_count": 0,
            "plugin_count": 2,
            "theme_count": 6,
            "plugin_outdated_count": 0,
            "theme_outdated_count": 0
        },
        {
            "id": 35,
            "domain": "wordpress.domain.in",
            "user": "domain",
            "url": null,
            "path": "\/home\/wordpress\/public_html\/wordpress_vfx",
            "type": "wordpress",
            "version": "6.6.2",
            "latest_version": "6.7.1",
            "version_status": "outdated",
            "threat_rating": "safe",
            "threat_score": null,
            "status": 0,
            "issues_count": 0,
            "plugin_count": 2,
            "theme_count": 6,
            "plugin_outdated_count": 0,
            "theme_outdated_count": 0
        }
    ]
}
```

---

### CMS Details

Retrieve details of specific CMS installations on the server.

```
POST http://localhost:9098/api/api.php?method=cmsDetails
```

**Payload:**

| Field | Type | Description |
|-------|------|-------------|
| `id`(required) | string  | ID of the CMS to retrieve data. |

**Response Format:**

```json
{
    "status": {
        "info": {
            "id": 50,
            "domain": "mercury.domain.com",
            "user": "mercury",
            "path": "\/home\/mercury\/public_html",
            "type": "wordpress",
            "url": null,
            "threat_rating": "Medium",
            "latest_version": "6.7.1",
            "version": "6.1.1",
            "version_status": "insecure",
            "threat_score": "6.6",
            "status": 1,
            "errors": [],
            "wp_cron": null,
            "issues_count": 12,
            "plugin_count": 9,
            "theme_count": 6,
            "plugin_outdated_count": 0,
            "theme_outdated_count": 0
        },
        "core_vulernabilities": {
            "vulnerabilities": [
                {
                    "title": "WordPress Core <= 6.4.3 - Sensitive Information Exposure via redirect_guess_404_permalink",
                    "description": "WordPress Core is vulnerable to Sensitive Information Exposure in versions up to, and including, 6.4.3 via the redirect_guess_404_permalink function. This can allow unauthenticated attackers to expose the slug of a custom post whose 'publicly_queryable' post status has been set to 'false'.",
                    "patched_versions": "6.5",
                    "remediation": "Update to version 6.5, or a newer patched version",
                    "cvss_rating": "Medium",
                    "cvss_score": 5.3,
                    "published": "2024-04-04 00:00:00",
                    "time": "2024-12-16 06:25:40"
                }
            ],
            "cvss_rating": "Medium",
            "cvss_score": "5.3",
            "published": "2024-04-04 00:00:00"
        },
        "plugin": [
            {
                "id": 320,
                "name": "Akismet Anti-spam: Spam Protection",
                "identifier": "akismet",
                "type": "plugin",
                "version": "5.3.5",
                "latest_version": "5.3.5",
                "update_available": "0",
                "auto_update": 1,
                "status": 0,
                "vulnerabilities": [],
                "cvss_rating": "safe",
                "cvss_score": 0,
                "published": 0
            }
        ],
        "theme": [
            {
                "id": 281,
                "name": "Twenty Nineteen",
                "identifier": "twentynineteen",
                "type": "theme",
                "version": "3.0",
                "latest_version": 3,
                "update_available": "0",
                "auto_update": 1,
                "status": 0,
                "vulnerabilities": [],
                "cvss_rating": "safe",
                "cvss_score": 0,
                "published": 0
            },
            {
                "id": 286,
                "name": "Twenty Twenty-Two",
                "identifier": "twentytwentytwo",
                "type": "theme",
                "version": "1.9",
                "latest_version": 1.9,
                "update_available": "0",
                "auto_update": 0,
                "status": 0,
                "vulnerabilities": [],
                "cvss_rating": "safe",
                "cvss_score": 0,
                "published": 0
            }
        ],
        "threat": []
    }
}
```

---

### Refresh CMS Threats

Refresh the security threat data for a CMS installation.

```
POST https://localhost:9098/api/api.php?method=cmsRefresh
```

**Payload:**

| Field | Type | Description |
|-------|------|-------------|
| `id`(required) | string  | ID of the CMS to refresh. |

**Response Format:**

```json
{
    "status": true,
    "response": {
        "status": "queued",
        "job_id": 42030
    },
    "version": 5.36,
    "errors": null
}
```

---

### Update Plugin, Theme and Core of CMS

Update core, plugins, or themes of a CMS installation.

```
POST https://localhost:9098/api/api.php?method=cmsUpdate
```

**Payload:**

| Field | Type | Description |
|-------|------|-------------|
| `id`(required) | string  | ID of the CMS or CMS component to update. |
| `type`(optional) | string  | Acceptable values: `all`, `core`, `theme`, `plugin`. |

**Response Format:**

```json
{
    "status": true,
    "response": {
        "status": "queued",
        "job_id": 41980
    },
    "version": 5.36,
    "errors": null
}
```

---

### Enable or Disable Auto Update for CMS Types

Enable or disable auto-updates for CMS components.

```
POST http://localhost:9098/api/api.php?method=cmsAutoUpdate
```

**Payload:**

| Field | Type | Description |
|-------|------|-------------|
| `id`(required) | string  | ID of the CMS or CMS component to update. |
| `type`(optional) | string  | Acceptable values: `all`, `core`, `theme`, `plugin`. |
| `action`(required) | string  | Acceptable values: `enable`, `disable`. |

> **Note:** In CMS auto-update, if the type is passed along with the ID and action, the ID will be treated as the CMS ID. If the type is not specified, the ID will be considered as the CMS component ID.

**Response Format:**

```json
{
    "status": true,
    "response": {
        "status": "queued",
        "job_id": 41980
    },
    "version": 5.36,
    "errors": null
}
```

---

### Whitelist IP on Server

Add given IP to whitelist.

```
POST http://localhost:9098/api/api.php?method=allowIp
```

**Payload:**

| Field | Type | Description |
|-------|------|-------------|
| `ip`(required) | string  | IP to whitelist on server. |
| `reason` | string | Reason for whitelisting IP. |

**Response Format:**

```json
{
    "status": true,
    "response": {
        "status": "added"
    },
    "version": 5.65,
    "errors": null,
    "alerts": null
}
```

---

### Check IP

Checking whether an IP address is present in any whitelist or blacklist on the server.

```
POST http://localhost:9098/api/api.php?method=checkIp
```

**Payload:**

| Field | Type | Description |
|-------|------|-------------|
| `ip`(required) | string  | IP address to check. |

**Response Format:**

```json
{
    "status": true,
    "response": [
        {
            "in": "whitelist-user",
            "match": "30.30.30.123",
            "reason": "Manually added - United States (US), Columbus, Ohio - ASN 749 DoD Network Information Center - at 2025-09-26 08:55 UTC"
        }
    ],
    "version": 5.65,
    "errors": null,
    "alerts": null
}
```

---

### Remove IP

Removing IPs from the blacklist or temporary block list, excluding IPDB and extra blocks.

```
POST http://localhost:9098/api/api.php?method=removeIp
```

**Payload:**

| Field | Type | Description |
|-------|------|-------------|
| `ip`(required) | string  | IP address to remove from blacklists. |

**Response Format:**

```json
{
    "status": true,
    "response": {
        "removed": true,
        "status": false,
        "exists": "ipdb"
    },
    "version": 5.65,
    "errors": null,
    "alerts": null
}
```

> **Note:** An IP in the blacklist or temporary block list is removed. If the IP is also present in another list, the response will show the list name in `"exists"`.

---


### Ignore IP

Adding IPs to ignore IP list, allows you to exclude specific IP addresses from firewall processing.
```
POST https://localhost:9098/api/api.php?method=ignoreIp
```
**Payload:**
| Field | Type | Description |
|-------|------|-------------|
| `ip`(required) | string  | IP address to add to the ignore list. |

**Response Format:**
```json
{
  "status": true,
  "response": {
    "status": "updated"
  },
  "version": "5.84.02",
  "errors": null,
  "alerts": null
}
```

---

### Remove Ignore IP

Remove an IP address from the ignore list.
```
POST http://localhost:9098/api/api.php?method=removeIgnoreIp
```
**Payload:**

| Field | Type | Description |
|-------|------|-------------|
| `ip`(required) | string  | IP address to remove from the ignore list. |

**Response Format:**
```json
{
  "status": true,
  "response": {
    "status": "updated"
  },
  "version": "5.84.03",
  "errors": null,
  "alerts": null
}
```

---

### Ignore Country

Add a country to the ignore list, preventing it from being blocked or flagged.
```
POST http://localhost:9098/api/api.php?method=ignoreCountry
```
**Payload:**
| Field | Type | Description |
|-------|------|-------------|
| `country`(required) | string  | Two-letter country code (ISO 3166-1 alpha-2) to add to the ignore list. |

**Response Format:**
```json
{
  "status": true,
  "response": {
    "status": "updated"
  },
  "version": "5.84.02",
  "errors": null,
  "alerts": null
}
```

---

### Remove Ignore Country

Remove a country from the ignore list.
```
POST http://localhost:9098/api/api.php?method=removeIgnoreCountry
```
**Payload:**
| Field | Type | Description |
|-------|------|-------------|
| `country`(required) | string  | Two-letter country code (ISO 3166-1 alpha-2) to remove from the ignore list. |

**Response Format:**
```json
{
  "status": true,
  "response": {
    "status": "updated"
  },
  "version": "5.84.02",
  "errors": null,
  "alerts": null
}
```

## Conclusion

This documentation provides a structured guide to using cPGuard's APIs for managing security, monitoring threats, and automating protection measures. Ensure that API keys are handled securely, and use authentication headers appropriately when making API requests.

For additional support, contact cPGuard's technical team.