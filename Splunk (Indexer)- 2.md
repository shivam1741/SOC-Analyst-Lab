# Splunk (Indexer)-2

## Indexer

**Indexer** is the core Splunk component that receives data from forwarders or other sources, parses the events, stores them in indexes, and makes them searchable for analysis and reporting.

## What Does an Indexer Do?

The Indexer performs five main tasks:

1. **Receives Data** from Universal Forwarders, Heavy Forwarders, Syslog devices, and other inputs.
2. **Parses Data** by identifying timestamps, host, source, and sourcetype.
3. **Indexes Data** by creating internal structures for fast searching.
4. **Stores Data** on disk in compressed files.
5. **Searches Data** when a Search Head sends a query.

## Data Flow in Splunk
```
Source System → Forwarder → Indexer → Search Head → User
```
### Example

```
Windows Server → Universal Forwarder → Indexer → Search Head
```

## What Is an Index?

An **index** is a logical container where Splunk stores events or logs.

Simple Definition
> An index is a searchable repository used by Splunk to store log data.

### Common Default Indexes

* `main` – Default index for most data.
* `_internal` – Splunk's own operational logs.
* `_audit` – Tracks user activity and configuration changes.
* `_introspection` – Performance monitoring data.

### Custom Indexes
Organizations often create custom indexes such as:

* `windows`
* `linux`
* `firewall`
* `security`
* `application`


## Why Use Different Indexes?

Indexes help:

* Organize data by source or use case.
* Apply different retention policies.
* Control access to specific datasets.
* Improve search efficiency.


## How Splunk Stores Data Internally

Each index is made up of multiple **buckets**.

```
Index → Buckets → Events
```


## What Is a Bucket?

A **bucket** is a physical directory on disk that contains a portion of indexed data.

Simple Definition
> A bucket is a storage unit inside an index that holds compressed events and index files.

### Types of Buckets

***1. Hot Bucket***
A Hot Bucket is the current bucket where Splunk writes new events.

* Receives incoming data.
* Actively written to.
* Searchable.


***2. Warm Bucket***
A Warm Bucket contains older data that remains searchable but is no longer being written to.

* No longer receives new data.
* Still searchable.
* Created when a hot bucket rolls over.

***3. Cold Bucket***
A Cold Bucket stores older data that is still searchable.

* Older searchable data.
* Usually moved to cheaper storage.

***4. Frozen Bucket***
A Frozen Bucket contains expired data that Splunk removes from active storage.

* No longer searchable.
* Deleted or archived.


***5. Thawed Bucket***
A Thawed Bucket is archived data restored back into Splunk for searching.

* Data restored from archived frozen data.
* Searchable again.



---

# Bucket Lifecycle

```text
Hot → Warm → Cold → Frozen → Thawed (optional)
```

---

# Real-Life Analogy

Imagine a filing cabinet:

* **Hot** = files currently being added.
* **Warm** = recently completed files.
* **Cold** = older files stored in a warehouse.
* **Frozen** = archived or discarded files.
* **Thawed** = archived files brought back.

---

# Metadata Stored with Events

Every event is associated with:

* `host` – system that generated the event.
* `source` – file or input source.
* `sourcetype` – data format.
* `index` – storage location.

### Example

```text
host=WIN-SERVER01
source=WinEventLog:Security
sourcetype=WinEventLog
index=windows
```

---

# Indexer vs Search Head

| Component   | Role                               |
| ----------- | ---------------------------------- |
| Indexer     | Stores and searches data           |
| Search Head | Sends queries and displays results |

---

# Indexer vs Forwarder

| Component           | Role                    |
| ------------------- | ----------------------- |
| Universal Forwarder | Collects and sends data |
| Indexer             | Stores and indexes data |

---

# Important Indexer Ports

| Port | Purpose                       |
| ---: | ----------------------------- |
| 9997 | Receives data from forwarders |
| 8089 | Management/API port           |
| 8000 | Splunk Web (if enabled)       |

---

# Example Workflow

1. A Windows server generates a failed login event.
2. Universal Forwarder sends the event to the Indexer.
3. Indexer parses the event.
4. Event is stored in the `windows` index.
5. Search Head retrieves it when you search.

---

# Sample Search

```spl
index=windows EventCode=4625
```

This search returns failed login events stored in the `windows` index.

---

# One-Line Summary

> The Indexer is the Splunk component that receives data, parses it, stores it in indexes, and makes it searchable.

---

# Interview Questions

### What is an Indexer?

The Splunk component that receives, parses, stores, and searches data.

### What is an Index?

A logical container where Splunk stores events.

### What is a Bucket?

A physical storage directory within an index.

### What is the Bucket Lifecycle?

Hot → Warm → Cold → Frozen → Thawed.

### Which Port Does the Indexer Use to Receive Data?

TCP 9997.
---

# Note (Metadata)

## What Is Metadata in Splunk?

* Metadata is information about each event that describes the event but is not necessarily part of the raw log text itself.
* Splunk automatically attaches metadata to every event.


## Main Metadata Fields

| Metadata Field | Meaning                                                 |
| -------------- | ------------------------------------------------------- |
| `host`         | The system that generated the event                     |
| `source`       | The file path, input, or source that produced the event |
| `sourcetype`   | The format/type of the data                             |
| `index`        | The index where the event is stored                     |
| `_time`        | The timestamp of the event                              |


## Example

### Raw Event

```
May 17 10:30:15 FW01 %ASA-6-302013: Built outbound TCP connection...
```

### Metadata Attached by Splunk

* `host = FW01`
* `source = udp:514`
* `sourcetype = cisco:asa`
* `index = network`
* `_time = 2026-05-17 10:30:15`


# Why Metadata Is Important

Metadata allows Splunk to:

* Organize data
* Search efficiently
* Apply parsing rules
* Extract fields
* Drive dashboards and alerts


## What Is Sourcetype?

* Sourcetype is a metadata field that tells Splunk what kind of log format it is receiving.

Examples:

* `cisco:asa`
* `WinEventLog:Security`
* `linux_secure`
* `pan:traffic`


## Why Sourcetype Must Match the Add-on's Sourcetype

* Splunk add-ons are built to recognize specific sourcetypes.
* Inside an add-on, configuration files such as `props.conf` and `transforms.conf` are written under those exact sourcetype names.


## Example

The Cisco ASA add-on may contain:

```
[cisco:asa]
REPORT-extractions = cisco_asa_fields
```

This means these field extractions run only when:

```
sourcetype = cisco:asa
```

## If Sourcetype Does Not Match

Suppose you ingest the same logs using:

```
sourcetype = my_firewall_logs
```

Then:

* Data is indexed.
* The add-on does not recognize it.
* Fields are not extracted.
* Dashboards may not work.
* CIM mappings may fail.


## Real-Life Analogy

Imagine a hospital.

* Raw log = patient
* Metadata = patient ID card
* Sourcetype = department label (Cardiology, Neurology)
* Add-on = doctor specialized for that department

If the label is wrong, the patient goes to the wrong doctor.

---
