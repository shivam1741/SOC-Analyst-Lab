# Splunk (Forwarder)-1

### Splunk 
Splunk is used as
 * SIEM
 * Data Analytics

### Splunk Deployment Modal
As a SIEM Splunk offer two ways to use
1. Splunk Enterprise(on-premises)
2. Splunk Cloud

## Splunk deployment Architecture (Splunk Enterprise(on-premises))
It has 3 parts
1. Forwarder
2. Indexer
3. Search Head
Let's talk about the Forwarder first
---
## Forwarder:
A forwarder is a lightweight Splunk component that collects logs from a machine and sends them to another Splunk instance, usually an Indexer.

Simple Definition
> A forwarder is an agent installed on a server that reads logs and forwards them to Splunk(Indexer) for indexing and analysis.

## Types of Forwarders in Splunk
### 1. Universal Forwarder (UF)
The most commonly used forwarder.

***Characteristics:***
* Very lightweight agent.
* Uses minimal CPU, memory, and disk.
* Collects and forwards data only.
* Does not perform full event parsing or indexing.

***Use Cases***
* Windows servers
* Linux servers
* Application servers

Simple Definition
> Universal Forwarder is a lightweight Splunk agent used to collect and send logs to Splunk.

### 2. Heavy Forwarder (HF)
A full Splunk Enterprise instance configured to forward data.

***Characteristics***
* Performs parsing and filtering.
* Can run apps and add-ons.
* Can route data to multiple destinations
* Uses more resources than a UF.

***Use Cases***
* Filtering unwanted logs
* Masking sensitive data
* Receiving data from syslog devices
* Protocol conversion

Simple Definition
> Heavy Forwarder is a Splunk Enterprise instance that can parse, transform, and forward data.

## Example Scenario

* You have 100 Windows servers.
> Install a Universal Forwarder on each server.   
> Each UF sends logs to a central Indexer.

* If you need to filter or modify logs before indexing:
> Send data to a Heavy Forwarder first.   
> The HF processes and forwards the data to the Indexer.

Now Let's Understand about Syslog 

---
# Note:(Syslog)

## Two Ways Logs Reach to Splunk

### 1. Log File on Disk

* The application or operating system writes logs to a file.
* The Splunk Universal Forwarder reads the file and sends the contents to Splunk.

### 2. Syslog Over the Network
* The device sends the log immediately over the network using the Syslog protocol.
Example:
> Firewall → Syslog → Splunk

## Real-Life Analogy
Imagine you want to send a letter.

### Method 1: Store in a Notebook
* You write the message in a notebook. Later, someone reads the notebook and delivers the message.

### Method 2: Send Directly by Courier
* You hand the message directly to a courier who delivers it immediately.
> The message is the same; only the delivery method differs.

## Why Network Devices Commonly Use Syslog
Devices such as routers, switches, and firewalls:

* Often do not allow installing software agents.
* Need a standard protocol to send logs externally.

So they use Syslog.

## Syslog
* Syslog (System Logging Protocol) is a standard method used by devices and operating systems to generate and send log messages to a central server.
* Protocols Used
  > UDP 514   
  > TCP 514   
  > TLS (Transport Layer Security) 6514

Simple Definition
> Syslog is a protocol used to send logs from devices such as firewalls, routers, switches, and Linux servers to a centralized log server.

## Common Devices That Use Syslog
* Firewalls
* Routers
* Switches
* Linux servers
* Load balancers
* IDS/IPS systems 

---

