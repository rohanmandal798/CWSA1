**Lesson - 1 : Managing MultiPHP from the Command-line
Lesson - 2 : Managing PHP Handlers with MultiPHP
Lesson - 3 : Editing the system php.ini Files
Lesson - 4 : Using PHP from the Command-line
Lesson - 5 : Changing the PHP version in Use **

# Lesson 1: Managing MultiPHP from the Command Line

This lesson introduces **MultiPHP** and the important `rebuild_phpconf` command.

## 1. What is MultiPHP?

**MultiPHP** allows you to use different PHP versions for different virtual hosts/domains.

For example:

|Domain|PHP Version|
|---|---|
|`site1.com`|PHP 8.1|
|`site2.com`|PHP 8.2|
|`site3.com`|PHP 8.4|

So PHP can be configured **per virtual host**.

---

## 2. Global PHP Configuration

Even though individual domains can have different PHP versions, the server still needs a **global/default PHP configuration**.

cPanel provides:

```
/usr/local/cpanel/bin/rebuild_phpconf
```

This script is used to manage the global PHP configuration.

---

## 3. What can `rebuild_phpconf` do?

It can:

- Rebuild PHP configuration
- Change the **default PHP version**
- Manage PHP handlers
- Restore/rebuild configuration after manual changes

### Important command

```
/usr/local/cpanel/bin/rebuild_phpconf
```

You can use it to view the current PHP configuration and make changes.

---

## 4. Key Concept

Think of it like this:

```
                MultiPHP
                   |
        ┌──────────┴──────────┐
        ↓                     ↓
 Per-domain PHP          Global/default PHP
        |                     |
 site1 → PHP 8.1       rebuild_phpconf
 site2 → PHP 8.2
 site3 → PHP 8.4
```

---

# Interview Focus

### Q: What is MultiPHP?

> MultiPHP allows different virtual hosts or domains on the same cPanel server to use different PHP versions.

### Q: What is `rebuild_phpconf`?

> `/usr/local/cpanel/bin/rebuild_phpconf` is a cPanel command-line utility used to rebuild PHP configuration, change the default PHP version, and manage PHP handlers.

### Must Remember

```
MultiPHP
   ↓
Per-domain PHP versions

rebuild_phpconf
   ↓
Global/default PHP configuration
```

**Command path to memorize:**

```
/usr/local/cpanel/bin/rebuild_phpconf
```

---

# Lesson 2: Managing PHP Handlers with MultiPHP

This lesson is very important for **cPanel interview questions** because it focuses on PHP handlers and `rebuild_phpconf`.

## 1. What is a PHP Handler?

A **PHP handler** tells Apache **how PHP should be executed**.

Common handlers:

- `CGI`
- `suPHP`
- `DSO`
- `FCGI`

### Important

Handlers are configured **globally**, not per virtual host.

---

## 2. PHP Handler Comparison

|Handler|Key point|
|---|---|
|**CGI**|Guaranteed to be installed, mainly fallback|
|**suPHP**|Recommended for most scenarios in this lesson|
|**DSO**|Requires the appropriate `ea-phpXX-php` RPM|
|**FCGI**|Requires additional configuration|
|**none**|Disables that PHP version through Apache|

---

# 3. Check Current Configuration

Command:

```
/usr/local/cpanel/bin/rebuild_phpconf --current
```

Example:

```
DEFAULT PHP: ea-php55
ea-php54 SAPI: CGI
ea-php55 SAPI: CGI
ea-php56 SAPI: CGI
```

This tells you:

- Default PHP = `ea-php55`
- PHP 5.4 handler = `CGI`
- PHP 5.5 handler = `CGI`
- PHP 5.6 handler = `CGI`

---

# 4. Change a PHP Handler

Example:

```
/usr/local/cpanel/bin/rebuild_phpconf --ea-php54=suphp --errors
```

This changes:

```
PHP 5.4
CGI → suPHP
```

### `--errors`

```
--errors
```

Prints errors directly to **STDERR**, so you can see them in the terminal.

---

# 5. Check Available Handlers

Use:

```
/usr/local/cpanel/bin/rebuild_phpconf --available
```

Example:

```
ea-php54: CGI none suphp
ea-php55: CGI none suphp
ea-php56: CGI none suphp
```

This shows which handlers are currently available for each PHP version.

---

# 6. What Does `none` Mean?

If you configure:

```
ea-php54: none
```

PHP 5.4 is **disabled through Apache**.

