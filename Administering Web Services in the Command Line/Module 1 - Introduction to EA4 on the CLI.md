**Lesson - 1 : Introduction & Preparation
Lesson - 2 : Looking into Apache
Lesson - 3 : Looking into Package Managers
Lesson - 4 : Looking into PHP**

## Lesson 1: Introduction to EA4 on the CLI

### 1. What is EasyApache 4?

**EasyApache 4 (EA4)** is cPanel's system for managing:

- Apache
- PHP
- PHP versions
- Apache modules
- PHP extensions
- MPMs
- Related web-server packages

The important point is:

> **EA4 does not provide one single CLI command that handles everything interactively.**

Instead, you use normal Linux administration tools.

For example:

```
yum install package-name
```

or:

```
yum remove package-name
```

or:

```
yum update package-name
```

So, EA4 CLI administration is basically:

```
EasyApache 4
     |
     +---- Apache
     |
     +---- PHP
     |
     +---- Modules
     |
     +---- MPM
     |
     v
Linux Package Manager
     |
     v
yum / RPM
```

---

### 2. Why is this important?

Suppose you need to install an Apache module.

Instead of using WHM:

```
WHM
  ↓
EasyApache 4
  ↓
Customize
  ↓
Apache Modules
  ↓
Select Module
  ↓
Review
  ↓
Provision
```

You can perform package management from CLI.

For example:

```
yum install ea-apache24-mod_security2
```

The exact package name depends on what you're installing.

This is useful when:

- Managing multiple servers
- Automating server configuration
- Troubleshooting
- Working through SSH
- Writing deployment scripts
- Managing servers without using WHM

---

### 3. What is `yum`?

`yum` is a **package manager** commonly used on RPM-based Linux systems.

It can:

- Install packages
- Remove packages
- Update packages
- Search packages
- Display package information
- List installed/available packages
- Manage repositories

Basic syntax:

```
yum <command> <package>
```

Examples:

```
yum install package
```

```
yum remove package
```

```
yum update package
```

```
yum info package
```

```
yum list package
```

---

### 4. EA4 Package Naming

EA4 packages commonly use prefixes such as:

```
ea-apache24-
ea-phpXX-
ea-phpXX-php-
```

For example:

```
ea-apache24
```

means the EA4 Apache package.

A PHP version could look like:

```
ea-php82
```

And a PHP extension might look like:

```
ea-php82-php-mysqlnd
```

The `XX` represents the PHP version.

For example:

```
ea-php81
ea-php82
ea-php83
```

---

### 5. Install a Package

General syntax:

```
yum install package-name
```

Example:

```
yum install ea-apache24
```

You may see something like:

```
Dependencies resolved.

================================================================================
 Package             Architecture       Version
================================================================================
Installing:
 ea-apache24         x86_64             ...

Transaction Summary
================================================================================
Install ...

Is this ok [y/N]:
```

Enter:

```
y
```

---

### 6. Remove a Package

```
yum remove package-name
```

Example:

```
yum remove ea-apache24
```

Be careful here.

Removing an Apache package can affect websites and dependent packages.

Always understand dependencies before confirming.

---

### 7. Update Packages

Update a specific package:

```
yum update package-name
```

For example:

```
yum update ea-apache24
```

Update all available packages:

```
yum update
```

On a production cPanel server, don't blindly run:

```
yum update
```

You should understand what packages will change and whether the update can affect production services.

---

### 8. `yum list`

One of the objectives specifically mentions:

> Describe the output of `yum list`.

Basic:

```
yum list
```

This lists packages and their status.

More useful:

```
yum list installed
```

Shows installed packages.

```
yum list available
```

Shows packages available from configured repositories.

You can also search for EA4 packages:

```
yum list available | grep ea-
```

Example output could look like:

```
ea-apache24.x86_64
ea-apache24-mod_ssl.x86_64
ea-php82.x86_64
ea-php83.x86_64
```

---

### 9. `yum info`

`yum info` gives detailed information about a package.

Example:

```
yum info ea-apache24
```

You may see:

```
Name        : ea-apache24
Arch        : x86_64
Version     : ...
Release     : ...
Size        : ...
Repo        : ...
Summary     : ...
Description : ...
```

### Remember the difference

|Command|Purpose|
|---|---|
|`yum list`|Shows package availability/status|
|`yum info`|Shows detailed package information|

---

### 10. What is an MPM?

The lesson also mentions:

> Swap out MPMs

**MPM = Multi-Processing Module**

Apache MPM controls how Apache handles requests and processes/connections.

Common Apache MPMs include:

```
prefork
worker
event
```

Very simplified:

```
Client Requests
      |
      v
Apache
      |
      v
MPM
      |
      +---- Processes
      |
      +---- Threads
      |
      +---- Connections
```

In EA4, Apache MPM configuration is managed through EA4 packages/configuration.

---

### 11. Experimental Repository

Another course objective is:

> Enable the experimental repository for EasyApache 4.

Repositories are sources from which packages are downloaded.

You can inspect repositories with:

