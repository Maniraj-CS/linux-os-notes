# Linux Shell Commands

Linux commands are used to **manage files, directories, processes, system resources, and applications** from the terminal.

---

## Basic Commands

### `date`

Shows the current date and time.

```bash
date
```

---

### `mkdir`

Creates a directory.

```bash
mkdir devops
```

Create multiple directories:

```bash
mkdir linux docker aws
```

---

### `ls`

Lists files and directories.

```bash
ls
```

Useful options:

```bash
ls -l      # detailed information
ls -a      # includes hidden files
ls -la     # detailed information + hidden files
```

---

### `pwd`

Shows the current working directory.

```bash
pwd
```

Example:

```text
/home/ec2-user
```

---

### `touch`

Creates an empty file if it does not exist.

```bash
touch hello.txt
```

It can also update the file's timestamp if the file already exists.

---

### `cd`

Changes the current directory.

```bash
cd devops
```

Go one level up:

```bash
cd ..
```

Go to the home directory:

```bash
cd ~
```

---

### `rm`

Removes files.

```bash
rm hello.txt
```

Force removal without asking for confirmation:

```bash
rm -f hello.txt
```

Remove a directory and everything inside it:

```bash
rm -r devops
```

Forcefully remove a directory and its contents:

```bash
rm -rf devops
```

> **Be careful with `rm -rf`** because deleted files normally cannot be recovered easily.

For an empty directory:

```bash
rmdir devops
```

---

### `cat`

Displays the contents of a file.

```bash
cat hello.txt
```

It can also combine files:

```bash
cat file1.txt file2.txt
```

---

### `echo`

Prints text to the terminal.

```bash
echo "Hello Linux"
```

Write text to a file:

```bash
echo "Hello Linux" > hello.txt
```

`>` **overwrites** the existing file.

To append instead:

```bash
echo "Another line" >> hello.txt
```

If the file does not exist, `>` and `>>` create it.

---

## Reading Files

### `head`

Displays the first **10 lines** of a file by default.

```bash
head hello.txt
```

Show the first 5 lines:

```bash
head -n 5 hello.txt
```

Show the first 20 lines:

```bash
head -n 20 hello.txt
```

---

### `tail`

Displays the last **10 lines** of a file by default.

```bash
tail hello.txt
```

Show the last 5 lines:

```bash
tail -n 5 hello.txt
```

> `tail hello.txt 5` is not the recommended syntax. Use `tail -n 5 hello.txt`.

---

### `tail -f`

Continuously monitors the end of a file.

This is especially useful for **application and server logs**.

```bash
tail -f app.log
```

If new lines are added to `app.log`, they appear automatically in the terminal.

Stop monitoring:

```text
Ctrl + C
```

---

### `more`

Displays a file page by page.

```bash
more hello.txt
```

Useful for reading large files, but it has fewer features than `less`.

---

### `less`

Reads large files efficiently and allows you to move forward and backward.

```bash
less app.log
```

Useful for large log files.

Common keys:

```text
Space → next page
b     → previous page
/word → search
q     → quit
```

---

### `zcat`

Displays the contents of a **gzip-compressed (`.gz`) file**.

```bash
zcat app.log.gz
```

> Important: `.gz` is gzip compression, not the same as a `.zip` archive.

---

# Intermediate Commands

## `cp`

Copies a file.

```bash
cp file.txt backup.txt
```

Copy a file to another directory:

```bash
cp file.txt /tmp/
```

Copy a directory and its contents:

```bash
cp -r devops /tmp/
```

---

## `mv`

Moves a file or directory.

```bash
mv file.txt /tmp/
```

It is also commonly used to rename files.

```bash
mv old.txt new.txt
```

### `mv -v`

Shows what is being moved.

```bash
mv -v file.txt /tmp/
```

---

## `wc`

Counts lines, words, and bytes in a file.

```bash
wc hello.txt
```

Example output:

```text
10  25  150 hello.txt
```

Meaning:

```text
10   → lines
25   → words
150  → bytes
```

Useful options:

```bash
wc -l hello.txt    # lines
wc -w hello.txt    # words
wc -c hello.txt    # bytes
```

---

# Hard Link and Soft Link

Links allow another filename/path to point to an existing file.

## Soft Link (Symbolic Link)

```bash
ln -s original.txt shortcut.txt
```

A soft link works like a **shortcut**.

```text
original.txt
     ↑
     |
shortcut.txt
```

If the original file is deleted, the symbolic link becomes **broken**.

Check it with:

```bash
ls -l
```

You may see:

```text
shortcut.txt -> original.txt
```

---

## Hard Link

```bash
ln original.txt backup.txt
```

A hard link is another directory entry pointing to the **same file data/inode**.

If the original filename is deleted, the hard link can still access the data:

```bash
rm original.txt
cat backup.txt
```

The data remains as long as at least one hard link exists.

> Hard links normally cannot be created for directories and cannot cross filesystems.

### Easy Difference

| Soft Link               | Hard Link                                                |
| ----------------------- | -------------------------------------------------------- |
| Points to a path        | Points to the same inode/data                            |
| Can link to directories | Normally files only                                      |
| Can cross filesystems   | Cannot cross filesystems                                 |
| Can become broken       | Does not become broken when original filename is deleted |

---

## `ls -ltr`

Shows files with detailed information and sorts them by modification time.

```bash
ls -ltr
```

Meaning:

```text
-l → long/detailed format
-t → sort by modification time
-r → reverse the order
```

So `ls -ltr` is useful for finding **older files first**.

---

## `cut`

Extracts specific parts from each line.

For characters/bytes:

```bash
cut -b 1 file.txt
```

Print the first byte/character from each line.

