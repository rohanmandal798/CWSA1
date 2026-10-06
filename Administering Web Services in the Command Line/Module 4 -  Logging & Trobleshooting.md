**Lesson - 1 : Introduction & Quick Reference
Lesson - 2 : Reading the Access Logs
Lesson - 3 : Reading the Error Logs**


![[Pasted image 20261006155044.png]]

![[Pasted image 20261006155112.png]]


# Lesson 1: Introduction & Quick Reference

This lesson is mainly about **Apache logs + HTTP status codes**. For interview and practical troubleshooting, these are the points to remember.

## 1. Apache Log Files

|Log|Location|What it contains|When to check|
|---|---|---|---|
|**Apache Error**|`/etc/apache2/logs/error_log`|Apache errors except 404|Website not working as expected|
|**Apache Access**|`/etc/apache2/logs/access_log`|Requests to default virtual hosts and IP addresses|Checking incoming requests|
|**suPHP**|`/etc/apache2/logs/suphp_log`|suPHP execution logs and errors|PHP site using suPHP has issues|
|**Domain Access**|`/etc/apache2/logs/domlogs/$DOMAIN`|Requests going through a specific virtual host|Check whether requests reach the correct domain/site|
|**EasyApache**|`/var/log/yum.log`|RPM installations, removals, updates|Check when packages/EasyApache components changed|

### Important commands

Read the latest Apache errors:

```
tail -f /etc/apache2/logs/error_log
```

Search for errors:

```
grep -i "error" /etc/apache2/logs/error_log
```

Check a domain's access log:

```
tail -f /etc/apache2/logs/domlogs/example.com
```

Check package changes:

```
grep -i "easyapache\|httpd\|apache" /var/log/yum.log
```

---

# 2. HTTP Status Codes

|Code|Meaning|Simple explanation|
|---|---|---|
|**200**|OK|Request succeeded|
|**401**|Unauthorized|Authentication/password required|
|**403**|Forbidden|Server refuses access|
|**404**|Not Found|Requested file/page cannot be found|
|**500**|Internal Server Error|Server-side problem, check logs|

### Easy way to remember

```
200 → Everything OK
401 → Login required
403 → Access denied
404 → Page/file missing
500 → Server-side error
```

---

## 3. Troubleshooting Flow

If a website is not working:

```
Website problem
      ↓
Check HTTP status code
      ↓
Check Apache access log
      ↓
Check Apache error log
      ↓
Check domain-specific log
      ↓
Check permissions/config/PHP
      ↓
Fix the issue
      ↓
Test website again
```

### Interview example

**Q: A customer reports that their website is showing HTTP 500. What will you do?**

**Answer:**

> First, I will confirm the HTTP status code. Then I will check the Apache error log and the domain-specific log to identify the exact server-side error. I will check PHP/application errors, file permissions, `.htaccess`, and Apache configuration. After fixing the root cause, I will test the website again.

**Key point:**  
For **500 errors**, don't guess. **Check the logs first.**


---

# Lesson 2: Reading the Access Logs

This lesson is important for **cPanel/Linux web hosting troubleshooting**.

![[Pasted image 20261006155557.png]]
## 1. What is an Apache Access Log?

The **access log** records requests made to the web server.

It helps you identify:

- **Who** made the request
- **When** the request happened
- **What URL** was requested
- **HTTP status code**
- **Response size**
- **Referrer**
- **Browser/client information**

### cPanel locations

```
Main Apache access log:
/etc/apache2/logs/access_log

Domain-specific logs:
/etc/apache2/logs/domlogs/
```

For example:

```
/etc/apache2/logs/domlogs/example.com
```

---

# 2. Understanding an Access Log Entry

Example:

```
10.7.65.27 - - [26/Jan/2016:12:17:42 -0600] "GET / HTTP/1.1" 200 43 "-" "Mozilla/5.0 ..."
```

Break it down:

|Part|Example|Meaning|
|---|---|---|
|1|`10.7.65.27`|Client IP address|
|2|`- -`|Identd and authenticated username|
|3|`[26/Jan/2016:12:17:42 -0600]`|Date, time and timezone|
|4|`"GET / HTTP/1.1"`|HTTP request|
|5|`200`|HTTP response status|
|6|`43`|Response size in bytes|
|7|`"-"`|Referrer|
|8|`"Mozilla/5.0..."`|User-Agent/browser|

---

## 3. Important Parts

### Client IP

```
10.7.65.27
```

This tells you where the request originated.

---

### Request

```
"GET / HTTP/1.1"
```

Means:

- `GET` = HTTP method
- `/` = root/home page
- `HTTP/1.1` = HTTP protocol version

Example:

```
"GET /login.php HTTP/1.1"
```

The client requested `/login.php`.

---

### Status Code

```
200
```

The request was successful.

You may also see:

```
404
403
500
```

For example:

```
192.168.1.10 - - [...] "GET /admin HTTP/1.1" 404 512
```

This means the requested `/admin` resource returned **404 Not Found**.

---

### Response Size

```
43
```

The server returned **43 bytes** of response body.

---

### Referrer

```
"-"
```

A `-` means no referrer was provided.

If you see:

```
"https://google.com/"
```

the request came through a link/referral from Google.

---

### User-Agent

```
"Mozilla/5.0 ... Chrome/47..."
```

This identifies the client software/browser.

It can help identify:

- Chrome
- Firefox
- Safari
- Bots
- Crawlers
- Scripts/tools

---

# 4. Main Access Log vs Domain Log

### Main access log

```
/etc/apache2/logs/access_log
```

Contains requests that go to the **default virtual host**, such as requests to:

- Server's main IP
- Server hostname
- Default virtual host