```
yum repolist
```

This shows enabled repositories.

For example:

```
repo id
repo name
status
```

The exact repository-management commands can depend on the cPanel/OS version, so when we reach that section, we'll use the course's exact EA4 procedure rather than guessing.

---

# 12. The Big Picture

You should understand this workflow:

```
                cPanel / WHM
                     |
                     v
              EasyApache 4
                     |
          +----------+----------+
          |          |          |
        Apache      PHP       Modules
          |          |          |
          +----------+----------+
                     |
                     v
              RPM Packages
                     |
                     v
                 yum/dnf
                     |
                     v
              Linux OS
```

So when someone says:

**"Manage Apache through EA4 CLI."**

Think:

```
EA4
 ↓
EA4 packages
 ↓
yum/RPM
 ↓
Apache/PHP configuration
```

---

### 13. Commands We Should Practice

On an actual **cPanel/EA4 server**, practice:

```
yum repolist
```

```
yum list installed
```

```
yum list available
```

```
yum info ea-apache24
```

```
yum list available | grep ea-apache
```

```
yum list available | grep ea-php
```

Also check:

```
httpd -v
```

and:

```
php -v
```

These help you understand what Apache and PHP are actually installed.

---

# Lesson 1: Understanding the Terminology

### Quick understanding

|Term|Simple meaning|Example|
|---|---|---|
|**Wildcard**|Pattern matching character|`*.log`|
|**String**|Text/characters treated literally|`"apache24"`|
|**Regular Expression**|Pattern used to find/match text|`^ea-php`|
|**RPM**|Package manager, package format, or package itself|`ea-apache24` RPM|
|**Thread**|Execution unit inside a process|Apache worker thread|
|**Repository**|Server/location containing packages|EA4 repository|

### 1. Wildcard

`*` means **zero or more characters**.

Example:

```
ls *.log
```

This can match:

```
error.log
access.log
apache.log
```

Think:

```
*.log
 ^
anything
```

---

### 2. String

A **string** is simply a sequence of characters treated as text.

Examples:

```
apache
ea-apache24
PHP 8.3
/var/log/httpd/error_log
```

In scripting:

```
name="apache"
```

Here, `"apache"` is a string.

---

### 3. Regular Expression

A **regular expression**, or regex, is a pattern used to search or match text.

For example:

```
^ea-php
```

means text beginning with `ea-php`.

You may use it with commands such as:

```
yum list installed | grep '^ea-php'
```

This is particularly useful when managing many EA4 packages.

**Important:** Don't confuse wildcard `*` with regex `*`. Their behavior depends on the context.

---

### 4. RPM

RPM stands for:

> **RPM Package Manager**

It can refer to:

1. The package management system.
2. The `.rpm` package file format.
3. A package distributed in RPM format.

EA4 uses RPM packages.

Example:

```
ea-apache24
ea-php83
ea-php83-php-mysqlnd
```

---

### 5. Thread

A **thread** is an execution unit inside a process.

A process can have multiple threads sharing resources such as memory.

```
Process
├── Thread 1
├── Thread 2
├── Thread 3
└── Thread 4
```

This becomes important when discussing Apache MPMs such as:

```
prefork
worker
event
```

`prefork` is process-based, while `worker` and `event` use threaded designs.

---

### 6. Repository

A **repository** is a source containing software packages.

Think of it like a software warehouse:

```
EA4 Repository
       |
       +-- Apache packages
       +-- PHP packages
       +-- PHP extensions
       +-- Apache modules
```

Your server uses the repository to find and download packages.

Useful command:

```
yum repolist
```

---
# Lesson 2: Looking into Apache

## 1. What is Apache?

**Apache HTTP Server** is an open-source web server maintained by the **Apache Software Foundation**.

In simple terms:

> Apache receives HTTP requests from clients and returns the requested web content.

Example:

```
Browser
   |
   | HTTP/HTTPS request
   v
Apache Web Server
   |
   | Find requested resource
   v
Website files / Application
   |
   v
HTTP Response
   |
   v
Browser
```

Example:

You visit:

```
https://example.com/index.html
```

Apache receives the request and looks for the requested resource, then sends the response back to your browser.

---

# 2. What is a Web Server?

A web server is software that:

1. Accepts requests from clients.
2. Processes the request.
3. Finds or generates the requested content.
4. Returns an HTTP response.

Common clients:

- Chrome
- Firefox
- Edge
- `curl`
- Other applications

Example using CLI:

```
curl -I https://example.com
```

The request goes to the web server.

The server might respond:

```
HTTP/2 200
content-type: text/html
```

---

# 3. Apache Uses Modules

One of the most important points from this lesson:

> **Apache uses modules to extend its functionality.**

Think of Apache as a base system:

```
Apache
   |
   +-- Core functionality
   |
   +-- SSL module
   |
   +-- Rewrite module
   |
   +-- PHP integration
   |
   +-- Security modules
   |
   +-- Other modules
```

Instead of putting every feature directly into Apache, functionality can be added through modules.