```bash
cut -b 1-4 file.txt
```

Print bytes 1 through 4.

Example:

```text
hello
world
```

```bash
cut -b 1-3 file.txt
```

Output:

```text
hel
wor
```

---

## `tee`

Displays output on the terminal **and** writes it to a file.

```bash
echo "hello" | tee hello.txt
```

Output:

```text
hello
```

At the same time, `hello.txt` contains:

```text
hello
```

By default, `tee` overwrites the file.

To append:

```bash
echo "new line" | tee -a hello.txt
```

This is useful when you want to **see command output and save it at the same time**.

---

## `sort`

Sorts lines alphabetically by default.

```bash
sort hello.txt
```

Example:

```text
z
a
c
b
```

Output:

```text
a
b
c
z
```

Reverse order:

```bash
sort -r hello.txt
```

---

## `clear`

Clears the terminal screen.

```bash
clear
```

Shortcut:

```text
Ctrl + L
```

---

## `diff`

Compares two files and shows their differences.

```bash
diff file1.txt file2.txt
```

If the files are identical, normally no output is shown.

Very useful for comparing:

```text
configuration files
code files
deployment files
```

---

# Advanced Commands

## `ssh`

SSH (**Secure Shell**) is used to securely connect to a remote Linux machine.

```bash
ssh username@server_ip
```

Example:

```bash
ssh ec2-user@54.123.45.67
```

After connecting, commands run on the **remote server**, not your local machine.

---

# Disk Usage

## `du`

`du` (**disk usage**) shows how much disk space files and directories are using.

```bash
du
```

Example:

```text
4       ./.ssh
0       ./devops
0       ./cloud
16      .
```

The default unit is usually **KiB blocks**, so human-readable output is easier to understand.

### `du -h`

Human-readable sizes.

```bash
du -h
```

Example:

```text
4.0K    ./.ssh
12K     ./devops
500M    ./logs
```

### `du -sh`

Shows only the total size.

```bash
du -sh logs
```

Example:

```text
500M    logs
```

`-s` → summary
`-h` → human-readable

### `du -ah`

Shows files as well as directories.

```bash
du -ah logs
```

`-a` → all files and directories
`-h` → human-readable

### `--max-depth`

Controls how many directory levels are displayed.

```bash
du -h --max-depth=1 .
```

This shows the size of the current directory and its immediate subdirectories.

To find the largest directories:

```bash
du -h --max-depth=1 . | sort -hr
```

Here:

```text
-h → human-readable
-r → reverse sort
```

Because `sort -h` understands sizes such as `10K`, `2M`, and `5G`.

---

# Processes

A **process** is a running program/application in Linux.

## `ps`

Shows a snapshot of running processes.

```bash
ps
```

For processes from the current terminal:

```bash
ps
```

For a more complete view:

```bash
ps aux
```

`ps` gives a **static snapshot**, while `top` continuously updates the process information.

---

## `fuser`

Shows which processes are using a file, directory, or port.

For a directory:

```bash
fuser /var/log
```

For a network port:

```bash
fuser -n tcp 8080
```

This is useful when a port is already being used and you need to find the responsible process.

---

# `kill`

Sends a signal to a process.

```bash
kill <PID>
```

The default signal is `SIGTERM` (15), which asks the process to terminate gracefully.

```bash
kill -15 <PID>
```

If the process does not stop, `SIGKILL` can force it to stop:

```bash
kill -9 <PID>
```

> Prefer `kill` or `kill -15` first. Use `kill -9` only when necessary because the process does not get a chance to clean up properly.

List available signals:

```bash
kill -l
```

---

# `nohup`

`nohup` allows a command to continue running even after the terminal/session is disconnected.

```bash
nohup <command> &
```

By default, output is written to:

```text
nohup.out
```

Example:

```bash
nohup python app.py &
```

The application can continue running after you close the SSH session.

You can view the output with:

```bash
cat nohup.out
```

A better production-style example is to explicitly specify the log file:

```bash
nohup python app.py > app.log 2>&1 &
```

Here:

```text
> app.log   → save normal output
2>&1        → send errors to the same file
&           → run in background
```

> `nohup` is useful for simple background tasks, but production applications are normally managed with tools such as `systemd`, Docker, or Kubernetes.

---

# `vmstat`

`vmstat` (**virtual memory statistics**) gives a summary of:

* Processes
* Memory
* Swap
* Disk I/O
* CPU activity

```bash
vmstat
```

Example:

```text
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 0  0      0 560284   2168 214720    0    0    85     7   40   43  1  0 99  0  0
```

Some important columns:

```text
r  → runnable processes
free → free memory
si/so → swap in/out
bi/bo → block input/output
us → user CPU time
sy → system CPU time
id → idle CPU time
wa → CPU waiting for I/O
```

For continuous monitoring:

```bash
vmstat 2
```

This updates every **2 seconds**.

---

# Quick DevOps Command Map

| Need                    | Command   |
| ----------------------- | --------- |
| Current directory       | `pwd`     |
| List files              | `ls -la`  |
| Create directory        | `mkdir`   |
| Create file             | `touch`   |
| Read file               | `cat`     |
| Read large file         | `less`    |
| First lines             | `head`    |
| Last lines              | `tail`    |
| Monitor logs            | `tail -f` |
| Copy                    | `cp`      |
| Move/Rename             | `mv`      |
| Disk space              | `df -h`   |
| Directory size          | `du -sh`  |
| Processes               | `ps aux`  |
| Live processes          | `top`     |
| Find process using port | `fuser`   |
| Stop process            | `kill`    |
| Remote server           | `ssh`     |
| Compare files           | `diff`    |
| Run after logout        | `nohup`   |
| System statistics       | `vmstat`  |
