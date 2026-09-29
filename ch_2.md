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