It also contains **cPanel service-check requests** used to verify that Apache is running.

### Domain logs

```
/etc/apache2/logs/domlogs/
```

These are **per-domain/virtual-host logs**.

Example:

```
tail -f /etc/apache2/logs/domlogs/example.com
```

Use these when troubleshooting a **specific website**.

---

# 5. 404 Errors in Access Logs

Access logs also contain **404 requests**.

Example:

```
"GET /missing.html HTTP/1.1" 404
```

Don't immediately assume this is a server problem.

404s can happen because:

- User entered a wrong URL
- Old link exists
- Website has a broken link
- Bot scanned a random path
- Requested file was removed

Usually, **404 entries are more useful to the web developer than the system administrator**, unless there is an unusual pattern.

---

# 6. Useful Commands

Watch the access log live:

```
tail -f /etc/apache2/logs/access_log
```

Watch a specific domain:

```
tail -f /etc/apache2/logs/domlogs/example.com
```

Find 404 requests:

```
grep " 404 " /etc/apache2/logs/access_log
```

Find 500 errors:

```
grep " 500 " /etc/apache2/logs/access_log
```

Count requests by IP:

```
awk '{print $1}' /etc/apache2/logs/access_log | sort | uniq -c | sort -nr
```

This is especially useful when checking for **unusual traffic or possible DoS activity**.

---

## Interview Question

**Q: What information can you get from an Apache access log?**

**Answer:**

> An Apache access log shows the client IP, request time, HTTP method and URL, HTTP status code, response size, referrer, and User-Agent. In cPanel, domain-specific access logs are stored under `/etc/apache2/logs/domlogs/`.

### Remember this structure

```
IP
 ↓
Identity
 ↓
Date/Time
 ↓
Request
 ↓
Status
 ↓
Size
 ↓
Referrer
 ↓
User-Agent
```

**Access log = "Who requested what, when, and what did Apache return?"**


---

![[Pasted image 20261006155724.png]]

# Lesson 3: Reading the Error Logs

This is the **most important log for troubleshooting Apache server-side problems**.

## 1. What is the Apache Error Log?

The Apache error log records **unsuccessful responses other than 404 errors**.

### Location

```
/etc/apache2/logs/error_log
```

Use it when:

- Website shows **500 Internal Server Error**
- PHP/CGI script fails
- Apache configuration has a problem
- Permission-related errors occur
- A module reports an error
- Application/script crashes

---

# 2. Understanding the Example

Example:

```
[Tue Jan 26 12:31:29.364984 2016]
[core:error]
[PID 18924]
[client 10.7.65.184:57950]
End of script output before headers: info.php
```

Let's break it down.

|Part|Example|Meaning|
|---|---|---|
|**1. Date/time**|`Tue Jan 26 12:31:29.364984 2016`|When the error occurred|
|**2. Module/severity**|`[core:error]`|Apache module + severity|
|**3. PID**|`18924`|Process ID handling the request|
|**4. Client**|`10.7.65.184:57950`|Client IP + source port|
|**5. Message**|`End of script output before headers: info.php`|Actual error information|

---

# 3. Apache Error Severity Levels

Apache has **16 severity levels**, from most severe to least severe:

```
emerg
alert
crit
error
warn
notice
info
debug
trace1
trace2
trace3
trace4
trace5
trace6
trace7
trace8
```

### Easy way to remember the important ones

|Level|Meaning|
|---|---|
|`emerg`|Emergency|
|`alert`|Immediate action needed|
|`crit`|Critical problem|
|`error`|Error occurred|
|`warn`|Warning|
|`notice`|Normal but notable|
|`info`|Informational|
|`debug`|Debugging information|

For normal troubleshooting, you'll commonly focus on:

```
error
warn
crit
alert
emerg
```

---

# 4. Understanding the Actual Error

The important part is:

```
End of script output before headers: info.php
```

This usually means Apache expected the PHP/CGI script to produce valid HTTP headers, but the script ended before producing them.

Possible causes include:

- PHP/CGI script error
- Incorrect script output
- Permission problems
- Incorrect PHP/CGI configuration
- Application failure

So the next step would be to investigate `info.php` and the relevant PHP/application logs.

---

# 5. Useful Commands

Watch Apache errors live:

```
tail -f /etc/apache2/logs/error_log
```

Show the last 50 errors:

```
tail -n 50 /etc/apache2/logs/error_log
```

Search for PHP-related errors:

```
grep -i "php" /etc/apache2/logs/error_log
```

Search for a specific client IP:

```
grep "10.7.65.184" /etc/apache2/logs/error_log
```

Search for a specific script:

```
grep "info.php" /etc/apache2/logs/error_log
```

---

# 6. Access Log vs Error Log

This distinction is **very important for your interview**.

|Access Log|Error Log|
|---|---|
|Records requests|Records errors/problems|
|Shows who requested something|Shows why something failed|
|Contains 404 entries|Generally excludes 404|
|Shows status code|Gives detailed error information|
|`/etc/apache2/logs/access_log`|`/etc/apache2/logs/error_log`|

### Simple memory trick

```
ACCESS = What happened?

ERROR = Why did it fail?
```

---

## Interview Question

**Q: A customer says their website is showing HTTP 500. What log will you check first?**

**Answer:**

> I would check the Apache error log at `/etc/apache2/logs/error_log`. I would look for errors around the time of the customer's request, identify the affected script or configuration, and then investigate the root cause.

### Most important paths from this module

```
/etc/apache2/logs/access_log
/etc/apache2/logs/error_log
/etc/apache2/logs/domlogs/
```

**Access log tells you what requests are happening. Error log tells you what went wrong.**