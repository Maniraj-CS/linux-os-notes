# User and File Management System

## System level command

---

### &bull; uname (Unix Name)

It is an built-in utility used to print detailed information about your system's hardware architecture, hostname, and the Linux kernel.

Syntax : 
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

### &bull; uptime 

It is a quick diagnostic tool used to check how long your system has been running without a reboot

Syntax : 
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

### &bull; who 

It is a built-in utility used to show which users are currently logged into the system, along with their terminal names and login times

Syntax:

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

### &bull; Whoami

It is a single-purpose utility used to print the username of the user currently logged into the active terminal session

Example :
```bash
[ec2-user@ip-172-31-38-144 ~]$ whoami
ec2-user 
```

---

### &bull; Which



---

### &bull; id

It is a built-in utility used to display the User ID (UID), Group ID (GID), and all group memberships for your current user account or a specified user.

Syntax :
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

### &bull; Shutdown 
It is used to safely powers off, halts, or reboots the operating system.

Syntax :
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

### &bull; reboot

It is a system administration utility used to safely restart the operating system and hardware immediately

Syntax :
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

### &bull; package manager
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

### &bull; sudo (Superuser Do)
It is a tool that allows a regular user to run a specific program or command with the temporary administrative security privileges of another user, usually the superuser or ``root``.

---

### &bull; adduser 

It is a user-friendly, interactive utility used to add a new user account to a Linux system while automatically creating their home directory, prompting for a password, and setting up default configurations.

Syntax :
```bash
sudo adduser [username]
```
---

### &bull; passwd

It is a built-in Linux utility used to create, update, or lock user account passwords and manage password expiration rules.

Syntax :
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

### &bull; su  (Switch User)

It is a built-in utility used to switch from your current user account to another user account during a terminal session without logging out of the server.

Syntax :
```bash
sudo su [options] username
```

Example : 

```bash
sudo su devops-user
```

---

### &bull; userdel (User Delete)

It is a low-level utility used to remove a user account from the system. It cleans out the user's records from the core identity files like `` /etc/passwd ``,`` /etc/shadow ``, and `` /etc/group ``.

Syntax :

```bash
sudo userdel [options] username
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

### &bull; groupadd  (Create a Group)

The `` groupadd `` command creates a brand-new user group on your Linux system. Groups are used to bundle users together to give them shared permissions to specific files, folders, or services.

Syntax :

```bash
sudo groupadd [OPTIONS] group_name
```

> To check the group is created or not , use this command `` cat /etc/group `` . So this the path where all group stored.

Example :

```bash
sudo groupadd devops-group
```

---

### &bull; gpasswd  (Manage Group Members & Passwords)

The `` gpasswd `` command is used to administer groups. For DevOps engineers, it is the primary tool used to add or remove users from existing groups.

Syntax :

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

### &bull; groupdel (Delete a Group)

The `` groupdel `` command removes an existing group from the system.

Syntax :

```bash
sudo groupdel group_name
```

> ⚠️ Essential Rule for groupdel :
> You cannot delete a primary group of an existing user. If a user account still uses that group as their main group (the default group assigned when they were created), groupdel will fail with an error. You must delete the user first, or change their primary group using usermod -g before deleting the group.

---

## File and Folder Permession

It a fundamental security system that controls who can view, modify, or run files and directories on a system .

--- 

### &bull; The Permissions Component Chart

| Permission  |	Character |	Numeric (Octal) Value |	Meaning for a File	Meaning for a Folder (Directory)  |
|---------------|----------|---------------------|---------------------------------|
| Read |	r |	4 |	View the file contents.	List the files inside the folder (ls). |
| Write |	w |	2 |	Modify or edit the file.	Create, delete, or rename files inside it. |
| Execute|	x |	1 |	Run the file as a program/script.	Enter the folder (cd) and access its files. |
| None	| - |	0 |	No permissions granted.	No access granted. |

---

### &bull; 🗺️ Full Bash Code Binary Mapping Diagram

```bash
 File Type
   │
   ▼   [ OWNER (u) ]       [ GROUP (g) ]       [ OTHERS (o) ]
 ┌───┐ ┌───┬───┬───┐     ┌───┬───┬───┐     ┌───┬───┬───┐
 │ - │ │ r │ w │ x │     │ r │ - │ x │     │ r │ - │ - │  <-- Symbolic Code
 └───┘ └───┴───┴───┘     └───┴───┴───┘     └───┴───┴───┘
   │     │   │   │         │   │   │         │   │   │
   │     ▼   ▼   ▼         ▼   ▼   ▼         ▼   ▼   ▼
   │     4 + 2 + 1         4 + 0 + 1         4 + 0 + 0   <-- Mathematical Weights
   │     └───┬───┘         └───┬───┘         └───┬───┘
   │         ▼                 ▼                 ▼
   │         7                 5                 4       <-- Octal Code (Absolute Mode)
   │
   └─► (-) Regular File  |  (d) Directory  |  (l) Symbolic Link
