
## 1. What is SOC?

* SOC is a **team that monitors, detects, and responds to security threats**
* They protect systems using:

    * Logs
    * Tools (SIEM)
    * Monitoring

## 2. What are Logs?

*  **Logs = records of events**
   > Login attempts   
   > Errors   
   > Network activity

* Stored in:
  > Linux → `/var/log/`   
  > Windows → Event Viewer

* Logs are **not permanent** → they expire (retention policy)


## 3. SIEM Concept

* **SIEM = Security Information and Event Management**
   * It Collects logs from different systems
   * Analyzes them
   * Detects suspicious activity

## 4. Splunk (SIEM Tool)

* Splunk is a **tool used for SIEM**   
It helps to:   
    * Search logs
    * Create dashboards
    * Detect attacks


## 5. Log Flow 

* **Linux/Server → Logs → SIEM (Splunk) → You analyze**

* You don’t always go to server
* You mostly use **Splunk dashboard**



## 6. Linux Role in SOC   
Not always directly used, BUT important:
* Logs are generated in Linux
* Used for troubleshooting
* Used to configure log forwarding


## 7. Can anyone see logs?

* No
* Logs are **private & sensitive**
* Only authorized people can access


## 8. Can hackers delete logs?
They can try 

* Delete / modify / stop logs
But:

* SIEM keeps copies
* Alerts are generated


## 9. How you view logs (practical)
* On Linux:

```
cd /var/log
tail -f filename.log
grep "error" filename.log
```
---