---

# 4. Apache Modules in EasyApache 4

This is where **EA4** becomes important.

On a cPanel server using EasyApache 4:

```
Apache Module
      |
      v
EA4 Package
      |
      v
RPM
      |
      v
Package Manager
      |
      v
yum
```

For example, an Apache module may be distributed as an EA4 RPM package.

You can search for EA4 Apache packages with:

```
yum list available | grep ea-apache
```

You may see packages such as:

```
ea-apache24
ea-apache24-mod_ssl
```

The exact available packages depend on the server and repositories.

---

# 5. What is CGI?

The lesson also introduces **CGI**.

CGI stands for:

> **Common Gateway Interface**

CGI allows Apache to communicate with an external program.

Basic flow:

```
Browser
   |
   | HTTP Request
   v
Apache
   |
   | CGI
   v
External Program
   |
   | Generated response
   v
Apache
   |
   v
Browser
```

For example, imagine a request:

```
/cgi-bin/test.cgi
```

Apache can pass that request to a CGI program.

The program generates output, and Apache sends that output back to the browser.

---

# 6. Apache Module vs CGI

These are different concepts.

|Apache Module|CGI|
|---|---|
|Extends Apache itself|Connects Apache to external programs|
|Runs as part of Apache architecture|Runs an external program|
|Often distributed as RPMs in EA4|Uses CGI interface|
|Example: SSL/rewrite functionality|Example: CGI script|

Simple memory trick:

```
Module = Apache functionality

CGI = Apache talks to external program
```

---

# 7. Why This Matters in cPanel

In a cPanel environment, you will frequently work with:

```
cPanel
   |
   v
EasyApache 4
   |
   +-- Apache
   |
   +-- Apache Modules
   |
   +-- PHP
   |
   +-- PHP Extensions
   |
   +-- MPM
```

As a Web Hosting Support Engineer, you may receive tickets such as:

> "My website is not loading."

> "I need SSL enabled."

> "URL rewriting isn't working."

> "I need a specific Apache module."

> "My PHP application isn't working."

You need to understand whether the problem is related to:

- Apache
- Apache module
- PHP
- PHP extension
- Configuration
- Permissions
- Application
- DNS
- Firewall

---

# 8. Important CLI Commands

### Check Apache version

On a typical Apache installation:

```
httpd -v
```

Example:

```
Server version: Apache/2.4.x
Server built: ...
```

### Check Apache configuration

```
httpd -t
```

Expected:

```
Syntax OK
```

### Check Apache service

On RHEL-based systems:

```
systemctl status httpd
```

Start:

```
systemctl start httpd
```

Restart:

```
systemctl restart httpd
```

Reload:

```
systemctl reload httpd
```

---

# 9. Check Apache Modules

You can use:

```
httpd -M
```

This displays loaded Apache modules.

For example, you might see:

```
rewrite_module
ssl_module
proxy_module
```

You can search:

```
httpd -M | grep rewrite
```

Or:

```
httpd -M | grep ssl
```

This is very useful during troubleshooting.

---

# 10. Apache + MPM

Apache uses an **MPM**, or Multi-Processing Module, to handle connections and request processing.

Common MPMs:

```
prefork
worker
event
```

You can check the active MPM with:

```
httpd -V | grep MPM
```

The exact output can vary depending on the Apache build.

Conceptually:

```
Apache
   |
   v
MPM
   |
   +-- Process/Thread handling
   |
   v
Client connections
```

---

### Quick Revision

```
Apache
  ↓
Web Server
  ↓
Accepts HTTP requests
  ↓
Processes requests
  ↓
Returns HTTP responses
```

### Remember these 5 points

```
1. Apache = Web Server

2. Apache functionality can be extended with modules

3. EA4 packages Apache modules as RPMs

4. yum is used to manage these packages

5. CGI allows Apache to communicate with external programs
```

### Commands to remember

```
httpd -v
httpd -t
httpd -M
httpd -V
systemctl status httpd
yum list available | grep ea-apache
```


---

# Lesson 2: cPanel, Apache, and EasyApache 4

## 1. cPanel Hosted Websites Use Apache

cPanel uses **Apache** to serve the websites hosted on the server.

The flow is:

```
Internet
   |
   v
Client Browser
   |
   | HTTP / HTTPS
   v
Apache
   |
   v
Hosted Website
```

So if Apache is down, the **hosted websites** can become unavailable.

---

## 2. But cPanel/WHM Itself Is Different

This is the key point from the lesson.

The internal **cPanel and WHM interface pages are not served by Apache**.

So:

```
Apache DOWN
     |
     +---- Hosted websites → May be DOWN
     |
     +---- WHM/cPanel → Can still be accessible
```

This is extremely useful for troubleshooting.

### Example

Suppose Apache configuration breaks after a change.

Your hosted website:

```
https://example.com
```

might stop working.

But you may still be able to access:

```
WHM
```

From WHM, you can use **EasyApache 4** to repair or rebuild Apache.

You can also work through SSH:

