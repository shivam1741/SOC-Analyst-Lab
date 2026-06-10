
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

```
company.com → 1.2.3.4
```

### AAAA Record

Maps hostname to IPv6 address.

### MX Record

Specifies which server receives emails.

```
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

```
mail.company.com
↓
smtp.company.com
```


# 3. SPF (Sender Policy Framework)

## Definition

SPF is a DNS TXT record that specifies which mail servers are authorized to send email for a domain.

---

## Purpose

SPF answers:

```
Did this email come from an authorized mail server?
```


## SPF Record Example

```
v=spf1 ip4:1.2.3.4 -all
```

Meaning:

```
Only 1.2.3.4 can send emails
for this domain.
```


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

---
---


Yes bro. Let's forget all the technical definitions for 5 minutes and follow **one email in Outlook** from start to finish.

# Scenario

You work at DXC.

Your email:

```text
shivam@dxc.com
```

You send an email from Outlook to:

```text
friend@gmail.com
```

---

# Step 1: You Open Outlook

You type:

```text
To: friend@gmail.com
Subject: Hello

Hi Bro
```

and click **Send**.

---

# Step 2: Outlook Needs To Find DXC's Mail Server

Outlook asks DNS:

```text
Where is the mail server for dxc.com?
```

DNS replies using **MX Record**:

```text
dxc.com
   ↓
mail.dxc.com
```

### Why MX?

Because MX tells:

> Which server handles emails for this domain.

---

# Step 3: Need IP of mail.dxc.com

Now Outlook knows:

```text
mail.dxc.com
```

But computers need IPs.

DNS is asked again:

```text
mail.dxc.com
```

DNS returns an **A Record**:

```text
mail.dxc.com
      ↓
20.30.40.50
```

### Why A Record?

Because A Record means:

```text
Hostname → IP Address
```

---

# Step 4: Email Reaches DXC Mail Server

```text
Outlook
   ↓
20.30.40.50
(DXC Mail Server)
```

---

# Step 5: SPF Comes Into Picture

When Gmail receives the email, Gmail asks:

```text
This email claims to be from dxc.com.

Which servers are allowed
to send mail for dxc.com?
```

DNS returns the SPF record:

```text
v=spf1 ip4:20.30.40.50 -all
```

Gmail checks:

```text
Did email come from
20.30.40.50 ?
```

If yes:

```text
SPF PASS
```

---

# Step 6: DKIM Comes Into Picture

Before sending, DXC's mail server creates a digital signature.

To verify it, Gmail asks DNS:

```text
Give me DXC's public key.
```

DNS returns the DKIM public key.

Gmail verifies the signature.

If valid:

```text
DKIM PASS
```

---

# Step 7: DMARC Comes Into Picture

Suppose SPF fails.

Suppose DKIM fails.

Now Gmail asks:

```text
What does dxc.com
want me to do?
```

DNS returns:

```text
v=DMARC1; p=reject;
```

Meaning:

```text
Reject the email.
```

---

# Where Does CNAME Come?

This is the part confusing you.

Imagine DXC uses Microsoft 365.

Microsoft says:

```text
Use:

autodiscover.dxc.com
```

DXC creates:

```text
autodiscover.dxc.com
          ↓
        CNAME
          ↓
autodiscover.outlook.com
```

Now Outlook asks:

```text
Where are my mailbox settings?
```

DNS says:

```text
Go to
autodiscover.outlook.com
```

### Why not directly use Outlook's IP?

Because Microsoft keeps changing IPs.

Today:

```text
40.1.1.1
```

Tomorrow:

```text
52.2.2.2
```

If DXC stored Microsoft's IP directly, it would constantly break.

Instead:

```text
autodiscover.dxc.com
       ↓
CNAME
       ↓
autodiscover.outlook.com
```

Microsoft manages the IP changes.

DXC doesn't need to care.

---

# One Big Picture

```text
You Send Email
      ↓
