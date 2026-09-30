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

> There are more flag such as : ``-r``,``-n``,``-v``,``-m``,``-p``,``-i``,``-o`` . This flag use to show specific part from ``-a`` flag result. such as for ``-o`` means GNU/Linux (Operating System) 

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

---

### adduser 

It is a user-friendly, interactive utility used to add a new user account to a Linux system while automatically creating their home directory, prompting for a password, and setting up default configurations.

Syntex :
```bash
sudo adduser [username]
```
---

### passwd

It is a built-in Linux utility used to create, update, or lock user account passwords and manage password expiration rules.

Syntex :
```bash
sudo passwd [options] [username]
```

Some examples using options and flag :

```bash

# 1. Change your own password
passwd



# 2. Change another user's password (Administrator)

# If you need to reset a password for a coworker or a service account you must run it with administrator power. Root users do not need to know or type the user's old password:

sudo passwd [username]



# 3. Lock a user account

# If a user leaves the team or a service needs to be temporarily disabled for security, you can lock their password so they cannot log in:

sudo passwd -l [username]



# 4. Unlock a locked account

# To restore access and let the user log back in with their existing credentials, use the unlock flag:

sudo passwd -u [username]


# 5. Check password status

# To see a quick summary of an account's health—such as whether the password is locked, has an expiration date, or what encryption algorithm is used—run:

sudo passwd -S [username]

```

---

### su  (Switch User)

It is a built-in utility used to switch from your current user account to another user account during a terminal session without logging out of the server.

Syntex :
```bash
sudo su [options] [username]
```

Example : 

```bash
sudo su devops-user
```

---

### userdel (User Delete)

It is a low-level utility used to remove a user account from the system. It cleans out the user's records from the core identity files like `` /etc/passwd ``,`` /etc/shadow ``, and `` /etc/group ``.

Syntex :

```bash
sudo userdel [options] [username]
```

Example with options and flag :

```bash
# 1. Delete a user completely (Highly Recommended for DevOps)

# If an engineer leaves the company or a temporary testing account is no longer needed, you usually want to wipe out their files to save disk space. Use the -r (remove) flag:

sudo userdel -r devops-user

# 2. Force-delete a user who is currently logged in

#If a background application or an active SSH session is still running under that username, the standard userdel command will block you and print an error saying the user is currently logged in. To force the deletion anyway, add the -f flag:

sudo userdel -r -f devops-user

```

---

### groupadd  (Create a Group)

The `` groupadd `` command creates a brand-new user group on your Linux system. Groups are used to bundle users together to give them shared permissions to specific files, folders, or services.

Syntex :

```bash
sudo groupadd [OPTIONS] group_name
```

> To check the group is created or not , use this command `` cat /etc/group `` . So this the path where all group stored.

Example :

```bash
sudo groupadd devops-group
```

---

### gpasswd  (Manage Group Members & Passwords)

The `` gpasswd `` command is used to administer groups. For DevOps engineers, it is the primary tool used to add or remove users from existing groups.

Syntex :

```bash
sudo gpasswd [OPTION] username group_name
```

Example wiht Options and flag :

```bash

# 1. The -a Flag (Add a User)

# The -a (add) flag appends a single user to an existing group without disturbing any current members.

# Example: To add the user linux to the devops group

sudo gpasswd -a linux devops

# Result: The linux user is now a member of devops and inherits any permissions that group holds.



# 2. The -M Flag (Define Group Members / Mass Reset)

# The -M (members) flag defines the complete, absolute list of users belonging to the group.

# ⚠️ CRITICAL WARNING: This flag overwrites the existing group membership list. Anyone who was in the group before but is not included in your new command will be instantly removed.

# Example: To make sure only ec2-user and linux belong to the devops group

sudo gpasswd -M ec2-user,linux devops

# Result: ec2-user and linux are now the only members. If a user named john was in devops before, he is kicked out automatically.

```

---

### groupdel (Delete a Group)

The `` groupdel `` command removes an existing group from the system.

Syntex :

```bash
sudo groupdel group_name
```

> ⚠️ Essential Rule for groupdel :
> You cannot delete a primary group of an existing user. If a user account still uses that group as their main group (the default group assigned when they were created), groupdel will fail with an error. You must delete the user first, or change their primary group using usermod -g before deleting the group.

---

## File and Folder Permession

It a fundamental security system that controls who can view, modify, or run files and directories on a system .

--- 

### The Permissions Component Chart

| Permission  |	Character |	Numeric (Octal) Value |	Meaning for a File	Meaning for a Folder (Directory)  |
|---------------|----------|---------------------|---------------------------------|
| Read |	r |	4 |	View the file contents.	List the files inside the folder (ls). |
| Write |	w |	2 |	Modify or edit the file.	Create, delete, or rename files inside it. |
| Execute|	x |	1 |	Run the file as a program/script.	Enter the folder (cd) and access its files. |
| None	| - |	0 |	No permissions granted.	No access granted. |