```
SSH
 |
 v
CLI
 |
 v
EasyApache 4 / Package Manager
 |
 v
Apache
```

---

# 3. Why Is This Design Useful?

Imagine Apache is broken because of a bad configuration.

If WHM itself depended on Apache, you could potentially lose the ability to manage the server through WHM.

Instead:

```
Apache failure
      |
      v
WHM still accessible
      |
      v
EasyApache 4
      |
      v
Repair Apache
```

That's one reason this architecture is useful for server administration.

---

# 4. Two Ways to Manage EA4

EasyApache 4 can be managed through:

### WHM

```
WHM
 ↓
EasyApache 4
 ↓
Customize
 ↓
Apache / PHP / Modules
```

### Command Line

```
SSH
 ↓
Linux CLI
 ↓
yum / RPM / EA4 tools
 ↓
Apache / PHP / Modules
```

This course is specifically focused on:

> **Managing Apache with EasyApache 4 through the command line.**

---

# 5. EA4 Configuration Files

Another important point:

> **EasyApache 4 uses many configuration files in different locations.**

Don't expect everything to be inside one file such as:

```
/etc/httpd/httpd.conf
```

There are multiple EA4-managed paths and configuration files.

Conceptually:

```
EasyApache 4
     |
     +-- Apache configuration
     |
     +-- Virtual hosts
     |
     +-- Modules
     |
     +-- PHP configuration
     |
     +-- MPM configuration
     |
     +-- Other generated configuration
```

Later in the course, we'll identify:

- **What the files are**
- **Where they are**
- **What they do**
- **Which files are generated**
- **Which files you should avoid manually editing**

That last point is particularly important on cPanel servers.

---

# 6. Important cPanel Support Concept

If you're working on a cPanel server, don't think:

> "Apache is broken, I'll manually edit every Apache config file."

Instead think:

```
Problem
   ↓
Identify EA4 component
   ↓
Check configuration
   ↓
Use EA4/package tools
   ↓
Rebuild/reconfigure if required
   ↓
Test Apache
```

EA4 manages a lot of the Apache configuration for you.

---

## Remember This

```
cPanel websites
      ↓
    Apache

WHM/cPanel interface
      ↓
Not Apache

Apache broken
      ↓
WHM may still work
      ↓
Use EA4
      ↓
Repair Apache
```

### Interview one-liner

> **Apache serves cPanel-hosted websites, while the internal cPanel/WHM interface is independent of Apache, allowing administrators to access WHM and repair Apache through EasyApache 4 when Apache fails.**

---

# Lesson 2: Apache Paths to Remember

This is a **memorize section**. These paths are important for cPanel/Web Hosting Support and will come up during troubleshooting.

## Apache + EA4 Important Paths

| Purpose           | Path                              | What it contains                         |
| ----------------- | --------------------------------- | ---------------------------------------- |
| **Binary**        | `/usr/sbin/httpd`                 | Apache executable                        |
| **Logs**          | `/var/log/apache2/`               | Apache logs                              |
| **Configuration** | `/etc/apache2/`                   | Apache configuration                     |
| **Modules**       | `/usr/lib[64]/apache2/modules/`   | Apache modules                           |
| **Templates**     | `/var/cpanel/templates/apache2_*` | EA4 Apache configuration templates       |
| **User Data**     | `/var/cpanel/userdata/`           | cPanel account/domain configuration data |

---

## 1. Apache Binary

```
/usr/sbin/httpd
```

This is the **Apache executable/binary**.

You can verify it:

```
ls -l /usr/sbin/httpd
```

Check version:

```
/usr/sbin/httpd -v
```

You can also check:

```
which httpd
```

Expected:

```
/usr/sbin/httpd
```

### Remember

> **Binary = `/usr/sbin/httpd`**

---

# 2. Apache Logs

```
/var/log/apache2/
```

This directory contains Apache logs.

Check:

```
ls -lah /var/log/apache2/
```

You may find logs such as:

```
access_log
error_log
```

View the error log:

```
tail -f /var/log/apache2/error_log
```

View access log:

```
tail -f /var/log/apache2/access_log
```

For troubleshooting, logs are usually one of your **first places to check**.

### Remember

> **Logs = `/var/log/apache2/`**

---

# 3. Apache Configuration

```
/etc/apache2/
```

This is the main Apache configuration directory in EA4.

Check:

```
ls -lah /etc/apache2/
```

You will find various Apache configuration files and directories.

You can inspect the configuration:

```
find /etc/apache2 -maxdepth 2 -type f
```

### Remember

> **Configuration = `/etc/apache2/`**

---

# 4. Apache Modules

```
/usr/lib[64]/apache2/modules/
```

The exact path depends on the architecture.

On a 64-bit server, the course notes indicate:

```
/usr/lib64/apache2/modules/
```

Check:

```
ls -lah /usr/lib64/apache2/modules/
```

You may see module files such as:

```
mod_ssl.so
mod_rewrite.so
```

The `.so` files are compiled shared-object modules.

### Remember

