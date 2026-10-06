# Interview Questions

### Q1. What is EasyApache 4?

**Answer:**

EasyApache 4 is cPanel's system for managing Apache, PHP, modules, extensions, and related web-server components.

### Q2. Does EA4 have a single unified CLI?

**Answer:**

No. EA4 uses normal Linux command-line tools, mainly the package manager and other system utilities, to manage its components.

### Q3. What package manager is commonly used with EA4?

**Answer:**

`yum` on the traditional RPM-based cPanel systems covered by this course.

### Q4. How do you install an EA4 package?

```
yum install package-name
```

### Q5. How do you remove an EA4 package?

```
yum remove package-name
```

### Q6. How do you update an EA4 package?

```
yum update package-name
```

### Q7. What does `yum list` do?

It displays package availability and package status.

### Q8. What does `yum info` do?

It displays detailed information about a package.

### Q9. What does MPM stand for?

**Multi-Processing Module.**

### Q10. Why is CLI management useful?

It allows administrators to manage and troubleshoot servers remotely, automate tasks, and work without relying on the WHM graphical interface.

---

# 11. Important Interview Questions

### Q1. What is Apache?

Apache HTTP Server is an open-source web server that accepts HTTP requests and returns web content to clients.

### Q2. What is EasyApache 4?

EA4 is cPanel's system for managing Apache, PHP, modules, extensions, and related web-server components.

### Q3. How does EA4 distribute Apache modules?

Apache modules are distributed as **RPM packages** and managed through the system package manager.

### Q4. What is CGI?

CGI stands for **Common Gateway Interface**. It allows Apache to pass requests to external programs and return their output to clients.

### Q5. How do you check Apache's version?

```
httpd -v
```

### Q6. How do you test Apache configuration?

```
httpd -t
```

Expected:

```
Syntax OK
```

### Q7. How do you check loaded Apache modules?

```
httpd -M
```

### Q8. What is an Apache module?

A module is a component that extends Apache's functionality.

### Q9. What is MPM?

MPM stands for **Multi-Processing Module**. It controls how Apache handles processes, threads, and client connections.

---

# Interview Questions

### Q1. Does cPanel use Apache to serve hosted websites?

**Yes.** Apache serves the websites hosted on the cPanel server.

### Q2. Is WHM itself served by Apache?

**No.** Internal cPanel/WHM interface pages are not served by Apache.

### Q3. What happens if Apache stops?

Hosted websites may become unavailable, but cPanel/WHM can still be accessible.

### Q4. Why is this useful?

It allows administrators to access WHM and/or SSH and use EasyApache 4 to troubleshoot and restore Apache.

### Q5. How can EA4 be managed?

Through:

- WHM
- Command line


---
## Interview Questions

**Q: Where is the Apache binary?**

```
/usr/sbin/httpd
```

**Q: Where are Apache logs?**

```
/var/log/apache2/
```

**Q: Where is the Apache configuration?**

```
/etc/apache2/
```

**Q: Where are Apache modules?**

```
/usr/lib64/apache2/modules/
```

**Q: Where are EA4 Apache templates?**

```
/var/cpanel/templates/apache2_*
```

**Q: Where is cPanel Apache user data?**

```
/var/cpanel/userdata/
```

### One thing to be careful about

Don't confuse:

```
/etc/apache2/
```

with:

```
/var/cpanel/templates/
```

The first is the **Apache configuration area**.

The second contains **cPanel/EA4 templates used to generate configuration**.

That's a common interview/troubleshooting distinction.

---

# 10. Interview Questions

### Q1. What does YUM stand for?

**Yellowdog Updater, Modified.**

### Q2. What is YUM?

A package management tool used primarily on RHEL-based Linux systems to install, update, remove, and manage software packages.

### Q3. What is RPM?

RPM stands for **RPM Package Manager**. It refers to the package management system, RPM package format, or packages using that format.

### Q4. What is a repository?

A repository is a source/location containing software packages that the package manager can access.

### Q5. What is the difference between YUM and RPM?

**RPM** works directly with RPM packages, while **YUM** provides higher-level package management and repository/dependency handling.

### Q6. Which package manager does Ubuntu use?

**APT**, or Advanced Package Tool.

### Q7. How do you list enabled repositories?

```
yum repolist
```

### Q8. How do you search for an Apache package?

```
yum search apache
```

### Q9. How do you get information about a package?

```
yum info package-name
```

### Q10. How does this relate to EA4?

EA4 distributes Apache, PHP, modules, and related components as packages, which are managed through the operating system's package-management tools.


---

# 8. Important Interview Questions

### Q1. How do you install a single EA4 package?

Use the system package manager, for example:

```
yum install <package>
```

### Q2. Do you need an EA4-specific script to install one module?

**No.** You can use the system package manager directly.

### Q3. When should you consider using an EA4 profile?