But PHP 5.4 can still be used from the **command line**.

For example, a cron job could still execute PHP 5.4.

That's an important distinction.

---

# 7. `rebuild_phpconf` Options

Memorize these:

|Option|Purpose|
|---|---|
|`--current`|Show current PHP settings|
|`--available`|Show available handlers|
|`--default`|Set default PHP version|
|`--<ver>=<handler>`|Set handler for a PHP version|
|`--dry-run`|Show what would change without applying|
|`--no-restart`|Don't restart Apache|
|`--errors`|Display errors on STDERR|
|`--no-users`|Don't update user settings|
|`--help`|Show help|

---

## 8. Very Important Commands

### Check current configuration

```
/usr/local/cpanel/bin/rebuild_phpconf --current
```

### Check available handlers

```
/usr/local/cpanel/bin/rebuild_phpconf --available
```

### Change handler

```
/usr/local/cpanel/bin/rebuild_phpconf --ea-php54=suphp --errors
```

### Set default PHP

```
/usr/local/cpanel/bin/rebuild_phpconf --default=ea-php84
```

### Test changes without applying them

```
/usr/local/cpanel/bin/rebuild_phpconf --dry-run
```

### See help

```
/usr/local/cpanel/bin/rebuild_phpconf --help
```

---

# 9. DSO RPM Important Point

For DSO, the RPM follows this naming pattern:

```
ea-phpXX-php
```

Examples:

```
ea-php56-php
ea-php72-php
ea-php84-php
```

`XX` represents the PHP version.

Also, **only one DSO version can be installed at a time** because they use the same module name.

---

# Interview Questions

### Q1. Are PHP handlers configured per virtual host?

**Answer:** No. PHP handlers are configured **globally**.

### Q2. Which handler is guaranteed to be installed?

**Answer:** `CGI`.

### Q3. Which handler does this course recommend for most scenarios?

**Answer:** `suPHP`.

### Q4. How do you check the current PHP handler configuration?

```
/usr/local/cpanel/bin/rebuild_phpconf --current
```

### Q5. How do you see available handlers?

```
/usr/local/cpanel/bin/rebuild_phpconf --available
```

### Q6. What does `--dry-run` do?

> It shows the changes that would be made without actually applying them.

### Q7. What does `--no-restart` do?

> It prevents Apache from being restarted after making the configuration changes.

### Q8. What happens if the handler is set to `none`?

> That PHP version is disabled through Apache, but it remains available for command-line execution such as cron jobs.

---

## Quick Memory

```
--current    → What am I using?
--available  → What can I use?
--default    → Which PHP is default?
--<ver>      → Change handler
--dry-run    → Show changes only
--errors     → Show errors
--no-restart → Don't restart Apache
--help       → Show options
```

**Core command to remember:**

```
/usr/local/cpanel/bin/rebuild_phpconf
```

This is the command you'll keep coming back to throughout the MultiPHP lessons.

---

# Lesson 3: Editing the System `php.ini` Files

This lesson is about **where MultiPHP stores `php.ini` files** and what you need to do after editing them.

## 1. Why are there multiple `php.ini` files?

With MultiPHP, a server can run multiple PHP versions at the same time.

For example:

```
PHP 5.6 → php.ini
PHP 7.2 → php.ini
PHP 8.4 → php.ini
```

Each PHP version needs its **own configuration file**.

---

## 2. Important Path

The old file:

```
/usr/local/lib/php.ini
```

still exists, but **MultiPHP does not use it**.

MultiPHP uses:

```
/opt/cpanel/ea-php##/root/etc/php.ini
```

Where `##` is the two-digit PHP version.

### Examples

PHP 5.6:

```
/opt/cpanel/ea-php56/root/etc/php.ini
```

PHP 7.2:

```
/opt/cpanel/ea-php72/root/etc/php.ini
```

PHP 8.4:

```
/opt/cpanel/ea-php84/root/etc/php.ini
```

---

# 3. Editing php.ini

There is nothing special about editing these files.

You can use:

```
vi /opt/cpanel/ea-php84/root/etc/php.ini
```

or:

```
nano /opt/cpanel/ea-php84/root/etc/php.ini
```

For example, you might change:

```
memory_limit = 128M
```

to:

```
memory_limit = 256M
```

---

# 4. Important: Changes Depend on the PHP Handler

After editing `php.ini`, whether you need to restart Apache depends on the **PHP handler**.