> **Modules = `/usr/lib64/apache2/modules/` on 64-bit systems**

---

# 5. EA4 Apache Templates

```
/var/cpanel/templates/apache2_*
```

These are **templates used by cPanel/EasyApache** when generating Apache configuration.

The `*` is the wildcard we learned earlier.

So:

```
apache2_*
```

means:

> Anything beginning with `apache2_`.

You can inspect them with:

```
ls -lah /var/cpanel/templates/
```

Or:

```
ls -lah /var/cpanel/templates/apache2_*
```

### Important

Don't casually edit EA4-generated configuration files.

EA4/cPanel may regenerate configuration, so you need to understand **which files are source/templates and which are generated** before making changes.

---

# 6. cPanel User Data

```
/var/cpanel/userdata/
```

This contains cPanel account/domain-related configuration data used when building Apache configuration.

You can inspect it:

```
ls -lah /var/cpanel/userdata/
```

You may see directories associated with cPanel users.

Conceptually:

```
/var/cpanel/userdata/
        |
        +-- User 1
        |     |
        |     +-- Domain configuration
        |
        +-- User 2
              |
              +-- Domain configuration
```

This becomes particularly useful when troubleshooting **virtual hosts and domain-specific Apache configuration**.

---

# The 6 Paths You MUST Memorize

Use this memory table:

```
Binary       → /usr/sbin/httpd

Logs         → /var/log/apache2/

Configuration→ /etc/apache2/

Modules      → /usr/lib64/apache2/modules/

Templates    → /var/cpanel/templates/apache2_*

User Data    → /var/cpanel/userdata/
```

### Easy memory pattern

```
/usr/sbin/httpd
       ↓
   BINARY

/var/log/apache2/
       ↓
     LOGS

/etc/apache2/
       ↓
    CONFIG

/usr/lib64/apache2/modules/
       ↓
    MODULES

/var/cpanel/templates/apache2_*
       ↓
   TEMPLATES

/var/cpanel/userdata/
       ↓
   USER DATA
```


---

# Lesson 3: Looking into Package Managers

## 1. What is a Package Manager?

A **package manager** is a tool used to manage software packages on Linux.

It can:

- Install software
- Update software
- Remove software
- Find packages
- Resolve dependencies
- Get packages from repositories

For example:

```
yum install package-name
```

means:

> Find this package, download it, resolve its dependencies, and install it.

---

# 2. What is YUM?

**YUM** stands for:

> **Yellowdog Updater, Modified**

It is a package management tool used primarily with **RHEL-based Linux systems**.

Examples include:

- RHEL
- CentOS
- AlmaLinux
- Rocky Linux

EA4 traditionally uses RPM packages and tools such as `yum` to manage them.

The relationship is:

```
EasyApache 4
      |
      v
    RPM
      |
      v
     YUM
      |
      v
 Repository
      |
      v
Download / Install packages
```

---

# 3. What Can YUM Do?

### Install

```
yum install package-name
```

### Remove

```
yum remove package-name
```

### Update

```
yum update package-name
```

### Search

```
yum search apache
```

### Package information

```
yum info package-name
```

### List packages

```
yum list
```

### List installed packages

```
yum list installed
```

### List available packages

```
yum list available
```

### Show repositories

```
yum repolist
```

These commands are worth knowing for your interview.

---

# 4. YUM and RPM

Don't confuse **YUM** and **RPM**.

|RPM|YUM|
|---|---|
|Lower-level package tool|Higher-level package manager|
|Works directly with RPM packages|Manages packages and repositories|
|Does not automatically handle all dependencies|Handles dependency resolution|
|Can install `.rpm` files|Downloads packages from repositories|

Think:

```
YUM
 ↓
Find package
 ↓
Resolve dependencies
 ↓
Download RPM
 ↓
Install RPM
```

So:

> **RPM is the package format/tool, while YUM provides easier package management and repository handling.**

---

# 5. What is a Repository?

A repository is basically a **package warehouse**.

For example:

```
EA4 Repository
      |
      +-- Apache packages
      +-- PHP packages
      +-- PHP extensions
      +-- Apache modules
```

YUM connects to repositories to find packages.

Check repositories:

```
yum repolist
```

Example:

```
repo id             repo name
base                BaseOS
appstream            AppStream
ea4                 EasyApache 4
```

The exact repositories depend on the server.

---

# 6. Ubuntu and APT

The course is mainly focused on **RHEL-based systems**, but cPanel also supports Ubuntu.

Ubuntu uses:

> **APT = Advanced Package Tool**

So the basic comparison is:

|RHEL-based|Debian/Ubuntu|
|---|---|
|`yum`|`apt`|
|RPM packages|DEB packages|
|`.rpm`|`.deb`|
|RHEL/CentOS/AlmaLinux/Rocky|Ubuntu/Debian|

For example:

### Update repository information

RHEL-based:

```
yum update
```

Ubuntu:

```
apt update
```

### Install package

```
yum install nginx
```

Ubuntu:

```
apt install nginx
```

### Remove package

