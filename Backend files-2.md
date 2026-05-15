# Backend Files
---
## Important Splunk Configuration Files and Their Full Locations

> **Base path:**    
> `$SPLUNK_HOME = /opt/splunk` on Linux    
> `C:\Program Files\Splunk` on Windows

* Most GUI changes are written to:   
> `$SPLUNK_HOME/etc/system/local/`   
or   
> `$SPLUNK_HOME/etc/apps/<app_name>/local/`


## Core Configuration Backend Files locations

| Task / GUI Activity               | Configuration File              | Full Location                                            |
| --------------------------------- | ------------------------------- | -------------------------------------------------------- |
| Create Index                      | `indexes.conf`                  | `$SPLUNK_HOME/etc/system/local/indexes.conf`             |
| Create Data Inputs                | `inputs.conf`                   | `$SPLUNK_HOME/etc/system/local/inputs.conf`              |
| Configure Forwarding              | `outputs.conf`                  | `$SPLUNK_HOME/etc/system/local/outputs.conf`             |
| Saved Searches / Alerts / Reports | `savedsearches.conf`            | `$SPLUNK_HOME/etc/apps/search/local/savedsearches.conf`  |
| Search Macros                     | `macros.conf`                   | `$SPLUNK_HOME/etc/apps/search/local/macros.conf`         |
| Event Types                       | `eventtypes.conf`               | `$SPLUNK_HOME/etc/apps/search/local/eventtypes.conf`     |
| Tags                              | `tags.conf`                     | `$SPLUNK_HOME/etc/apps/search/local/tags.conf`           |
| Field Extractions                 | `props.conf`, `transforms.conf` | `$SPLUNK_HOME/etc/apps/search/local/`                    |
| Lookups                           | `transforms.conf`               | `$SPLUNK_HOME/etc/apps/search/local/transforms.conf`     |
| Roles and Permissions             | `authorize.conf`                | `$SPLUNK_HOME/etc/system/local/authorize.conf`           |
| Authentication Settings           | `authentication.conf`           | `$SPLUNK_HOME/etc/system/local/authentication.conf`      |
| Web UI Settings                   | `web.conf`                      | `$SPLUNK_HOME/etc/system/local/web.conf`                 |
| General Server Settings           | `server.conf`                   | `$SPLUNK_HOME/etc/system/local/server.conf`              |
| Distributed Search                | `distsearch.conf`               | `$SPLUNK_HOME/etc/system/local/distsearch.conf`          |
| Deployment Server Classes         | `serverclass.conf`              | `$SPLUNK_HOME/etc/system/local/serverclass.conf`         |
| License Settings                  | `server.conf`                   | `$SPLUNK_HOME/etc/system/local/server.conf`              |
| Data Models                       | `datamodels.conf`               | `$SPLUNK_HOME/etc/apps/<app_name>/local/datamodels.conf` |



## Add-ons (Technology Add-ons)

When you install an add-on, it is stored in:

```
$SPLUNK_HOME/etc/apps/<TA_name>/
```

Example:

```
$SPLUNK_HOME/etc/apps/Splunk_TA_windows/
```

Inside it:

```
default/
local/
metadata/
bin/
lookups/
```

Your custom changes should go into:

```
$SPLUNK_HOME/etc/apps/Splunk_TA_windows/local/
```


## Saved Search Example

If you create an alert in the Search app, Splunk writes it to:

```
$SPLUNK_HOME/etc/apps/search/local/savedsearches.conf
```

---

## Index Example

If you create an index named `security` then it configurations will store in below both locations.
```
$SPLUNK_HOME/etc/system/local/indexes.conf
```
And
```
$SPLUNK_HOME/etc/apps/search/local/indexes.conf
```
### Common Locations for indexes.conf and Data of indexes

* Splunk may store index definitions or configurations in:
```
$SPLUNK_HOME/etc/apps/search/local/indexes.conf
$SPLUNK_HOME/etc/system/local/indexes.conf
$SPLUNK_HOME/etc/apps/<custom_app>/local/indexes.conf
```
All are valid.
* But Splunk store index data (hot/warm/cold) in
```
C:\Program Files\Splunk\var\lib\splunk
```


## Data Input Example

If you create a WinEventLog or monitor input:

```
$SPLUNK_HOME/etc/system/local/inputs.conf
```


## Universal Forwarder Configuration

On a Universal Forwarder, the most common files are:

| File                    | Location                                              |
| ----------------------- | ----------------------------------------------------- |
| `inputs.conf`           | `$SPLUNK_HOME/etc/system/local/inputs.conf`           |
| `outputs.conf`          | `$SPLUNK_HOME/etc/system/local/outputs.conf`          |
| `deploymentclient.conf` | `$SPLUNK_HOME/etc/system/local/deploymentclient.conf` |

---

## 🔍 How to Find the Effective Configuration

```bash
splunk btool indexes list --debug
splunk btool inputs list --debug
splunk btool outputs list --debug
splunk btool savedsearches list --debug
```

This shows:

* The final value used by Splunk
* The exact file path where it came from

---

# 📝 Short Notes Version

> Splunk stores GUI and manual configuration changes in `.conf` files under `$SPLUNK_HOME/etc/system/local/` or `$SPLUNK_HOME/etc/apps/<app_name>/local/`. For example, indexes are stored in `indexes.conf`, data inputs in `inputs.conf`, saved searches in `savedsearches.conf`, and field extractions in `props.conf` and `transforms.conf`.

---

# ⭐ Most Important Files to Memorize

* `$SPLUNK_HOME/etc/system/local/indexes.conf`
* `$SPLUNK_HOME/etc/system/local/inputs.conf`
* `$SPLUNK_HOME/etc/system/local/outputs.conf`
* `$SPLUNK_HOME/etc/apps/search/local/savedsearches.conf`
* `$SPLUNK_HOME/etc/apps/search/local/macros.conf`
* `$SPLUNK_HOME/etc/apps/search/local/props.conf`
* `$SPLUNK_HOME/etc/apps/search/local/transforms.conf`
* `$SPLUNK_HOME/etc/system/local/authorize.conf`
* `$SPLUNK_HOME/etc/system/local/serverclass.conf`