|PHP Handler|What happens after `php.ini` change|
|---|---|
|**suPHP**|Changes take effect immediately|
|**DSO**|Requires a graceful Apache restart|

### Remember

```
suPHP → Instant
DSO   → Graceful restart
```

This is a very likely interview question.

---

# 5. Practical Example

Suppose the domain uses **PHP 8.4** and you want to increase memory:

```
vi /opt/cpanel/ea-php84/root/etc/php.ini
```

Change:

```
memory_limit = 128M
```

to:

```
memory_limit = 256M
```

Then check which handler PHP 8.4 is using:

```
/usr/local/cpanel/bin/rebuild_phpconf --current
```

If it's **suPHP**, the change takes effect immediately.

If it's **DSO**, perform a graceful Apache restart.

---

# Interview Questions

### Q1. Is `/usr/local/lib/php.ini` used by MultiPHP?

**Answer:** No. It still exists, but MultiPHP does not use it.

### Q2. Where is PHP 5.6's `php.ini`?

```
/opt/cpanel/ea-php56/root/etc/php.ini
```

### Q3. Where is PHP 8.4's `php.ini`?

```
/opt/cpanel/ea-php84/root/etc/php.ini
```

### Q4. Does changing `php.ini` always require restarting Apache?

**Answer:** No.

- **suPHP:** changes take effect immediately
- **DSO:** requires a graceful restart

### Q5. Why does MultiPHP need multiple `php.ini` files?

> Because different PHP versions can run simultaneously, and each version can have its own configuration.

---

## Quick Memory

```
MultiPHP
   ↓
Multiple PHP versions
   ↓
Multiple php.ini files

PHP 8.4
   ↓
/opt/cpanel/ea-php84/root/etc/php.ini

suPHP → instant
DSO   → graceful restart
```

**Most important path to memorize:**

```
/opt/cpanel/ea-php##/root/etc/php.ini
```

---

# Lesson 4: Using PHP from the Command Line

This lesson explains how to run **different PHP versions from CLI** in a cPanel MultiPHP environment.

## 1. PHP CLI Package

Each PHP version needs its corresponding **`php-cli` package** installed.

Once installed, there are two main ways to run PHP from the command line:

1. `/usr/local/bin/php`
2. `scl enable`

---

# 2. `/usr/local/bin/php`

You can run:

```
/usr/local/bin/php --version
```

This PHP command checks for a **`.htaccess` file**.

### If `.htaccess` exists

It can determine which PHP version should be used.

### If `.htaccess` does not exist

The **system default PHP version** is used.

So:

```
/usr/local/bin/php
       ↓
Check .htaccess
       ↓
Found? → Use configured PHP version
       ↓
Not found? → Use system default PHP
```

---

# 3. Check PHP Version

Basic command:

```
php --version
```

or:

```
/usr/local/bin/php --version
```

Example output:

```
PHP 5.5.31 (cli)
```

The PHP version can change depending on **where/how the command is executed**.

---

# 4. Using SCL to Select PHP Explicitly

If you need a **specific PHP version**, use **Software Collection Libraries (SCL)**.

Syntax:

```
scl enable <php-package> "<command>"
```

Example:

```
scl enable ea-php55 "php --version"
```

This explicitly runs the command using:

```
ea-php55
```

which means **PHP 5.5**.

Output:

```
PHP 5.5.31 (cli)
```

---

# 5. SCL Command Structure

Remember the three parts:

```
scl enable ea-php55 "php --version"
│   │      │          │
│   │      │          └── Command to execute
│   │      └───────────── PHP package/version
│   └──────────────────── Action
└──────────────────────── SCL command
```

### General format

```
scl enable <package> "<command>"
```

Examples:

```
scl enable ea-php74 "php --version"
```

```
scl enable ea-php81 "php script.php"
```

```
scl enable ea-php84 "php -m"
```

---

# 6. When to Use Which?

|Method|Purpose|
|---|---|
|`/usr/local/bin/php`|Uses `.htaccess` or system default|
|`scl enable`|Explicitly select a PHP version|

### Easy memory

```
/usr/local/bin/php
→ Automatic/default selection

scl enable
→ Explicit PHP version
```

---

# Interview Questions

### Q1. How can you run PHP from the command line?

**Answer:**

> PHP can be run using `/usr/local/bin/php`, or a specific PHP version can be selected using the SCL `scl enable` command.

### Q2. How do you explicitly run PHP 5.5?

```
scl enable ea-php55 "php --version"
```

### Q3. What does `scl enable` do?

