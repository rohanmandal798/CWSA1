# cPanel University: Mail Administration Notes

## 1. Introduction & Mail Server Overview

**Course:** cPanel & WHM System Administration  
**Topic:** Administering Mail in the Command Line  
**Focus:** Administration and troubleshooting

---

# Part A. Course Notes

## 1. cPanel Mail Server Architecture

cPanel uses several components for email services:

|Component|Role|Protocol / Function|
|---|---|---|
|**Exim**|Mail Transfer Agent|SMTP|
|**Dovecot**|Mailbox access|IMAP / POP3|
|**Mailman**|Mailing lists|Email distribution|
|**SpamAssassin**|Spam detection|Spam filtering|

### Basic architecture

```
                    cPanel Mail Server
                           |
          ┌────────────────┼────────────────┐
          │                │                │
        Exim            Dovecot        SpamAssassin
          │                │                │
        SMTP          IMAP / POP3      Spam Detection
          │                │
          │                ↓
          │             Mailbox
          │
          ↓
      Mail Transfer

                    Mailman
                       ↓
                Mailing Lists
```

---

# 2. Exim

**Exim = Mail Transfer Agent (MTA)**

Its main responsibility is to **send, receive and transfer email** using SMTP.

Example:

```
Sender
  ↓
SMTP
  ↓
Exim
  ↓
Recipient Mail Server
```

cPanel installs Exim as an RPM and manages its configuration through cPanel's configuration system.

### Important

Do not casually modify Exim configuration files manually on a cPanel server.

Use:

- WHM
- cPanel-supported configuration
- cPanel scripts/tools

### Remember

```
Exim = Mail transfer
SMTP = Protocol used for mail transfer
```

---

# 3. Dovecot

**Dovecot = IMAP/POP3 server**

It allows users to access their stored mail.

```
Mailbox
   ↓
Dovecot
   ↓
IMAP / POP3
   ↓
Mail Client
```

Examples of mail clients:

- Outlook
- Thunderbird
- Mobile mail applications

### Remember

```
Exim    → Moves mail
Dovecot → Provides mailbox access
```

---

# 4. Mailman

**Mailman = Mailing list management**

Example:

```
security@example.com
        ↓
     Mailman
    /   |   \
   ↓    ↓    ↓
User1 User2 User3
```

One message can be distributed to multiple subscribers.

---

# 5. SpamAssassin

**SpamAssassin = Spam detection system**

It analyzes incoming messages and assigns spam scores.

Conceptually:

```
Incoming Mail
      ↓
SpamAssassin
      ↓
Analyze Message
      ↓
Spam Score
      ↓
Spam / Legitimate
```

SpamAssassin can also have **per-user configuration** through cPanel.

---

# 6. SMTP, IMAP and POP3

### SMTP

Used for sending and transferring email.

```
SMTP
 ↓
Send / Transfer mail
```

### IMAP

Used to access and synchronize mail stored on the server.

```
IMAP
 ↓
Access + Synchronize mailbox
```

### POP3

Used to retrieve/download email.

```
POP3
 ↓
Retrieve mail
```

### Easy memory trick

```
SMTP → Send
IMAP → Access + Sync
POP3 → Download
```

---

# 7. Important Mail Ports

|Port|Service|Purpose|
|---|---|---|
|**25**|SMTP|Server-to-server mail delivery|
|**26**|SMTP|Alternate SMTP port|
|**110**|POP3|Mail retrieval|
|**143**|IMAP|Mail access|
|**465**|SMTP over TLS|Secure mail submission|
|**587**|SMTP Submission|Authenticated outgoing mail|
|**993**|IMAPS|Secure IMAP|
|**995**|POP3S|Secure POP3|
|**2095**|Webmail|cPanel webmail|
|**2096**|Secure Webmail|HTTPS cPanel webmail|

### Important distinction

