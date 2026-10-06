**Lesson - 1 : Using Yum or APT to Manage your Installation
Lesson - 2 : Installing & Creating Profiles
Lesson - 3 : Multi-processing Modules (MPMs)**


# Lesson 1: Using Yum or Apt to Manage Your Installation

This lesson is about **managing EasyApache 4 packages directly from the Linux command line** using `yum` or `apt`. The course emphasizes that EA4 does not have one unified CLI like EasyApache 3. Instead, you use the normal OS package manager.

---

## 1. Main Commands

The lesson focuses on four important package-management operations:

```
yum list
yum info
yum install
yum remove
```

The equivalent `apt` commands are also covered. Pasted text

### Quick table

|Purpose|Yum|Apt|
|---|---|---|
|List packages|`yum list`|`apt list`|
|Package details|`yum info <package>`|`apt info <package>`|
|Install|`yum install <package>`|`apt install <package>`|
|Remove|`yum remove <package>`|`apt remove <package>`|

---

# 2. Listing EA4 Packages

All EasyApache packages begin with:

```
ea-
```

The `*` is a wildcard, meaning **zero or more characters**. Therefore:

```
yum list ea-*
```

can find packages such as:

```
ea-apache24
ea-apr
ea-profiles-tools
ea-php81
```

The course specifically explains that `ea-*` matches packages beginning with `ea-`. Pasted text

### Installed vs Available

Example:

```
Installed Packages
ea-apache24.x86_64    ...    @EA4
```

The `@EA4` indicates that the package is **installed**.

Packages listed below without the `@` marker are available to install.

### Ubuntu

To see only installed EA4 packages:

```
apt list ea-* --installed
```

Ubuntu marks installed packages with:

```
[installed]
```

---

# 3. `yum info`

Once you find a package, use:

```
yum info ea-apr
```

This gives detailed information about the package.

It can show:

- Package name
- Version
- Release
- Architecture
- Package size
- Repository
- Summary
- Description
- License
- Web resources

The course breaks the `yum info` output into these individual sections. 

### Easy way to remember

```
yum list
   ↓
"What packages exist?"

yum info
   ↓
"What is inside this package?"
```

---

# 4. Installing an EA4 Package

The lesson uses Apache's **mod_speling** as the example.

First, search for it:

```
yum list ea*spel*
```

The wildcards allow the search to match anything beginning with `ea-` and containing `spel`. Pasted text

The relevant package is:

```
ea-apache24-mod_speling
```

Install it with:

```
yum install ea-apache24-mod_speling
```

On Ubuntu:

```
apt install ea-apache24-mod-speling
```

You **do not need to specify the architecture** such as `.x86_64`. The package manager automatically chooses the appropriate architecture.

---

## What happens after installation?

EA4 automatically performs a **graceful Apache restart** so that the module becomes active. You don't need to completely rebuild Apache/PHP like you did with EasyApache 3. 

This is an important interview point.

> **EA4 allows individual Apache/PHP modules to be installed through the package manager without recompiling the entire Apache/PHP stack.**

---

# 5. EA4 Experimental Repository

On RHEL-based systems:

```
yum install ea4-experimental
```

After enabling it, packages from the experimental repository can be installed normally using `yum install`.

### Important warning

The experimental repository contains software that is **not yet considered ready for everyone**.

It can potentially:

- Affect server performance
- Cause compatibility problems
- Introduce instability

The course recommends **not using it on production servers** and says you use it at your own risk. Pasted text

---

# 6. The Complete Workflow

For an EA4 package, think like this:

```
             Need an EA4 package
                     |
                     v
             yum list ea-*
                     |
                     v
             Find package name
                     |
                     v
             yum info <package>
                     |
                     v
             Check package details
                     |
                     v
          yum install <package>
                     |
                     v
       EA4 installs the package
                     |
                     v
          Graceful Apache restart
```

---

# 7. Important Commands to Practice

```
# List EA4 packages
yum list ea-*

# Search for a specific package
yum list ea*spel*

# Package information
yum info ea-apr

# Install
yum install ea-apache24-mod_speling

# Remove
yum remove <package>

# Enable EA4 experimental repository
yum install ea4-experimental
```

For Ubuntu:

```
apt list ea-*

apt list ea-* --installed

apt info ea-apr

apt install ea-apache24-mod-speling
```

---
---

## What to Remember

```
EA4 = Uses OS package manager
       |
       +-- yum → RHEL-based
       |
       +-- apt → Ubuntu/Debian

yum list       → Find packages
yum info       → Package details
yum install    → Install
yum remove     → Remove

ea-*           → EA4 package pattern
*              → Zero or more characters

ea4-experimental → Experimental packages
```

