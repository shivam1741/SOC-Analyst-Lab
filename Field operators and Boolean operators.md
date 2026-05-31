# Field operators and Boolean operators
---
## Why do we use Field Operators and Boolean Operators?

* They help us **search logs quickly and efficiently**.
* They allow us to **filter only the relevant events** from millions of logs.
* They make log analysis **more systematic and accurate**.
* They help combine multiple conditions to perform **targeted investigations**.
* They reduce noise and save time during **SOC monitoring and incident response**.

### Examples

* **Field Operators (`=`, `!=`, `>`, `<`)** are used to compare field values.
* **Boolean Operators (`AND`, `OR`, `NOT`)** are used to combine or exclude search conditions.
---


# Field operators 

## 1. Equal To (=)

Find events where a field has an exact value.

```
action=allow
```

Meaning:

> Show me all events where the action is allow.

### More Examples

```
src_ip=192.168.1.10
```

```
app=YouTube
```

```
user=admin
```


## 2. Not Equal To (`!=`)

Exclude a value.

```
action!=allow
```

Meaning:

> Show everything except allowed traffic.

### Example

```
user!=admin
```


## 3. Greater Than (`>`)

Find values larger than a number.

```
bytes>1000000
```

Meaning:

> Show events with more than 1 MB of traffic.

### Example

```
duration>300
```

Meaning:

> Sessions longer than 5 minutes.



## 4. Less Than (`<`)

```
bytes<100
```

Meaning:

> Traffic smaller than 100 bytes.


## 5. Greater Than or Equal (`>=`)

```
bytes>=1000000
```


## 6. Less Than or Equal (`<=`)

```
bytes<=500
```


## 7. Wildcards (`*`)

Find partial matches.

```
user=adm*
```

Matches:

```
admin
administrator
admin01
```

---

# Boolean Operators

Think of them like logic gates.


## AND

Both conditions must be true.

```
action=allow AND app=YouTube
```

Meaning:

> Allowed YouTube traffic only.

### Example

```
src_ip=10.0.0.5 AND dest_port=443
```


# OR

Either condition can be true.

```
action=allow OR action=deny
```

Meaning:

> Show allowed and denied traffic.

### Example

```
app=YouTube OR app=Netflix
```

---

# NOT

Exclude something.

```spl
NOT action=allow
```

Meaning:

> Show everything except allowed traffic.

### Example

```spl
NOT user=admin
```

---

# Combining Operators

Example:

```spl
src_ip=10.0.0.5 AND action=allow AND bytes>1000000
```

Meaning:

> Show traffic from 10.0.0.5 that was allowed and transferred more than 1 MB.

---

# Parentheses

Very important.

```spl
(action=allow OR action=accept) AND app=YouTube
```

Meaning:

> YouTube traffic where action is either allow or accept.

---

# Real SOC Examples

### Find all failed logins

```spl
action=failure
```

---

### Find all blocked traffic

```spl
action=deny
```

---

### Find traffic to Google DNS

```spl
dest_ip=8.8.8.8
```

---

### Find large downloads

```spl
bytes_in>10000000
```

---

### Find SSH traffic

```spl
dest_port=22
```

---

### Find RDP traffic

```spl
dest_port=3389
```

---

### Find PowerShell events

```spl
process_name=powershell.exe
```

---

# Practice Tasks

Try to write the SPL yourself before looking at the answer.

---

### Task 1

Find all allowed traffic.

**Answer**

```spl
action=allow
```

---

### Task 2

Find all denied traffic.

**Answer**

```spl
action=deny
```

---

### Task 3

Find traffic from IP 192.168.1.100.

**Answer**

```spl
src_ip=192.168.1.100
```

---

### Task 4

Find traffic to port 443.

**Answer**

```spl
dest_port=443
```

---

### Task 5

Find allowed SSH traffic.

**Answer**

```spl
action=allow AND dest_port=22
```

---

### Task 6

Find traffic where bytes are greater than 1 MB.

**Answer**

```spl
bytes>1000000
```

---

### Task 7

Find traffic where application is YouTube or Netflix.

**Answer**

```spl
app=YouTube OR app=Netflix
```

---

### Task 8

Find all users except admin.

**Answer**

```spl
user!=admin
```

---

### Task 9

Find blocked RDP traffic.

**Answer**

```spl
action=deny AND dest_port=3389
```

---

### Task 10 (SOC Interview Level)

Find events where:

* Source IP = 10.0.0.5
* Destination Port = 443
* Action = allow
* Bytes > 1,000,000

**Answer**

```spl
src_ip=10.0.0.5 AND dest_port=443 AND action=allow AND bytes>1000000
```

### One important thing, bro

Before writing searches, always identify:

```text
Field = What information?
Value = What data inside that field?
```

Example:

```text
Field: action
Value: allow

Field: src_ip
Value: 192.168.1.10

Field: app
Value: YouTube
```

Once you understand **field + value**, Splunk searching becomes much easier.