```
25
→ Server-to-server SMTP

587
→ SMTP submission, commonly STARTTLS

465
→ SMTP submission over implicit TLS
```

---

# 8. Checking Exim Activity

Before troubleshooting, check what Exim is doing **right now**.

## `exiwhat`

```
exiwhat
```

Shows active Exim connections.

Think:

```
exiwhat
   ↓
"What is Exim doing right now?"
```

Useful for:

- Spam investigation
- High mail traffic
- Active SMTP connections
- Suspicious activity

---

## `ps -C exim wwwu`

```
ps -C exim wwwu
```

Shows running Exim processes.

Useful for checking:

- Number of processes
- CPU usage
- Memory usage
- Process IDs

---

## `lsof -c exim`

```
lsof -c exim
```

Shows files, sockets and other resources currently opened by Exim.

### Remember

```
exiwhat
→ Connections

ps
→ Processes

lsof
→ Open files + sockets
```

---

# 9. Exim Queue

The Exim queue contains messages waiting for delivery.

```
Message
   ↓
Exim
   ↓
Queue
   ↓
Destination Server
```

Common reasons for queued mail:

- Destination server unavailable
- DNS problem
- Network problem
- Connection timeout
- Temporary SMTP rejection
- Rate limiting

### Important commands

List queued messages:

```
exim -bp
```

Count queued messages:

```
exim -bpc
```

View message headers:

```
exim -Mvh MESSAGE_ID
```

View message body:

```
exim -Mvb MESSAGE_ID
```

View message delivery log:

```
exim -Mvl MESSAGE_ID
```

Attempt delivery:

```
exim -M MESSAGE_ID
```

Be careful with queue deletion commands because they can permanently remove messages.

---

# 10. Testing IMAP

Dovecot provides IMAP.

The first step is to test whether Dovecot is actually reachable and responding.

## Plain IMAP

```
telnet mail.example.com 143
```

Successful connection may show:

```
* OK [CAPABILITY IMAP4rev1 ...] Dovecot ready.
```

This confirms:

```
Network       ✓
Port 143      ✓
Dovecot       ✓
IMAP          ✓
```

---

# 11. Testing Secure IMAP

Use OpenSSL for encrypted testing.

## IMAPS

```
openssl s_client -connect mail.example.com:993
```

## IMAP STARTTLS

```
openssl s_client \
  -connect mail.example.com:143 \
  -starttls imap
```

### With SNI

```
openssl s_client \
  -connect 192.0.2.10:993 \
  -servername mail.example.com
```

OpenSSL allows you to inspect:

- TLS handshake
- Certificate
- Certificate expiry
- Hostname
- TLS version
- Cipher
- Verification result

---

# 12. IMAP Commands

Once connected to IMAP, commands can be sent manually.

### Login

```
a001 LOGIN username password
```

### List mailboxes

```
a002 LIST "" "*"
```

### Select Inbox

```
a003 SELECT INBOX
```

### Logout

```
a004 LOGOUT
```

The `a001`, `a002`, etc. are **command tags** used to match requests with responses.

### Security warning

Don't send real production passwords over Telnet because Telnet is plaintext.

For secure testing, use TLS.

---

# 13. Finding Mail Logs

## cPanel

### Exim main log

```
/var/log/exim_mainlog
```

Contains information about:

- Incoming mail
- Outgoing mail
- Delivery
- Failures
- Message IDs
- Mail processing

Watch in real time:

```
tail -f /var/log/exim_mainlog
```

---

### Dovecot + SpamAssassin

```
/var/log/maillog
```

Useful for:

- IMAP
- POP3
- Authentication
- Mailbox access
- SpamAssassin activity

Example:

```
tail -f /var/log/maillog
```

---

### Mailman

```
/usr/local/cpanel/3rdparty/mailman/logs/*
```

Used for troubleshooting mailing-list activity.

---

## Ubuntu difference

On Ubuntu, Dovecot commonly uses:

```
/var/log/mail.log
```