**This lesson's core idea:**

> **EA4 package management is basically Linux package management. Find the package, inspect it, then install/remove it using `yum` or `apt`.**

##  Remove & Update

### 1. Remove an EA4 Package

First, verify that the package is installed:

```
yum list ea-apache24-mod_speling
```

Ubuntu:

```
apt list ea-apache24-mod-speling
```

If it appears under **Installed Packages**, remove it with:

```
yum remove ea-apache24-mod_speling
```

Ubuntu:

```
apt remove ea-apache24-mod-speling
```

The lesson's workflow is:

```
Check package
     ↓
yum list <package>
     ↓
Is it installed?
     ↓
yum remove <package>
```

---

# 2. Yum Update vs Apt Update

This is **very important for interviews**.

### RHEL-based systems

`yum update` actually **updates packages**.

```
yum update
```

### Ubuntu/Debian

`apt update` does **NOT** install updates.

```
apt update
```

It only refreshes the repository/package information.

To actually upgrade packages:

```
apt upgrade
```

The course specifically highlights this difference.

---

## 3. Update Only EA4 Packages

You can use the `*` wildcard to target only EasyApache packages:

```
yum update ea-*
```

This means:

```
yum update
    +
ea-*
    ↓
Update only packages beginning with "ea-"
```

So it does **not** mean update every package on the server.

---

## 4. Important PHP Version Concept

The course points out that PHP packages include the **minor PHP version in the package name**.

For example:

```
ea-php54
ea-php55
ea-php56
```

Therefore:

```
yum update
```

will **not automatically change PHP 5.4 → PHP 5.5**.

You would need to deliberately install/configure the newer PHP version.

### Remember

> **Package update ≠ PHP version migration**

---

# 5. Ubuntu Commands

To refresh repositories:

```
apt update
```

To upgrade everything:

```
apt upgrade
```

To upgrade only EA4 packages:

```
apt upgrade ea-*
```

You can combine repository refresh + upgrade:

```
apt update && apt upgrade
```

Or only EA4 packages:

```
apt update && apt upgrade ea-*
```

---

# Quick Revision Table

|Task|RHEL/CentOS|Ubuntu|
|---|---|---|
|Check package|`yum list <pkg>`|`apt list <pkg>`|
|Remove|`yum remove <pkg>`|`apt remove <pkg>`|
|Update packages|`yum update`|`apt upgrade`|
|Refresh repositories|Automatic with yum commands|`apt update`|
|Update only EA4|`yum update ea-*`|`apt upgrade ea-*`|

### Most important commands from this section

```
yum remove ea-apache24-mod_speling
```

```
yum update ea-*
```

```
apt update
```

```
apt upgrade ea-*
```

**Exam memory:**

> `yum update` = updates packages  
> `apt update` = refreshes package information  
> `apt upgrade` = actually applies updates.

---

# Lesson 2 : Installing & Creating Profiles

This lesson focuses on **EA4 profiles**: what they are, how to install one, and how to create a custom profile.

---

## 1. What is an EA4 Profile?

An EA4 profile is a **collection of packages** that EA4 installs together.

Think of it like a configuration blueprint:

```
EA4 Profile
     |
     +-- Apache
     +-- PHP
     +-- PHP extensions
     +-- Apache modules
     +-- Other EA4 packages
```

### Important behavior

When you install an EA4 profile:

> **EA4-related packages that are NOT listed in that profile will be removed.**

So installing a profile isn't simply "add these packages." It can also remove existing EA4 packages that aren't part of the selected profile.

**This is very important.**

---

# 2. Installing the Default cPanel Profile

The course gives this command:

```
/usr/local/bin/ea_install_profile --install /etc/cpanel/ea4/profiles/cpanel/default.json
```

Break it down:

```
/usr/local/bin/ea_install_profile
        ↓
EA4 profile installation script

--install
        ↓
Actually apply the profile

/etc/cpanel/ea4/profiles/cpanel/default.json
        ↓
Default cPanel EA4 profile
```

### Easy memory

```
ea_install_profile --install <profile.json>
```

---

## 3. What if You Don't Use `--install`?

If you run the profile command **without** `--install`, the course says it returns a **JSON-formatted list of changes** that would need to be made.

So conceptually:

```
Without --install
       ↓
Show planned changes
       ↓
No actual installation
```

With:

```
--install
```

the changes are actually applied.

---

# 4. Creating a Custom Profile

Creating a new profile from CLI is quite simple.

### Step 1: Find an existing profile

Profiles are stored under:

```
/etc/cpanel/ea4/profiles
```

cPanel-provided profiles are located under:

```
/etc/cpanel/ea4/profiles/cpanel
```

---

### Step 2: Copy an existing profile

The course says to make a copy of an existing profile.

Conceptually:

```
Existing profile
      |
      | copy
      v
Custom profile
      |
      | edit
      v
Your configuration
```

You can use a text editor such as:

```
nano
```

or:

```
vim
```

The course specifically describes `nano` and `vi/vim` as command-line text editors used to modify plain-text files.

---

# 5. Where Should Custom Profiles Go?

This is **very important**.

### cPanel-provided profiles

```
/etc/cpanel/ea4/profiles/cpanel/
```

### Custom profiles

```
/etc/cpanel/ea4/profiles/custom/
```

If the `custom` directory doesn't exist, you create it.

### Why?

The course says custom profiles should be placed in the `custom` directory because profiles stored elsewhere may be **overwritten or deleted during updates**.

---

# 6. WHM and Custom Profiles

An interesting point from the lesson:

Profiles uploaded through the **EasyApache 4 WHM interface** are stored in:

```
/etc/cpanel/ea4/profiles/custom/
```

And profiles that you manually place in that directory will also appear in the **WHM interface**.

So CLI and WHM work together:

```
                 EA4 Profiles
                     |
          +----------+----------+
          |                     |
         CLI                   WHM
          |                     |
          +----------+----------+
                     |
             custom profile
                     |
      /etc/cpanel/ea4/profiles/custom/
```

---

# 7. Practical Example

Suppose you want to create your own profile.

### Create custom directory

```
mkdir -p /etc/cpanel/ea4/profiles/custom
```

### Copy an existing profile

For example:

```
cp /etc/cpanel/ea4/profiles/cpanel/default.json \
/etc/cpanel/ea4/profiles/custom/my-profile.json
```

### Edit it

```
nano /etc/cpanel/ea4/profiles/custom/my-profile.json
```

Then install it:

```
/usr/local/bin/ea_install_profile --install \
/etc/cpanel/ea4/profiles/custom/my-profile.json
```

**Note:** The course's core instruction is to copy an existing profile and modify the copy. The exact profile JSON structure is covered by the documentation/demo rather than specified in the text you provided, so don't memorize an invented JSON structure.

---
# Must Remember

```
EA4 Profile
    ↓
Collection of packages
    ↓
Install profile
    ↓
Packages not listed in profile
    ↓
Removed
```

### Important paths

```
/etc/cpanel/ea4/profiles/
```

```
/etc/cpanel/ea4/profiles/cpanel/
```

```
/etc/cpanel/ea4/profiles/custom/
```

### Important command

```
/usr/local/bin/ea_install_profile --install \
/etc/cpanel/ea4/profiles/cpanel/default.json
```

### Core concept

> **EA4 profiles are configuration/package blueprints. cPanel profiles go in `cpanel/`, custom profiles go in `custom/`, and installing a profile can remove EA4 packages that aren't defined in that profile.**

---

#  Lesson 3 : Multi-Processing Modules (MPMs)

## 1. What is an MPM?

**MPM = Multi-Processing Module**

An MPM is a **core Apache module** responsible for important tasks such as:

- Creating network connections
- Binding Apache to ports
- Listening for client requests
- Accepting requests
- Creating child processes or threads
- Handling client requests

The important point is:

> **Only one MPM can be loaded at a time.**

EA4 prevents multiple MPMs from being installed concurrently.

---

# 2. Main MPMs

Your chart compares four MPMs:

|Feature|Prefork|Event|Worker|ITK|
|---|---|---|---|---|
|Threaded?|No|Yes|Yes|No|
|Parent runs as|nobody|nobody|nobody|root|
|Children run as|nobody|nobody|nobody|user|
|Can use DSO?|Yes|No|No|Yes|
|Thread/child per|Connection|Request|Connection|Connection|
|Resource usage|High|Lowest|Low|High|

### The easiest way to understand them

```
Prefork
  → Process-based
  → No threads
  → Higher resource usage

Worker
  → Process + threads
  → Lower resource usage

Event
  → Threaded
  → Optimized for concurrent connections
  → Lowest resource usage in the chart

ITK
  → Process-based
  → Runs children as individual users
  → Higher resource usage
```

---

# 3. How to Check Which MPM Is Installed

The lesson gives a simple method:

Check:

```
/usr/lib64/apache2/modules
```

and look for a module containing:

```
mpm
```

For example:

```
ls /usr/lib64/apache2/modules | grep mpm
```

You may see something such as:

```
ea-apache24-mod_mpm_event.so
```

That tells you the **Event MPM** is installed.

---