MX Record
      ↓
Find Mail Server

mail.dxc.com
      ↓
A Record
      ↓
20.30.40.50

Email Sent
      ↓
Gmail Receives

SPF
↓
Is sender server authorized?

DKIM
↓
Was email altered?

DMARC
↓
What should I do if checks fail?

CNAME
↓
Alias to another hostname
(mainly for easier management)
```

### Interview Cheat Sheet

| Record | Purpose                             |
| ------ | ----------------------------------- |
| A      | Hostname → IP                       |
| MX     | Which server receives email         |
| SPF    | Which servers can send email        |
| DKIM   | Verify signature/integrity          |
| DMARC  | Action if SPF/DKIM fail             |
| CNAME  | Hostname → Another hostname (alias) |

The biggest confusion you have right now is mixing **MX** and **A** records. Once you remember:

> **MX = Find the mail server**
> **A = Find the IP of that server**

---
---


Brooo 🔥 that's actually a very good observation.

At first glance, yes, both seem to do:

```text
Something
   ↓
Hostname
```

So they look similar.

But their **purpose is completely different.**

---

# CNAME

CNAME means:

```text
One Hostname
      ↓
Another Hostname
```

Example:

```text
www.shivamtech.com
         ↓
       CNAME
         ↓
shivamapp.azurewebsites.net
```

DNS is saying:

> "[www.shivamtech.com](http://www.shivamtech.com) is just an alias of shivamapp.azurewebsites.net."

There is no concept of email here.

It's just a nickname.

---

# MX

MX means:

```text
For this domain,
which mail server should receive emails?
```

Example:

```text
shivamtech.com
       ↓
      MX
       ↓
mail.shivamtech.com
```

DNS is saying:

> "If someone sends an email to @shivamtech.com, deliver it to mail.shivamtech.com."

---

# Real Analogy

Imagine your company.

### CNAME

```text
Reception
    ↓
Nickname
    ↓
Real Department
```

Example:

```text
Help Desk
    ↓
IT Department
```

A nickname pointing to another name.

---

### MX

```text
Company
    ↓
Mail Room
```

Example:

```text
ShivamTech
     ↓
Mail Room = Building B
```

MX tells:

> "Where should letters be delivered?"

Not:

> "What is this name an alias for?"

---

# Technical Difference

## CNAME

Used during normal DNS resolution.

```text
portal.company.com
        ↓
      CNAME
        ↓
app.azurewebsites.net
```

Browser follows it.

---

## MX

Used only by email systems.

When Gmail wants to send mail to:

```text
hr@company.com
```

it asks:

```text
What's the MX record
for company.com?
```

DNS replies:

```text
mail.company.com
```

Then Gmail connects there.

---

# Why Can't MX Be Replaced by CNAME?

Imagine Gmail asks:

```text
Where do I deliver
mail for company.com?
```

If DNS replied:

```text
portal.company.com
```

Gmail wouldn't know:

> Is this a website?
>
> A VPN?
>
> An API?
>
> A mail server?

MX specifically means:

```text
THIS HOST RECEIVES EMAIL
```

---

# The Easy Way To Remember

### CNAME

Answers:

> "What is the real name of this host?"

```text
www.company.com
      ↓
app.azurewebsites.net
```

---

### MX

Answers:

> "Where should emails for this domain go?"

```text
company.com
      ↓
mail.company.com
```

---

So your observation is correct:

```text
Both ultimately point to hostnames.
```

But they answer different questions:

| Record | Question Answered                            |
| ------ | -------------------------------------------- |
| CNAME  | "What is the real hostname?"                 |
| MX     | "Which host receives email for this domain?" |

That's why both exist even though they both contain hostnames. 🚀

A SOC interviewer would actually be impressed if you asked exactly the question you just asked, because it shows you're thinking about *why* the DNS records exist rather than just memorizing them.