```
yum remove nginx
```

Ubuntu:

```
apt remove nginx
```

The basic idea is the same, but the package ecosystem is different.

---

# 7. Important: `yum update` vs `apt update`

This can confuse beginners.

With Ubuntu:

```
apt update
```

primarily refreshes the package lists.

But:

```
apt upgrade
```

actually upgrades installed packages.

In the course's RHEL/YUM context:

```
yum update
```

can update installed packages.

So don't blindly assume these commands behave exactly the same.

---

# 8. Why This Matters for EasyApache

EA4 uses packages for its components.

For example:

```
EA4
 |
 +-- Apache
 |
 +-- Apache Modules
 |
 +-- PHP
 |
 +-- PHP Extensions
 |
 +-- MPM
 |
 v
RPM Packages
 |
 v
YUM
```

Therefore, if you understand YUM, you can understand a large part of **EA4 CLI administration**.

---

# 9. Practical Commands

On an actual EA4 server, these are useful:

### Check YUM

```
yum --version
```

### Show repositories

```
yum repolist
```

### Search Apache packages

```
yum search apache
```

### Search EA4 packages

```
yum search ea-apache
```

### List installed EA4 packages

```
yum list installed | grep '^ea-'
```

### List available EA4 packages

```
yum list available | grep '^ea-'
```

### Get package details

```
yum info ea-apache24
```

---

# Quick Revision

```
YUM
 ↓
Yellowdog Updater, Modified

RPM
 ↓
RPM Package Manager

Repository
 ↓
Package warehouse

RHEL-based
 ↓
YUM + RPM

Ubuntu
 ↓
APT + DEB

EA4
 ↓
Apache/PHP/Modules
 ↓
RPM packages
 ↓
YUM
```

### Commands to memorize

```
yum install <package>
yum remove <package>
yum update <package>
yum list
yum list installed
yum list available
yum info <package>
yum search <package>
yum repolist
```

**Key interview line:**

> **EasyApache 4 uses RPM-packaged components, and on RHEL-based cPanel systems, YUM is used to install, remove, update, and manage those packages.**

---

# Lesson 3: EasyApache 4 + Package Manager Paths

## 1. EA4 Can Be Managed Directly

For a **small change**, you don't need a special EA4 script.

For example, if you want to add or remove one Apache module:

```
EA4
 ↓
Find package name
 ↓
yum / apt
 ↓
Install or remove package
```

Example:

```
yum install <ea4-package>
```

Remove:

```
yum remove <ea4-package>
```

So the key idea is:

> **Small package-level changes can be done directly with the system package manager.**

---

# 2. When Should You Use a Profile?

If you have **many changes**, using individual `yum` commands becomes inconvenient.

Instead:

```
Many changes
    ↓
Create custom EA4 profile
    ↓
Provision profile
    ↓
Apache/PHP configuration updated
```

So remember:

|Situation|Approach|
|---|---|
|Add one module|Package manager|
|Remove one module|Package manager|
|Change a few packages|Package manager|
|Many Apache/PHP changes|Custom profile|

---

# 3. Important EA4 Package Manager Paths

There are **4 paths** you should memorize from this section.

## 3.1 Profile Installation Script

```
/usr/local/bin/ea_install_profile
```

This is used to **install/provision an EA4 profile**.

Think:

```
Profile
   ↓
ea_install_profile
   ↓
Install/provision configuration
```

---

## 3.2 Profile Creation Script

```
/usr/local/bin/ea_current_to_profile
```

This creates a profile from the **current EA4 configuration**.

Think:

```
Current EA4 configuration
          ↓
ea_current_to_profile
          ↓
Custom profile
```

This is useful when you have a server configured the way you want and want to save that configuration as a profile.

---

# 4. Repository Folder

For YUM:

```
/etc/yum.repos.d/
```

For APT:

```
/etc/apt/sources.list.d/
```

These locations contain repository configuration.

### YUM

```
ls -lah /etc/yum.repos.d/
```

### APT

```
ls -lah /etc/apt/sources.list.d/
```

Conceptually:

```
/etc/yum.repos.d/
        |
        +-- Repository definitions
        |
        +-- EA4 repository
        +-- Other repositories
```

---

# 5. YUM Universal Hooks

The course gives this path:

```
/etc/yum/universal-hooks/multi_pkgs/posttrans/ea-__WILDCARD__/
```

This is an advanced EA4 mechanism.

The important thing to understand now is:

> **Universal hooks allow actions to happen automatically when package transactions occur.**

For example:

```
yum install/update
       |
       v
Package transaction
       |
       v
Universal hook
       |
       v
EA4-related action
```

The `__WILDCARD__` part indicates that the path can match different EA4-related package names.

For APT, the corresponding location is:

```
/etc/apt/universal-hooks/
```

You don't need to memorize the entire hook mechanism yet. Just understand **what it is for**.

---

# 6. Four Paths to Memorize

### EA4 Profile Installation

```
/usr/local/bin/ea_install_profile
```

### EA4 Profile Creation

