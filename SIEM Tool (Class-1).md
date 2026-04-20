# Understanding SIEM(Security Information and Event Managment) Systems

## What is SIEM?

* **SIEM = Security Information and Event Management**
* It is a security system that collects, analyzes, and monitors logs from different sources to detect threats.
* In one line:  
  > SIEM = Central brain that watches everything happening in your IT system


## Why SIEM is used?

Companies use SIEM to:

* Detect attacks (hackers, malware, insider threats)
* Monitor user activity
* Meet compliance (ISO, GDPR, etc.)
* Investigate incidents


## Two Parts of SIEM

### 1. SIM (Security Information Management)

* Stores logs
* Helps in historical analysis
* **Example:** What happened 2 days ago?

### 2. SEM (Security Event Management)

* Works in real-time
* Detects attacks instantly
* **Example:** Alert when brute-force attack happens


## What SIEM Collects   
SIEM collects logs from:

* Firewalls 
* Routers & switches
* Servers (Linux/Windows)
* Applications
* Antivirus / EDR
* Databases
* Cloud (AWS, Azure)
* Basically: **Everything that generates logs**


## Important Components of SIEM

### 1. Log Collection
* Collect logs from different Network devices, Security devices, Servers, Applications, and databases.

### 2. Log Normalization
* Convert logs into a **common format** or Convert logs to unreadable to readable form.
**Example:**
Different logs → Same structur

### 3. Correlation Engine (Very Important)
* Connects multiple events to detect attacks
**Example:** 5 failed logins + 1 success → Suspicious

### 4. Alerting
* Sends alert when something suspicious happens

### 5. Dashboard
* Shows real-time security status

### 6. Storage
* Stores logs for months/years



## Types of SIEM Logs

* **Authentication logs** → login/logout
* **Network logs** → traffic data
* **System logs** → OS activity
* **Application logs** → app behavior


## How SIEM Works

1. Logs are generated
2. SIEM collects logs
3. Logs are normalized
4. Correlation rules applied
5. Alerts generated
6. Analyst investigates



## Real Example 

Suppose:
* Someone tries password 100 times in 2 minutes (failed)
* Then logs in successfully

SIEM detects: **Possible brute-force attack**


## SIEM Tools 

* Splunk
* IBM QRadar
* ArcSight
* LogRhythm
* Wazuh


## Advantages

* Centralized monitoring
* Early threat detection
* Helps in investigation
* Compliance support


## Disadvantages

* Expensive
* Complex to manage
* Requires skilled analysts
* Many false alerts (false positives)


## Important Terms

* **Event** → Any activity
* **Log** → Record of event
* **Alert** → Warning
* **Incident** → Confirmed attack
* **Correlation** → Linking events
---

# Correlation (SIEM)

## Correlation

In SIEM tools, every second we receive **thousands or even millions of logs continuously**, and it is not possible for humans to monitor each log manually.
So, a concept called **correlation** is used.

Correlation helps in **analyzing multiple logs together** and identifying meaningful patterns or suspicious activities.

By writing **correlation rules**, we can:

* Detect attacks automatically
* Reduce manual effort
* Identify real threats from huge data

These rules are written using SIEM query languages like:

* Splunk → SPL
* Microsoft Sentinel → KQL


### Final Line:

**Correlation in SIEM = combining multiple logs using rules to detect security incidents efficiently**

---