When you have **many changes** to make to the Apache/PHP configuration.

### Q4. What is `ea_install_profile`?

```
/usr/local/bin/ea_install_profile
```

It is the EA4 profile installation/provisioning script.

### Q5. What is `ea_current_to_profile`?

```
/usr/local/bin/ea_current_to_profile
```

It creates a profile from the current EA4 configuration.

### Q6. Where are YUM repository configuration files?

```
/etc/yum.repos.d/
```

### Q7. Where are APT repository configuration files?

```
/etc/apt/sources.list.d/
```

### Q8. What are universal hooks?

They allow package transactions to trigger additional automated actions.

---

# 14. Interview Questions

### Q1. What is PHP?

PHP is a server-side programming language commonly used for web applications.

### Q2. What engine powers PHP?

**Zend Engine.**

### Q3. Where is the PHP CLI binary?

```
/usr/bin/php
```

### Q4. Where is the Apache PHP configuration?

```
/etc/apache2/conf.d/php.conf
```

### Q5. What is `rebuild_phpconf`?

It is a cPanel utility used to rebuild PHP configuration.

Location:

```
/usr/local/cpanel/bin/rebuild_phpconf
```

### Q6. What is the MultiPHP base path?

```
/opt/cpanel/ea-php##
```

The `##` represents the PHP version.

### Q7. How do you check the PHP version?

```
php -v
```

### Q8. How do you list PHP extensions?

```
php -m
```

### Q9. Should you install the same PHP extension through both EA4 and PECL?

**No.**

### Q10. Which should you prefer when an extension is available through EA4?

**EasyApache 4.**

Use PECL when the required extension isn't available through EA4.

---


# Interview Questions From This Lesson

### Q1. How do you list EA4 packages?

```
yum list ea-*
```

### Q2. What does `*` mean?

It is a **wildcard representing zero or more characters**.

### Q3. How do you get detailed information about an EA4 package?

```
yum info <package>
```

### Q4. How do you install an EA4 package?

```
yum install <package>
```

### Q5. Do you need to specify `.x86_64` when installing?

**No.** The package manager automatically selects the appropriate architecture. Pasted text

### Q6. What happens after installing an EA4 module?

EA4 performs a **graceful Apache restart** so the change takes effect. Pasted text

### Q7. How do you enable the EA4 experimental repository?

```
yum install ea4-experimental
```

### Q8. Should you use the experimental repository on production?

**No, the course recommends avoiding it on production servers.**



---



# 8. Interview Questions

### Q1. What is an EA4 profile?

An **EA4 profile is a collection of packages that will be installed together**.

### Q2. What happens to EA4 packages not listed in the profile?

They are **removed** when the profile is installed.

### Q3. What command installs the default cPanel EA4 profile?

```
/usr/local/bin/ea_install_profile --install /etc/cpanel/ea4/profiles/cpanel/default.json
```

### Q4. What does `--install` do?

It tells `ea_install_profile` to **actually apply the profile**.

### Q5. Where are EA4 profiles stored?

```
/etc/cpanel/ea4/profiles/
```

### Q6. Where are cPanel-provided profiles?

```
/etc/cpanel/ea4/profiles/cpanel/
```

### Q7. Where should custom profiles be stored?

```
/etc/cpanel/ea4/profiles/custom/
```

### Q8. What happens if you manually add a profile to the custom directory?

It will also appear in the **WHM EasyApache 4 interface**.

### Q9. Why shouldn't custom profiles be stored elsewhere?

Because profiles stored elsewhere may be **overwritten or deleted during updates**.

---


# 10. Very Important Interview Questions

### Q1. What is MPM?

**MPM stands for Multi-Processing Module.**

It handles core Apache tasks such as network connections, listening for requests, and creating processes/threads.

---

### Q2. Can multiple MPMs be loaded simultaneously?

**No.**

Only **one MPM can be loaded at a time**.

---

### Q3. Name the MPMs covered in this lesson.

```
Prefork
Event
Worker
ITK
```

---

### Q4. Which MPMs are threaded according to the course chart?

```
Event
Worker
```

Prefork and ITK are shown as non-threaded.

---

### Q5. Which MPM has the lowest resource usage in the chart?

**Event**

---

### Q6. Which MPM has high resource usage?

**Prefork and ITK**

---

### Q7. How do you check for an MPM module on a RHEL-based EA4 server?

Check:

```
ls /usr/lib64/apache2/modules | grep mpm
```

---

### Q8. How do you change Worker to Event using Yum?

```
yum shell
```

Then:

```
> remove ea-apache24-mod_mpm_worker
> install ea-apache24-mod_mpm_event
> run
```

---

### Q9. Why is `run` important in Yum shell?

`remove` and `install` **stage** the operations. `run` executes the staged operations.

---

### Q10. What package manager is used to change MPM on Ubuntu?

The lesson uses:

```
apt-get
```

---