```
/usr/local/bin/ea_current_to_profile
```

### YUM Repositories

```
/etc/yum.repos.d/
```

### APT Repositories

```
/etc/apt/sources.list.d/
```

### YUM Hooks

```
/etc/yum/universal-hooks/
```

### APT Hooks

```
/etc/apt/universal-hooks/
```

---

# 7. Easy Memory Trick

Think:

```
PROFILE
/usr/local/bin/
       |
       +-- ea_install_profile
       +-- ea_current_to_profile

REPOSITORIES
/etc/
   |
   +-- yum.repos.d/
   +-- apt/sources.list.d/

HOOKS
/etc/
   |
   +-- yum/universal-hooks/
   +-- apt/universal-hooks/
```

---

## Final Lesson 3 Cheat Sheet

```
Small EA4 change
    ↓
yum / apt directly

Many EA4 changes
    ↓
Custom profile
```

```
Profile Install
/usr/local/bin/ea_install_profile

Profile Creation
/usr/local/bin/ea_current_to_profile

YUM Repositories
/etc/yum.repos.d/

APT Repositories
/etc/apt/sources.list.d/

YUM Hooks
/etc/yum/universal-hooks/

APT Hooks
/etc/apt/universal-hooks/
```

**Interview line to remember:**

> **EasyApache 4 integrates with the system package manager, so individual components can be installed or removed directly using YUM or APT. For larger configuration changes, an EA4 profile can be created and provisioned.**

---

# Lesson 4: Looking into PHP


---

# 1. What is PHP?

**PHP** stands for:

> **PHP: Hypertext Preprocessor**

It is a programming language mainly used for **server-side web applications**.

Example:

```
Browser
   |
   | HTTP request
   v
Apache
   |
   v
PHP
   |
   v
Application code
   |
   v
HTML response
   |
   v
Browser
```

For example, a WordPress website commonly uses PHP.

---

# 2. PHP Uses the Zend Engine

The PHP interpreter used by cPanel is powered by the **Zend Engine**.

Think of it like:

```
PHP Code
   |
   v
PHP Interpreter
   |
   v
Zend Engine
   |
   v
Execution
```

You don't need to go deep into Zend Engine for this course.

Just remember:

> **Zend Engine is the execution engine used by PHP.**

---

# 3. cPanel Has PHP in Two Contexts

This is one of the most important points in this lesson.

The course says PHP is installed **twice**.

### PHP for user websites

Used by:

```
Apache
   ↓
User websites
```

Examples:

- WordPress
- Laravel
- PHP applications
- Customer websites

### PHP for cPanel's internal applications

Used by:

- cPanel add-ons
- Webmail
- Other applications served by cPanel's internal web server

So conceptually:

```
                  PHP
                   |
          +--------+--------+
          |                 |
          v                 v
     Apache PHP        cPanel PHP
          |                 |
          v                 v
 User websites       Internal apps
```

### Important

This course focuses on:

> **PHP used by Apache for user websites.**

---

# 4. PHP Extensions

PHP can be extended using **extensions**.

For example, an application may need a particular PHP extension to communicate with a database or perform a specific task.

Conceptually:

```
PHP
 |
 +-- Core
 |
 +-- Extension 1
 |
 +-- Extension 2
 |
 +-- Extension 3
```

Extensions can be installed through:

### EasyApache 4

Preferred when the extension is available through EA4.

### PECL

PECL can provide extensions that aren't available through EasyApache.

---

# 5. EA4 vs PECL

The course gives an important rule:

> **Do not install the same extension using both EA4 and PECL.**

For example, don't do:

```
EA4
 +
PECL
 ↓
Same extension
```

This can create conflicts.

Preferred approach:

```
Extension available in EA4?
        |
       YES
        ↓
Install through EA4
```

If it's **not available through EA4**:

```
Not available in EA4
        |
        v
Consider PECL
```

### Interview answer

> **Use EasyApache 4 for PHP extensions when available. Use PECL only for extensions that are not provided by EasyApache 4.**

---

# 6. PHP Binary

The first important PHP path is:

```
/usr/bin/php
```

This is the PHP CLI binary.

Check it:

```
ls -l /usr/bin/php
```

Check PHP version:

```
php -v
```

Example:

```
PHP 8.x.x
```

You can also check:

```
which php
```

Expected:

```
/usr/bin/php
```

### Remember

> **PHP Binary = `/usr/bin/php`**

---

# 7. PHP Configuration

The course gives:

```
/etc/apache2/conf.d/php.conf
```

This is an Apache configuration file related to PHP.

Check it:

```
ls -l /etc/apache2/conf.d/php.conf
```

You can inspect it:

```
cat /etc/apache2/conf.d/php.conf
```

Or:

```
less /etc/apache2/conf.d/php.conf
```

### Remember

> **PHP Apache configuration = `/etc/apache2/conf.d/php.conf`**

---

# 8. PHP Rebuild Tool

Another very important path:

```
/usr/local/cpanel/bin/rebuild_phpconf
```

