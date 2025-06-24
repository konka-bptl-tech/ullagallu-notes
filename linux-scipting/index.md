## ✅ **1. Linux Introduction**

* **Kernel & OS Basics**

  * Kernel vs User Space
  * Linux distributions & init systems (SysV, systemd)
* **Shell & Terminal**

  * Bash vs Zsh
  * Interactive vs non-interactive shells
* **User Types**

  * Root vs Normal user
  * `sudo` vs `su`
* **Command Line Shortcuts**

  * `Ctrl+a`, `Ctrl+e`, `Ctrl+u`, `Ctrl+k`, `Ctrl+r`, ← → arrows
* **Basic Commands & Syntax**

  * `whoami`, `date`, `file`, `cat`, `head`, `tail`, `wc`
  * Command chaining: `;`, `&&`, `||`
  * Tab Completion, History (`history`, `!n`, `!!`), Continuation (`\`)

---

## ✅ **2. File System - Part 1**

* **Filesystem Hierarchy Standard (FHS)**

  * `/`, `/bin`, `/etc`, `/var`, `/usr`, `/tmp`, `/home`, etc.
* **Navigation & Manipulation**

  * `pwd`, `touch`, `mkdir`, `rmdir`, `cp`, `mv`, `echo`, `find`
* **Paths**

  * Absolute vs Relative
* **Links**

  * Hard vs Soft links (`ln`, `ln -s`)
* **Variables**

  * Defining, exporting, scope of variables

---

## ✅ **3. File System - Part 2**

* **Command Substitution**

  * `` `command` ``, `$(command)`
* **File Editing**

  * Using `vim` and `nano`
* **I/O Redirection**

  * `>`, `>>`, `<`, `2>`, `&>`, `tee`
* **Environment Variables**

  * `$PATH`, `$USER`, `$HOME`, `$EDITOR`, `env`, `export`
* **Shell Profile Files**

  * `~/.bashrc`, `~/.bash_profile`, `~/.profile`, `/etc/profile`

---

## ✅ **4. Mounting, Filesystems, & User Management**

* **Disk & Filesystem**

  * `df`, `du`, `lsblk`, `/etc/fstab`, `mount`, `umount`, `findmnt`, `locate`
* **User Management**

  * Config files: `/etc/passwd`, `/etc/shadow`, `/etc/group`, `/etc/sudoers`, `/etc/login.defs`
  * Commands: `useradd`, `passwd`, `usermod`, `userdel`, `id`, `su`, `sudo`, `groupadd`, nologin shells
  * Secure editing: `sudo visudo`

---

## ✅ **5. File Permissions**

* **Permission Types**

  * Symbolic (`rwx`) & Numeric (`chmod 755`)
* **User Classes**

  * `u`, `g`, `o`, `a` with `chmod`
* **Ownership**

  * `chown`, `chgrp`
* **Umask**

  * Default permission masks for users

---

## ✅ **6. Process, Service, SSH & Logs**

* **Process Management**

  * `ps`, `top`, `htop`, `kill`, `jobs`, `fg`, `bg`, `nice`, `renice`, `uptime`, `sleep`
* **Service Management**

  * `systemctl`, `service`, `journalctl`, `/etc/systemd/system/`
* **SSH Basics**

  * `ssh`, `/etc/ssh/ssh_config`, `ssh-keygen`, `ssh-agent`, `~/.ssh/config`
* **Logging**

  * `/var/log`, `rsyslog`, `logrotate`, `journalctl`
  * Log levels: `emerg`, `alert`, `crit`, `err`, `warning`, `notice`, `info`, `debug`

---

## ✅ **7. Networking**

* **Basic Tools**

  * `ip a`, `ping`, `ss`, `hostname`, `tracepath`
* **DNS Configuration**

  * `/etc/resolv.conf`
* **Cron Jobs**

  * `crontab -e`, `/etc/crontab`, `cron.allow`, `cron.deny`

---

## ✅ **8. Archiving & Package Management**

* **Archiving Tools**

  * `tar`, `gzip`, `gunzip`, `zip`, `unzip`
* **File Transfer**

  * `scp`, `rsync`, `sftp`
* **Package Managers**

  * Debian-based: `apt`, `apt-get`, `dpkg`
  * RHEL-based: `yum`, `dnf`, `rpm`

---

## ✅ **9. Scripting & Data Manipulation**

* **Text Processing**

  * `grep`, `cut`, `awk`, `sed`, `tr`, `sort`, `uniq`
* **File Searching**

  * `find`, `locate`, `xargs`
* **Shell Scripting**

  * Positional parameters: `$0`, `$1`, `$@`, `$#`
  * Control Structures: `if`, `for`, `while`, `case`
  * Script arguments, exit codes

---

## ✅ **10. Linux Security**

* **SELinux**

  * Modes (`Enforcing`, `Permissive`, `Disabled`), tools (`getenforce`, `setenforce`)
  * Config: `/etc/selinux/config`
* **AppArmor**

  * Profiles, commands: `aa-status`, `aa-complain`, `aa-enforce`, `/etc/apparmor.d`

---

## ➕ **Additional Suggestions for DevOps Context**

* **System Performance Monitoring**

  * `vmstat`, `iostat`, `iotop`, `netstat`, `dstat`
* **Containers (Intro)**

  * Basic Docker commands (though not pure Linux, essential for DevOps)
* **Automation**

  * Shell scripts for automating backups, reports, monitoring

---