So don't blindly assume the same log path on every Linux distribution.

---

# 14. Basic Log Commands

### Follow logs

```
tail -f /var/log/exim_mainlog
```

```
tail -f /var/log/maillog
```

### Search user

```
grep "user@example.com" /var/log/exim_mainlog
```

### Search errors

```
grep -i "error" /var/log/maillog
```

### Search failed authentication

```
grep -i "failed" /var/log/maillog
```

These commands are fundamental for server support.

---

# 15. Firewall Troubleshooting

A mail service can be running but still unreachable.

```
Internet
   ↓
External Firewall
   ↓
Server Firewall
   ↓
Dovecot / Exim
```

Check whether a port is listening:

```
ss -lntp | grep ':25'
```

Test remotely:

```
nc -vz mail.example.com 25
```

For IMAP:

```
nc -vz mail.example.com 143
```

### Important distinction

**Connection refused**

```
Host reachable
      ↓
Connection rejected
```

Possible causes:

- Service not listening
- Firewall rejection
- Wrong port

**Connection timeout**

```
No response
```

Possible causes:

- Firewall dropping traffic
- Routing problem
- Network issue
- Host unreachable

---

# 16. Mail Troubleshooting Workflow

This is probably the **most important thing learned today**.

Don't randomly restart services.

Use:

```
Customer reports mail problem
          ↓
Identify exact symptom
          ↓
Determine sending / receiving / login
          ↓
Check DNS / MX
          ↓
Check connectivity
          ↓
Check correct port
          ↓
Identify component
          ↓
Check logs
          ↓
Find message ID / error
          ↓
Check Exim queue
          ↓
Identify root cause
          ↓
Fix
          ↓
Verify
          ↓
Document
```

---

# Part B. Interview Notes

## 1. What is Exim?

**Answer:**

> Exim is the Mail Transfer Agent used by cPanel to send, receive and transfer email using SMTP.

---

## 2. What is Dovecot?

> Dovecot provides IMAP and POP3 services that allow users to access their mailboxes.

---

## 3. Exim vs Dovecot?

> Exim handles mail transfer, while Dovecot handles mailbox access through IMAP and POP3.

---

## 4. What is Mailman?

> Mailman is used to manage email mailing lists and distribute messages to subscribers.

---

## 5. What is SpamAssassin?

> SpamAssassin analyzes email messages and assigns spam scores to help identify unwanted mail.

---

## 6. What is SMTP?

> SMTP is the protocol used for sending and transferring email.

---

## 7. What is IMAP?

> IMAP allows users to access and synchronize email stored on the mail server.

---

## 8. What is POP3?

> POP3 is a protocol used to retrieve email from a mail server, traditionally by downloading messages.

---

## 9. What is port 25?

> Port 25 is primarily used for SMTP server-to-server mail delivery.

---

## 10. What is port 587?

> Port 587 is the standard SMTP submission port used by authenticated mail clients, commonly with STARTTLS.

---

## 11. What is port 465?

> Port 465 is commonly used for SMTP submission over implicit TLS.

---

## 12. What is port 993?

> Port 993 is IMAP over TLS.

---

## 13. What is port 995?

> Port 995 is POP3 over TLS.

---

## 14. How do you check active Exim connections?

```
exiwhat
```

---

## 15. How do you check Exim processes?

```
ps -C exim wwwu
```

---

## 16. How do you see resources opened by Exim?

```
lsof -c exim
```

---

## 17. How do you check the Exim queue?

```
exim -bp
```

---

## 18. How do you count queued messages?

```
exim -bpc
```

---

## 19. How do you view a message's headers?

```
exim -Mvh MESSAGE_ID
```

---

## 20. How do you test IMAP?

```
telnet mail.example.com 143
```

For secure IMAP:

```
openssl s_client -connect mail.example.com:993
```

---

## 21. How do you test IMAP STARTTLS?

