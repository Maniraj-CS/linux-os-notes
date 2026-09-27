# Introduction to Linux

Linux is an **open-source operating system kernel**. It manages the computer's hardware and provides the foundation on which applications and other software run.

In simple words:

> **Linux = Kernel + other software/tools → complete Linux operating system**

For example, **Ubuntu, Debian, Fedora, and Rocky Linux** are Linux distributions (distros).

---

## Unix vs Linux

Linux was inspired by Unix and follows many Unix design principles, but they are not the same operating system.

### Cost and Licensing

**Unix:**
Many traditional Unix systems are proprietary and are maintained by specific vendors.

**Linux:**
Linux is open source. Its source code can be viewed, modified, and distributed under open-source licenses.

### Distributions

**Unix:**
Different Unix systems are generally associated with specific vendors, such as **IBM AIX, Oracle Solaris, and HP-UX**.

**Linux:**
Linux has many distributions, such as:

* Ubuntu
* Debian
* Fedora
* Rocky Linux
* Arch Linux

A **distribution** combines the Linux kernel with system tools, package managers, libraries, and other software.

### Hardware and Flexibility

**Unix:**
Traditionally used heavily in enterprise systems and often associated with specific hardware platforms.

**Linux:**
Highly portable and can run on:

* Servers
* Laptops and desktops
* Cloud instances
* Embedded devices
* Supercomputers

---

# Checking System Hardware and Resources

Linux provides several commands to check system resources.

| Command   | Primary Focus | Best Used For                              |
| --------- | ------------- | ------------------------------------------ |
| `top`     | CPU & RAM     | Finding processes consuming high resources |
| `free -h` | RAM           | Checking memory usage                      |
| `df -h`   | Disk space    | Checking whether a filesystem is full      |

### `top`

Shows running processes and their CPU/memory usage.

```bash
top
```

Useful when a server or application is becoming slow.

### `free -h`

Shows RAM and swap usage in a human-readable format.

```bash
free -h
```

Example:

```text
               total   used   free
Mem:            7.7Gi   3.2Gi  1.8Gi
Swap:           2.0Gi   0.0Gi  2.0Gi
```

### `df -h`

Shows available and used disk space.

```bash
df -h
```

Example:

```text
Filesystem      Size  Used Avail Use%
/dev/sda1       100G   72G   28G  73%
```

If `Use%` becomes very high, the filesystem may run out of space.

---

## Simple DevOps Rule

When a Linux server is slow, these three commands are a good starting point:

```bash
top
free -h
df -h
```

They help you quickly check:

**CPU → RAM → Disk**