# 4. Changing MPM on RHEL-Based Systems

This is the most important practical section.

The course says to use the **Yum shell**.

Start it with:

```
yum shell
```

The Yum shell is a basic interactive environment.

It allows you to **stage multiple package operations first**, then execute them together with:

```
run
```

---

## Example: Worker → Event

Current MPM:

```
Worker
```

Want:

```
Event
```

Start Yum shell:

```
yum shell
```

Then:

```
> remove ea-apache24-mod_mpm_worker
```

This **does not immediately remove** the package.

It stages the removal.

Then:

```
> install ea-apache24-mod_mpm_event
```

This also stages the installation.

Finally:

```
> run
```

Now Yum executes the staged operations.

### Complete example

```
yum shell
```

```
> remove ea-apache24-mod_mpm_worker
> install ea-apache24-mod_mpm_event
> run
```

### Remember the sequence

```
yum shell
    ↓
remove old MPM
    ↓
install new MPM
    ↓
run
    ↓
Changes actually execute
```

This is a very good interview scenario.

---

# 5. Why Use the Yum Shell?

Because you don't want to end up in a state where you simply remove the existing MPM first and then deal with the replacement separately.

The Yum shell allows you to **stage the removal and installation together** and execute them sequentially.

### Key distinction

```
remove
```

inside Yum shell:

**Stage the removal**

while:

```
run
```

means:

**Execute the staged operations**

---

# 6. ITK MPM Warning

The lesson specifically warns about **ITK**.

Some MPMs, such as ITK, require additional changes to the default cPanel profile before installation.

Therefore, if you're not familiar with those requirements, the course recommends changing to ITK through the **WHM interface**, where the required changes can be presented to you.

### Interview point

> Don't blindly switch to ITK from CLI without understanding its profile and configuration requirements.

---

# 7. Changing MPM on Ubuntu

Ubuntu uses **APT**, so there is no Yum shell.

Instead, the lesson uses:

```
apt-get
```

The example command is:

```
/usr/bin/apt-get autoremove \
-o Dpkg::Options::=--force-confdef \
-o Dpkg::Options::=--force-confold \
--purge \
ea-apache24-mod-ruid2- \
ea-apache24-mod-mpm-prefork- \
ea-apache24-mod-cgi- \
ea-apache24-mod-mpm-event+ \
ea-apache24-mod-cgid+ \
ea-apache24-mod-mpm-worker-
```

This is complicated because it simultaneously tells `apt-get` which packages to remove and which package to install.

For the course example:

```
Worker
   ↓
Event
```

The output shows:

```
REMOVED:
ea-apache24-mod-mpm-worker

NEW:
ea-apache24-mod-mpm-event
```

Then Apache is restarted successfully.

---

# 8. Why the `+` and `-` Matter

In the `apt-get` command:

```
package-
```

means the package should be **removed**.

While:

```
package+
```

means the package should be **installed**.

So conceptually:

```
ea-apache24-mod-mpm-worker-
                    ↑
                  remove
```

and:

```
ea-apache24-mod-mpm-event+
                  ↑
                install
```

---

# 9. RHEL vs Ubuntu

|Task|RHEL-based|Ubuntu|
|---|---|---|
|Package manager|`yum`|`apt`|
|MPM switching method|`yum shell`|`apt-get`|
|Start|`yum shell`|`/usr/bin/apt-get ...`|
|Stage removal|`remove ...`|Package with `-`|
|Stage installation|`install ...`|Package with `+`|
|Execute staged operations|`run`|Direct execution|

---
# Final Module 2 Revision

```
                 APACHE MPM
                     |
       +-------------+-------------+
       |             |             |
    Prefork        Worker        Event
       |             |             |
    Process       Threads       Threads
       |             |             |
    High usage    Low usage   Lowest usage
```

### Must memorize

```
MPM = Multi-Processing Module
```

```
Only ONE MPM can be loaded at a time
```

```
# Check MPM
ls /usr/lib64/apache2/modules | grep mpm
```

```
# RHEL MPM change
yum shell
> remove <old-mpm>
> install <new-mpm>
> run
```

```
Worker → Event
```

```
Ubuntu → apt-get
RHEL → yum shell
```

### The big picture of Module 2

```
Module 2: Managing Apache in EA4
              |
      +-------+-------+
      |       |       |
     Yum    Profiles  MPM
      |       |       |
   Install   Create   Prefork
   Remove    Custom   Worker
   Update    Profile  Event
                     ITK
```

**Module 2 is now complete:**

1. **Using Yum/Apt** → package management
2. **Profiles** → package/configuration blueprints
3. **MPMs** → how Apache handles processes, threads, and requests