```
openssl s_client \
  -connect mail.example.com:143 \
  -starttls imap
```

---

## 22. Where is the Exim main log?

```
/var/log/exim_mainlog
```

---

## 23. Where is the Dovecot log on cPanel?

```
/var/log/maillog
```

---

## 24. Where is the Dovecot log commonly found on Ubuntu?

```
/var/log/mail.log
```

---

## 25. Where are Mailman logs?

```
/usr/local/cpanel/3rdparty/mailman/logs/*
```

---

# Scenario-Based Interview Questions

These are **more important than simple definitions** for your target role.

## Scenario 1: Customer cannot receive email

Your approach:

```
1. Confirm recipient address
2. Check MX records
3. Check port 25
4. Check firewall
5. Check Exim
6. Search exim_mainlog
7. Check reject logs
8. Check spam filtering
9. Check queue
10. Verify delivery
```

---

## Scenario 2: Customer cannot send email

Check:

```
SMTP port
    ↓
465 / 587
    ↓
Firewall
    ↓
Exim
    ↓
Authentication
    ↓
Mail queue
    ↓
Remote destination
```

---

## Scenario 3: Customer cannot log into email

Think:

```
Dovecot
   ↓
IMAP/POP3
   ↓
Authentication
```

Check:

```
tail -f /var/log/maillog
```

Then investigate:

- Username
- Password
- Authentication
- Account status
- Firewall
- Port
- Dovecot

---

## Scenario 4: Thousands of emails are being sent

Possible causes:

- Compromised account
- Compromised WordPress
- Malware
- Stolen credentials
- PHP mail abuse
- Spam script

Start with:

```
exiwhat
```

Then:

```
ps -C exim wwwu
```

Then:

```
exim -bp
```

And investigate:

```
/var/log/exim_mainlog
```

---

## Scenario 5: IMAP connection times out

Check:

```
DNS
 ↓
IP
 ↓
Firewall
 ↓
Port 143/993
 ↓
Dovecot
```

Test:

```
nc -vz mail.example.com 993
```

Then:

```
openssl s_client -connect mail.example.com:993
```

---

# Must-Know Command Cheat Sheet

```
# Exim activity
exiwhat

# Exim processes
ps -C exim wwwu

# Exim open resources
lsof -c exim

# Exim queue
exim -bp

# Queue count
exim -bpc

# Message headers
exim -Mvh MESSAGE_ID

# Message body
exim -Mvb MESSAGE_ID

# Message delivery log
exim -Mvl MESSAGE_ID

# Exim log
tail -f /var/log/exim_mainlog

# Mail/Dovecot log
tail -f /var/log/maillog

# Test IMAP
telnet mail.example.com 143

# Test secure IMAP
openssl s_client -connect mail.example.com:993

# Test IMAP STARTTLS
openssl s_client -connect mail.example.com:143 -starttls imap

# Check listening ports
ss -lntp

# Test TCP connectivity
nc -vz mail.example.com 25
nc -vz mail.example.com 993
```

---

# Final Mental Model

```
                    EMAIL INFRASTRUCTURE

                         Internet
                            |
                    ┌───────┴───────┐
                    ↓               ↓
                SMTP :25       Mail Client
                    ↓          /          \
                  Exim       465/587      993/995
                    ↓            ↓          ↓
                 Mailbox      SMTP       Dovecot
                                 submission
                                  /       \
                               IMAP       POP3
```

### Logs

```
Exim
 ↓
/var/log/exim_mainlog

Dovecot / SpamAssassin
 ↓
/var/log/maillog

Mailman
 ↓
/usr/local/cpanel/3rdparty/mailman/logs/*
```

### The one principle to remember

> **Identify the component first, then check its connectivity, logs, processes and configuration.**

That troubleshooting approach is going to carry over directly to **LiteSpeed, CloudLinux, MySQL, DNS, JetBackup, Imunify360 and the rest of the job.**