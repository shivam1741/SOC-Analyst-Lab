# Splunk Backend file(Default and Local)
---

## Why We Never Modify the `default` Folder

Splunk apps typically have two folders:

```
default/
local/
```

### `default/`

* Contains Splunk’s original configuration files.
* Installed by Splunk or by an app.
* Can be overwritten during upgrades or app updates.

### `local/`

* Contains your custom changes.
* Created and maintained by administrators.
* Not overwritten during upgrades.



## Problem with Editing `default`

Suppose you edit:

```
$SPLUNK_HOME/etc/apps/search/default/inputs.conf
```

Later:

* Splunk is upgraded, or
* The app is updated.

Your changes may be lost because the `default` folder gets replaced.



## Why Copy to `local`

Instead, copy the relevant stanza to:

```
$SPLUNK_HOME/etc/apps/search/local/inputs.conf
```

Then make your changes there.

This preserves your custom configuration.


## How Splunk Uses Both Files

Splunk merges configuration files using precedence rules.

Priority (highest to lowest):

1. `local/`
2. `default/`

So if the same setting exists in both places, the value in `local/` wins.


## Example

### `default/inputs.conf`

```
[monitor:///var/log/messages]
disabled = 1
index = main
```

### `local/inputs.conf`

```
[monitor:///var/log/messages]
disabled = 0
```

### Final Effective Configuration

```
[monitor:///var/log/messages]
disabled = 0
index = main
```

Only the changed setting is overridden; everything else is inherited from `default`.



## Real-Life Analogy

Think of `default/` as the original blueprint of a house.

`local/` is your list of custom modifications:

* Paint the wall blue
* Add a fan
* Replace the door

The original blueprint stays intact, but your customizations override it.


##  Benefits of Using `local`

* Safe during upgrades
* Easy to identify your changes
* Easier troubleshooting
* Follows Splunk best practices


## Short Definition for Notes

> In Splunk, custom configuration changes should be made in the `local` directory instead of the `default` directory because files in `default` may be overwritten during upgrades. Splunk gives higher priority to settings in `local`, so those values override the defaults.   
> We do not modify the `default` files because they are system-provided and can be replaced during upgrades. We copy the required configuration to the `local` folder and make changes there. Splunk reads both folders and uses the values from `local` whenever the same setting exists in both places.


---