```
---

### &bull; The Common Permission Combinations

Master Permission & Binary Conversion Chart
Below is the absolute truth table for how Linux maps binary inputs directly into system operations.


| Octal Value |	Binary Bits |	Symbolic Code |	Technical Definition & Access Level |
|-------------|-------------|-----------------|-----------------------------|
| 0	| 000	| --- |	No Permissions: Completely locked down. No user in this tier can view, edit, or interact with the resource.|
| 1	| 001	| --x |	Execute Only: Users can run binary files or scripts as programs, but they cannot open the file to inspect the raw code inside. |
| 2	| 010	| -w- |	Write Only: Users can modify file contents or write logs, but they cannot read the existing file data (rarely used). |
| 3	| 011	| -wx |	Write & Execute: Users can edit and execute a file, but cannot view it (commonly applied to secure system directories). |
| 4	| 100	| r-- |	Read Only: Pure safety mode. Users can open and view data or configurations, but cannot make alterations or execute them. |
| 5	| 101	| r-x |	Read & Execute: Standard for public executables and searchable folders. Users can browse directories (ls) and launch applications. |
| 6	| 110	| rw- |	Read & Write: Standard for development assets and user documents. You can read and save modifications, but cannot execute it. |
| 7	| 111	| rwx |	Full Access: Absolute control. The target tier can read, write, modify, delete, and execute without any kernel restrictions. |

---

### &bull; chmod (Change Mode)

The chmod (Change Mode) command in Linux is a system utility used to modify the read, write, and execute permissions of files and directories.

Syntax : 

```bash
chmod [OPTIONS] MODE FILE
```

### The Two Ways to Use `` chmod ``

1) The Numeric (Octal) Method

```bash
sudo chmod 777 hello.txt  # giving read,write and execute for all user, group, and others
```

2) The Symbolic Method

```bash
# You use letters and symbols to add or remove specific permissions without rewriting the whole code.

# • Target letters: u (user/owner), g (group), o (others), a (all).
# • Action operators: + (add permission), - (remove permission), = (set exact permission).

sudo chmod u+x hello.txt # Add execute power for the file owner.

sudo chmod g-w hello.txt # 	Remove write power from the group.

sudo chmod o=r hello.txt # 	Set others to read-only strictly.

sudo chmod ug+rw hello.txt # Add read and write for both user and group.

sudo chmod ugo=rwx hello.txt # Add read, write and execute for all user, group and others
```

Some usefull Options and flag :

```bash

# -R or --recursive

# Applies the permission change to a folder and every single file and subfolder inside it.

sudo chmod -R 777 /var/www/html

# -v or --verbose

# Prints a confirmation message on the screen for every file it updates.

sudo chmod -v 755 /var/www/html

```

---

### &bull; umask (User File Creation Mask)

It is a built-in shell command that acts as a permission filter. It dictates the default access rights assigned to files and folders the exact moment they are created.

Syntax and Example :

```bash
[ec2-user@ip-172-31-38-144 cloud]$ umask
0022  # It means that newly created files get 644 permissions and newly created directories get 755 permissions
```
---

### &bull; chown (Change Owner)
 
It is the primary Linux command used to reassign the user owner and/or group owner of a file or directory to a different account .


Syntax :

```bash
chown [options] new_owner[:new_group] target_file
```


|Command Example |	What it Does |
|----------------|---------------|
| chown alice report.txt |	Changes only the user owner to alice. |
| chown alice:developers report.txt | 	Changes the user owner to alice and the group to developers. |
| chown :developers report.txt	 | Changes only the group owner to developers (leaving the user owner the same). |
| chown alice: | 	Changes user owner to alice and updates the group to alice's default login group. |


Key Flags and Options

```text
• -R (Recursive): Applies the ownership change to a directory and everything inside it (all subfolders and files).
	• Example: sudo chown -R alice:developers /var/www/html


• -v (Verbose): Prints a message for every file it successfully changes, letting you track progress in real-time.


• --reference=FILE: Copies the user and group ownership from a reference file instead of typing them out manually.
	• Example: chown --reference=template.txt newfile.txt
```

---

### &bull; chgrp (Change Group)

It is a dedicated Linux command used specifically to change the group ownership of a file or directory without altering the user owner.

Syntax :

```bash
chgrp [options] new_group target_file
```

Example :

```bash 
chgrp developers report.txt
```
---

## Compression Command

---

### &bull; zip and Unzip

It is a utilities used to compress multiple files and folders into a single .zip archive and extract files back out of that archive.

Syntax :

```bash
zip [OPTIONS] archive_name.zip file1 file2 folder/
unzip archive_name.zip
```

Example with Options and flag :

```bash

