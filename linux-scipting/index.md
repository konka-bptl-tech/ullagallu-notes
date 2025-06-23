# Linux Introductoion
- Kernel Features
- Shell, Terminal
- Normal User and Root User
- Command Line Shortcuts
  ^+a|e|u|k|left arrow|right arrow|R
- Basic Command Sysntax
  - whoami,cmd1;cmd2,date,file,cat,head,tail,wc,Tab Completion,ls,cd,continuing command using /,history

# File System
- Fs Highrarchy
- absolute path and relative path
- pwd,touch,,mkdir,rmdir,cp,mv,echo,find
- soft and hard links
- variable defining
- command substitution
- file editing using vim and nano
- redirecting output
- $PATH,$USER,$HOMW,env,export EDITOR=nano
- ~/.bash_profile

# Mount & Umount Fs, UserManagement
- df,du,lsblk,etc/fstab,fid,locate
- Configuration Files
  - /etc/passwd,/etc/shadow,/etc/group,/etc/sudoers,sudo visudo./etc/login.defs
- useradd,id,passwd,userdel,usermod,su - ,su,sudo,groupadd,nologinshell

# File Permissions
- symbolic permissions[rwx]
- numeric permissions[421]
- chmod[u,g,o],chown
- umask for normal & root users

# Monitoring & Process Management,service management
- Commands
  top,ps aux,sleep,jobs,fg,bg,kill,w,uptime,lscpu,cat /etc/os-release,free
- systemctl list-units --type=service examples of systemctl
  - /etc/systemd/system/
- SSH /etc/ssh/ssh_config,ssh-keygen,ssh-agent
- log management
  - /var/log
- log prioritys[emerg,alert,crit,err,warning,notice,info,debug]
- log rotation
- journaling

# Networking
- ip a,ping,tracepath,ss,hostname
- /etc/resolv.conf

# Archiving & Package Management
- archiving , unarchiving 
- scp
- rpm,yum,apt,dnf,apt-get