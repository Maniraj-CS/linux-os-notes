# User and File Management System

---

## System level command

---

### uname (Unix Name)

It is an built-in utility used to print detailed information about your system's hardware architecture, hostname, and the Linux kernel.

Syntex : 
```bash
uname [options..]
```

Examples : 

```bash
[ec2-user@ip-172-31-38-144 ~]$ uname -a
Linux ip-172-31-38-144.ap-southeast-2.compute.internal 6.18.48-109.150.amzn2023.x86_64 #2 SMP PREEMPT_DYNAMIC Wed Sep 16 22:54:13 UTC 2026 x86_64 x86_64 x86_64 GNU/Linux
```
Breakdown of above command
``` text
Linux (Kernel Name) : The core engine running the computer is Linux.
ip-172-31-38-144.ap-southeast-2.compute.internal  (Network Hostname) :The specific internet ID or name of this computer inside Amazon Web Services (AWS).
6.18.48-109.150.amzn2023.x86_64 (Kernel Release) : The exact version number of the Linux engine. The amzn2023 part tells you this computer is running Amazon Linux 2023.
#2 SMP PREEMPT_DYNAMIC (Kernel Version Details) : Internal technical details about how the engine was built. It means this engine can use multiple CPU cores at the same time (SMP) and can instantly prioritize urgent tasks (PREEMPT_DYNAMIC).
Wed Sep 16 22:54:13 UTC 2026  (Build Timestamp) : The exact date and time this specific Linux engine version was created by the developers.
x86_64 (Machine Hardware Architecture) : The type of processor the computer uses. x86_64 means it is a standard 64-bit Intel or AMD processor.
x86_64 (Processor Type) : Confirms again that the CPU handles 64-bit instructions.
x86_64 (Hardware Platform) : Confirms one more time that the physical server platform is 64-bit.
GNU/Linux (Operating System) : The full official name of the operating system category.
```

> There are more flag such as : ``-r``,``-n``,``-v``,``-m``,``-p``,``-i``,``-o`` . This flag use to show spicific part from ``-a`` flag result. such as for ``-o`` means GNU/Linux (Operating System) 

---

### uptime 

It is a quick diagnostic tool used to check how long your system has been running without a reboot

Syntex : 
```bash 
uptime [options..]
```

Example :
```bash
[ec2-user@ip-172-31-38-144 ~]$ uptime
19:18:58 up 33 min,  1 user,  load average: 0.00, 0.00, 0.00

[ec2-user@ip-172-31-38-144 ~]$ uptime -p
up 37 minutes

[ec2-user@ip-172-31-38-144 ~]$ uptime -s 
2026-09-29 18:45:58 # exact timestamp when the system originally booted up
```

---

### who 

It is a built-in utility used to show which users are currently logged into the system, along with their terminal names and login times

Syntex:

```bash
who [options..]
```

Example : 

```bash
[ec2-user@ip-172-31-38-144 ~]$ who
ec2-user pts/0        2026-09-29 18:47 (192.168.1.50)
```

Break down

```text
• ec2-user (Username): The name of the user who logged into the system.
• pts/0 (Terminal Line): The virtual terminal or screen session the user is using (pts stands for pseudo-terminal).
• 2026-09-29 18:47 (Login Time): The exact date and time when the user logged in.
• (192.168.1.50) (Remote Host): The IP address or computer name the user connected from (if they logged in remotely via SSH). If blank, it means they are logged in directly at the physical machine's keyboard and monitor.
```

Usefull Options:

```bash
[ec2-user@ip-172-31-38-144 ~]$ who -H # H means Headings
NAME     LINE         TIME             COMMENT
ec2-user pts/0        2026-09-29 18:47 (27.34.65.216)

# Prints only the names of all logged-in users and a total count at the bottom
[ec2-user@ip-172-31-38-144 ~]$ who -q # q means Quick count
ec2-user
# users=1
```

---

### Whoami

It is a single-purpose utility used to print the username of the user currently logged into the active terminal session

Example :
```bash
[ec2-user@ip-172-31-38-144 ~]$ whoami
ec2-user 
```

---

### Which



---

### id

It is a built-in utility used to display the User ID (UID), Group ID (GID), and all group memberships for your current user account or a specified user.

Syntex :
```bash
id [OPTION]... [USERNAME]
```
Example :

```bash
[ec2-user@ip-172-31-38-144 ~]$ id
uid=1000(ec2-user) gid=1000(ec2-user) groups=1000(ec2-user),4(adm),10(wheel),190(systemd-journal) context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023
```

Breakdown
```text
• uid=1000(ec2-user) (User ID): Your unique digital number assigned by the system (1000) and your username (ec2-user). The root user always has a UID of 0.
• gid=1000(ec2-user) (Primary Group ID): Your primary or default group number and name. When you create files, they normally belong to this group by default.
• groups=... (Supplementary Groups): All the secondary or extra groups your user account belongs to (such as adm, wheel, or sudo). Being part of these groups gives you extra permissions to manage the computer.
```

> There are some usefull flag : ``-u``(User ID only) , ``-g`` (Primary Group ID only) , ``-G`` (All Groups) 

One-more example using flag and user-name :
```bash
[ec2-user@ip-172-31-38-144 ~]$ id -g ec2-user 
1000 # 1000 is Primary Group Id of ec2-user
```

---

### Shutdown 
It is used to safely powers off, halts, or reboots the operating system.

Syntex :
```bash
sudo shutdown [OPTIONS] [TIME] [MESSAGE]
```

Some Examples : 
```bash
# Shut down immediately
sudo shutdown now

# Reboot the system
sudo shutdown -r now

# Schedule a delayed shutdown
sudo shutdown +10 # In 10 minutes

sudo shutdown 23:30 # At 11:30 PM tonight

# Send a custom wall message
sudo shutdown +15 "Server going offline for scheduled DB maintenance. Save your work!" #We can append a custom message to warn everyone currently connected via SSH to save their progress:

# Cancel a scheduled shutdown
sudo shutdown -c

```

---

### reboot

It is a system administration utility used to safely restart the operating system and hardware immediately

Syntex :
```bash
sudo reboot [Options]
```

Usefull Options and Flag :

```text
• -f or --force (Force Reboot)
Bypasses the init system and contacts the kernel directly for an immediate restart. Warning: This skips graceful process termination and disk syncing, which can lead to data loss or file corruption. Use it only when the system is completely frozen.
• -w or --wtmp-only (Log Only)
Does not actually restart the machine. Instead, it writes a shutdown record to the /var/log/wtmp log file to simulate a reboot event for testing or auditing.
• --no-wall (Skip Warnings)
Prevents the terminal from broadcasting warning messages to other logged-in users before terminating their sessions
```
---

### package manager
A package manager is essentially an app store for our Linux server.

The prominent Linux package managers and the specific distributions they belong to:
```text
apt : Ubuntu, Debian, Linux Mint
dnf :  Amazon Linux, RedHat (RHEL), CentOS, Fedora
yum (Yellowdog Updater, Modified) : Older versions of RedHat (RHEL 6/7), CentOS 6/7, Amazon Linux 1 & 2
pacman : Arch Linux, Manjaro, EndeavourOS
portage : Gentoo Linux
```
---

## User and Group Management commands

---

### sudo (Superuser Do)
It is a tool that allows a regular user to run a specific program or command with the temporary administrative security privileges of another user, usually the superuser or ``root``.