# Compress a single file :
zip archive.zip myfile.txt


# Compress an entire folder (Recursive) :
zip -r myproject.zip project_folder/


# Create a password-protected zip file :
zip -e secure.zip sensitive_data.txt


# Extract files into your current folder :
unzip myproject.zip

# Extract files into a specific target folder :
# Use the -d flag to send the extracted contents to another path:
unzip myproject.zip -d /var/www/html/

# List the contents of a zip file without extracting it:
unzip -l myproject.zip
```
---

### &bull; tar

The standard utility used by DevOps engineers to bundle multiple files and directories into a single archive file (often called a "tarball"). 


Syntax :
```bash
tar [OPTIONS] archive_name.tar target_files_or_folders
```

The 4 Master Flags You Need to Remember:

You usually combine flags together to get the desired result. Here are the four primary action flags:

```text
• -c : Create a new archive.
• -x : Extract an existing archive.
• -v : Verbose mode (shows the files on screen while archiving).
• -f : File name specification (tells tar the next text string is the archive name). Note: This flag must always come last in the options block.
```

Examples :

```bash

# 1. Create a Standard Archive (No Compression)
# Groups files into a .tar container. It does not save disk space, but makes moving data simple.
tar -cvf backup.tar /var/www/html/


# 2. Create a Gzipped Archive (High Compression - Recommended)
# Adds the -z flag to run the files through the gzip compression tool. This creates a much smaller .tar.gz or .tgz file.
tar -czvf backup.tar.gz /var/www/html/


# 3. Extract a compressed .tar.gz file
# Swaps out the create flag (-c) for the extract flag (-x). It uncompresses and unpacks everything into your current directory.
tar -xzvf backup.tar.gz


# 4. Extract to a Specific Target Directory
# Use the -C flag followed by a target path to extract the files somewhere else instead of your active folder:
sudo tar -xzvf backup.tar.gz -C /opt/myapp/

# 5. List Contents Without Extracting
# If you want to view what files are inside the archive without unpacking them, use the -t flag:
tar -tzvf backup.tar.gz
```

---

## File Transfer Command

---

### &bull; scp (Secure Copy Protocol)

It is a command-line utility used to securely copy files and directories between different computers over a network. It runs on top of `` SSH (Secure Shell) ``, meaning all data transfers are completely encrypted.

Syntax :

```bash
scp [OPTIONS] [SOURCE] [DESTINATION]
```

Example : 
```bash

# Upload using an SSH Identity Key (.pem file)
scp -i "linux-for-devops.pem" app.tar.gz ec2-user@54.252.73.197:/home/ec2-user/


# Copy a remote file to your local computer (Download)
scp -i "linux-for-devops.pem" ec2-user@54.252.73.197:/home/ec2-user/ .


#Copy an entire folder (Recursive)
scp -i "linux-for-devops.pem" -r ./my-project ec2-user@54.252.73.197:/home/ec2-user/

```

---

### &bull; rsync (Remote Sync)

It is smarter, it checks both sides and only transfers the specific blocks of data that changed, making it significantly faster for large folders or repeated backups.


Syntax :

```bash
rsync [OPTIONS] SOURCE DESTINATION
```

Key Flags You Need to Know:

DevOps engineers almost always combine flags into the popular -avz combination:

```text
• -a (Archive mode): Preserves file permissions, timestamps, symbolic links, owner/group info, and runs recursively. (Equivalent to -rlptgoD).
• -v (Verbose): Prints details of what files are being transferred on the screen.
• -z (Compress): Compresses file data during transmission over the network to save bandwidth.
• -P (Progress + Partial): Shows a progress bar during transfer and allows resuming interrupted downloads.
```

Example with Some useCash :

```bash

# 1. Sync files locally (Backup folder)
#To back up your documents folder to an external drive or backup directory:
rsync -av /home/ec2-user/docs /mnt/backup/docs


# 2. Sync files to a remote server via SSH (Upload)
#To upload a project folder to an AWS EC2 instance using a private .pem key:
rsync -avz -e "ssh -i linux-for-devops.pem" ./my-project/ ec2-user@54.252.73.197:/home/ec2-user/my-project/
# (Trailing slashes / matter in rsync: ./my-project/ copies the contents inside the folder, while omitting the slash copies the folder itself).


# 3. Mirror folders and delete extra files (--delete)
# If a file was deleted from your source folder, you might want it deleted from the destination backup too. Adding --delete makes the destination an exact mirror of the source:
rsync -av --delete /source_folder/ /destination_backup/
# Warning: Use --delete with caution so you don't accidentally wipe out valuable data on your destination.



# 4. Dry Run (--dry-run or -n)
# If you want to test an rsync command to see what files would be copied without actually moving any data:
rsync -avn --delete /source/ /destination/

```