This is a cPanel tool used to **rebuild PHP configuration**.

Check:

```
ls -l /usr/local/cpanel/bin/rebuild_phpconf
```

You may also inspect its help:

```
/usr/local/cpanel/bin/rebuild_phpconf --help
```

### Remember

> **PHP rebuild tool = `/usr/local/cpanel/bin/rebuild_phpconf`**

This is especially relevant when dealing with PHP handler/configuration changes.

---

# 9. MultiPHP Base Path

The course gives:

```
/opt/cpanel/ea-php##
```

The `##` represents the PHP version.

For example, you might have:

```
/opt/cpanel/ea-php81
/opt/cpanel/ea-php82
/opt/cpanel/ea-php83
```

So:

```
/opt/cpanel/ea-php##
```

means:

> The EasyApache PHP installation directory for a particular PHP version.

You can check:

```
ls -lah /opt/cpanel/
```

Then:

```
ls -lah /opt/cpanel/ea-php83/
```

if PHP 8.3 is installed.

---

# 10. Why MultiPHP Matters

cPanel supports multiple PHP versions on the same server.

For example:

```
Server
 |
 +-- PHP 8.1
 |
 +-- PHP 8.2
 |
 +-- PHP 8.3
 |
 +-- PHP 8.4
```

Different websites can use different PHP versions.

Example:

```
example1.com → PHP 8.2
example2.com → PHP 8.3
example3.com → PHP 8.4
```

This is the basic idea behind **MultiPHP**.

---

# 11. PHP Paths to Memorize

These are the four paths from this lesson:

```
PHP Binary
/usr/bin/php

PHP Apache Configuration
/etc/apache2/conf.d/php.conf

PHP Rebuild Tool
/usr/local/cpanel/bin/rebuild_phpconf

MultiPHP Base Path
/opt/cpanel/ea-php##
```

### Memory table

|Purpose|Path|
|---|---|
|PHP Binary|`/usr/bin/php`|
|PHP Configuration|`/etc/apache2/conf.d/php.conf`|
|PHP Rebuild Tool|`/usr/local/cpanel/bin/rebuild_phpconf`|
|MultiPHP Base Path|`/opt/cpanel/ea-php##`|

---

# 12. Useful CLI Commands

Check PHP:

```
php -v
```

Find PHP:

```
which php
```

Check PHP modules:

```
php -m
```

Search for a module:

```
php -m | grep mysqli
```

Check PHP configuration:

```
php --ini
```

Show detailed PHP configuration:

```
php -i
```

Search configuration:

```
php -i | grep memory_limit
```

These are very useful for troubleshooting PHP applications.

---

# 13. Example Troubleshooting

Suppose a customer says:

> "My PHP application says `mysqli` is missing."

First check:

```
php -m | grep mysqli
```

If nothing appears, investigate whether the required PHP extension is installed for the **correct PHP version**.

You could then inspect EA4 packages:

```
yum list installed | grep ea-php
```

For example:

```
ea-php83
ea-php83-php-mysqlnd
```

The important point is that **PHP version matters**.

An extension installed for PHP 8.2 doesn't automatically mean it is available to PHP 8.3.

---

# Lesson 4 Quick Revision

```
PHP
 ↓
Server-side scripting language
 ↓
Zend Engine
```

### cPanel PHP

```
PHP
 |
 +-- Apache PHP
 |      ↓
 |   User websites
 |
 +-- cPanel PHP
        ↓
   Internal applications
```

### Extensions

```
Available in EA4?
       |
      YES
       ↓
Use EA4

Not available?
       |
       ↓
Consider PECL
```

### Must Memorize

```
/usr/bin/php
/etc/apache2/conf.d/php.conf
/usr/local/cpanel/bin/rebuild_phpconf
/opt/cpanel/ea-php##
```

### Most important interview concept

> **EA4 manages PHP versions and extensions for cPanel-hosted websites. When a PHP extension is available through EA4, it should be installed through EA4 rather than independently through PECL.**

**Next: the course should continue with the remaining PHP CLI/path details, then we can move toward the Module 1 quiz.**

---

| #     | Question                                                                              | Correct Answer            | Key Reason                                                    |
| ----- | ------------------------------------------------------------------------------------- | ------------------------- | ------------------------------------------------------------- |
| **1** | Which command displays all available EA4 packages from CLI?                           | **B. `yum list "ea-*"`**  | Lists packages matching the `ea-*` pattern                    |
| **2** | Which parent directory contains EasyApache templates and userdata?                    | **D. `/var/cpanel/`**     | Contains `/var/cpanel/templates/` and `/var/cpanel/userdata/` |
| **3** | Which term refers to a command-line program, package format, and individual packages? | **A. RPM**                | RPM can mean the manager, format, or package                  |
| **4** | Which term describes special text syntax for a search pattern?                        | **C. Regular Expression** | Regex defines patterns for searching/matching text            |
| **5** | Which term describes a part of a process that shares resources and executes commands? | **A. Thread**             | A thread is an execution unit within a process                |

---






