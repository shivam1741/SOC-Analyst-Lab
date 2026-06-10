
# DNS, SPF, DKIM, DMARC & Email Spoofing 

## 1. Why Email Spoofing Is Possible

* Email uses SMTP.
* Originally SMTP trusted the sender.

An attacker could send:

```
From: ceo@company.com
```

without owning that mailbox.

This is called **Email Spoofing**.

### Email Spoofing

**Definition:**

Email spoofing is the act of forging the sender's email address to make an email appear as if it originated from a trusted source.

### Example

```
From: ceo@company.com
To: employee@gmail.com

Transfer money urgently.
```

The email appears to come from the CEO even though it was sent by an attacker.


# 2. DNS (Domain Name System)

## Definition

DNS is a distributed database that translates domain names into IP addresses and stores service-related information.

### Example

```
google.com
↓
142.250.x.x
```


## Common DNS Records

### A Record

Maps hostname to IPv4 address.

```text
company.com → 1.2.3.4
```

### AAAA Record

Maps hostname to IPv6 address.

### MX Record

Specifies which server receives emails.

```text
company.com
↓
mail.company.com
```

### TXT Record

Stores text information.

Used by:

* SPF
* DKIM
* DMARC

### CNAME Record

Creates an alias.

```text
mail.company.com
↓
smtp.company.com
```

---

# 3. SPF (Sender Policy Framework)

## Definition

SPF is a DNS TXT record that specifies which mail servers are authorized to send email for a domain.

---

## Purpose

SPF answers:

```text
Did this email come from an authorized mail server?
```

---

## SPF Record Example

```text
v=spf1 ip4:1.2.3.4 -all
```

Meaning:

```text
Only 1.2.3.4 can send emails
for this domain.
```

---

## SPF Process

### Step 1

Email arrives:

```text
From: ceo@company.com
```

### Step 2

Receiving server checks:

```text
SPF record of company.com
```

### Step 3

Compares:

```text
Actual Sending IP

vs

Authorized IPs in SPF
```

### Step 4

Decision:

```text
Match
↓
SPF PASS
```

```text
No Match
↓
SPF FAIL
```

---

## What IP Does SPF Check?

SPF checks:

```text
The IP address of the sending mail server.
```

NOT:

```text
User Laptop IP
Home WiFi IP
Recipient IP
```

---

## Limitation of SPF

SPF only verifies:

```text
Who sent the email?
```

It does not verify:

```text
Whether email was modified.
```

---

# 4. DKIM (DomainKeys Identified Mail)

## Definition

DKIM uses digital signatures to verify that an email was authorized by the domain owner and was not modified during transit.

---

## Purpose

DKIM answers:

```text
Was the email modified?
```

and

```text
Was it signed by the domain owner?
```

---

# DKIM Components

## Private Key

Stored secretly on sender mail server.

Used for:

```text
Creating signatures.
```

---

## Public Key

Stored in DNS.

Used for:

```text
Verifying signatures.
```

---

# DKIM Process

### Step 1

Create email.

```text
Hello Shivam
```

### Step 2

Generate hash.

```text
Email
↓
SHA-256
↓
Hash
```

### Step 3

Sign hash.

```text
Hash
↓
Private Key
↓
Digital Signature
```

### Step 4

Send email.

```text
Email + Signature
```

### Step 5

Receiver gets email.

### Step 6

Receiver generates hash again.

### Step 7

Receiver downloads public key from DNS.

### Step 8

Verify signature.

Result:

```text
Valid Signature
↓
DKIM PASS
```

```text
Invalid Signature
↓
DKIM FAIL
```

---

# Why DKIM Works

Private and public keys are different.

```text
Private Key ≠ Public Key
```

But they are mathematically related.

Only the correct public key can verify signatures created by the corresponding private key.

---

# DKIM Use Case

Suppose original email:

```text
Amount = ₹1000
```

Attacker changes:

```text
Amount = ₹100000
```

Hash changes.

Signature becomes invalid.

Result:

```text
DKIM FAIL
```

---

# 5. DMARC

## Definition

DMARC is a DNS TXT record that tells receiving mail servers what action to take when SPF and/or DKIM checks fail.

---

## Purpose

DMARC answers:

```text
What should I do if authentication fails?
```

---

# DMARC Policies

## Monitor Only

```text
p=none
```

Meaning:

```text
Collect reports only.
```

---

## Quarantine

```text
p=quarantine
```

Meaning:

```text
Move suspicious emails
to spam/junk folder.
```

---

## Reject

```text
p=reject
```

Meaning:

```text
Reject the email completely.
```

---

# DMARC Example

```text
v=DMARC1; p=reject;
```

Meaning:

```text
Reject emails that fail
authentication checks.
```

---

# 6. Relationship Between SPF, DKIM and DMARC

## SPF

Checks:

```text
Who sent the email?
```

---

## DKIM

Checks:

```text
Was the email altered?
```

---

## DMARC

Checks:

```text
What should happen if
authentication fails?
```

---

# Easy Memory Trick

```text
SPF
↓
Who sent it?

DKIM
↓
Was it altered?

DMARC
↓
What should I do?
```

---

# Interview Questions

### What is Email Spoofing?

Forging the sender's email address to impersonate another sender.

---

### What is SPF?

A DNS TXT record that specifies authorized mail servers for a domain.

---

### What is DKIM?

A digital signature mechanism that verifies email integrity and authenticity.

---

### What is DMARC?

A policy framework that tells receiving servers how to handle emails that fail SPF or DKIM.

---

### Where is SPF Stored?

```text
DNS TXT Record
```

### Where is DKIM Public Key Stored?

```text
DNS TXT Record
```

### Where is DKIM Private Key Stored?

```text
Mail Server
```

### Does SPF Check User IP?

```text
No
```

It checks:

```text
Sending Mail Server IP
```

---

### One-Line Summary

```text
DNS stores email security information.

SPF verifies authorized mail servers.

DKIM verifies integrity using digital signatures.

DMARC tells receivers what action to take when checks fail.
```

These notes are enough for most SOC Level-1 interviews and will give you a strong foundation for analyzing phishing and spoofed emails.