> It enables a specific Software Collection and executes the specified command using that collection.

### Q4. What does `/usr/local/bin/php` use if no `.htaccess` file exists?

> It uses the system default PHP version.

### Q5. What package is required to run PHP from CLI?

> The appropriate **`php-cli` package** for the PHP version.

---

## Quick Revision

```
MultiPHP CLI
     ↓
php-cli package required
     ↓
 ┌─────────────────────┐
 │                     │
 ↓                     ↓
/usr/local/bin/php    scl enable
 │                     │
 ↓                     ↓
.htaccess/default     Specific PHP
                       version
```

**Most important command:**

```
scl enable ea-php84 "php --version"
```

This is the one to remember when the interviewer asks:

> **"How would you run a specific PHP version from the command line on a cPanel server?"**

---

# Lesson 5: Changing the PHP Version in Use

This lesson covers **changing PHP versions, custom PHP settings, and PECL**.

## 1. Changing PHP Version

In this lesson, two files are involved:

### `.htaccess`

Controls **which PHP version the PHP binary uses**.

### cPanel userdata file

```
/var/cpanel/userdata/user/domain
```

Controls **what PHP version is displayed in cPanel & WHM**.

So remember:

```
.htaccess
   ↓
Controls actual PHP version

userdata file
   ↓
Controls cPanel/WHM displayed configuration
```

### Important

`.htaccess` files are inherited by child directories.

Therefore, you **do not need to manually configure the PHP version in every subdirectory**.

---

# 2. Custom PHP Settings

The method for changing PHP settings depends on the PHP handler.

|Handler|Configuration method|
|---|---|
|**DSO**|`.htaccess`|
|**CGI**|Partial `php.ini`|
|**FCGI**|Partial `php.ini`|
|**suPHP**|Full `php.ini` + `suPHP_ConfigPath`|

### Easy memory

```
DSO       → .htaccess
CGI       → partial php.ini
FCGI      → partial php.ini
suPHP     → full php.ini + suPHP_ConfigPath
```

---

# 3. What is PECL?

**PECL** stands for:

> PHP Extension Community Library

It is a repository for **PHP extensions**.

PHP extensions add extra functionality to PHP.

For example, an application might require a particular PHP extension. The application's documentation will normally tell you which PECL extension is required.

### Simple example

```
Application
     ↓
Requires PHP extension
     ↓
Find extension in PECL
     ↓
Install/configure extension
     ↓
Application works
```

---

# Interview Questions

### Q1. Which file controls the PHP version actually used?

**Answer:** `.htaccess`.

### Q2. What does the userdata file control?

```
/var/cpanel/userdata/user/domain
```

It controls what is displayed in the **cPanel & WHM interfaces**.

### Q3. Do you need to configure PHP version in every child directory?

**Answer:** No. `.htaccess` settings are inherited by child directories.

### Q4. How do you customize PHP settings with DSO?

**Answer:** Using `.htaccess`.

### Q5. How do you customize PHP settings with CGI/FCGI?

**Answer:** Using partial `php.ini` files.

### Q6. How do you customize PHP settings with suPHP?

**Answer:** Using the complete `php.ini` file and `suPHP_ConfigPath` directives.

### Q7. What is PECL?

**Answer:**

> PECL, PHP Extension Community Library, is a repository for PHP extensions that add functionality to the PHP interpreter.

---

# Final MultiPHP Revision

You have now completed all **5 lessons**.

```
Lesson 1
rebuild_phpconf
        ↓
Global PHP configuration

Lesson 2
PHP Handlers
        ↓
CGI / suPHP / DSO / FCGI

Lesson 3
php.ini
        ↓
/opt/cpanel/ea-php##/root/etc/php.ini

Lesson 4
PHP CLI
        ↓
/usr/local/bin/php
        ↓
SCL for specific PHP version

Lesson 5
PHP Version + Extensions
        ↓
.htaccess
userdata
PECL
```

## Must-Memorize Paths

```
/usr/local/cpanel/bin/rebuild_phpconf

/opt/cpanel/ea-php##/root/etc/php.ini

/var/cpanel/userdata/user/domain

/usr/local/bin/php
```

## Must-Memorize Commands

```
rebuild_phpconf --current
rebuild_phpconf --available
rebuild_phpconf --default=ea-php84
rebuild_phpconf --ea-php84=suphp
scl enable ea-php84 "php --version"
```

These are the **high-value points** I'd expect in a cPanel/Linux support interview.