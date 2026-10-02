# Networking Command


##  ping (Packet InterNetwork Groper)

The ping (Packet InterNetwork Groper)  command is the fundamental network diagnostics tool used to check if a host (like a server, website, or router) is online and reachable over the network.

Syntax :
```bash
ping [OPTIONS] TARGET
```

Example : 

```bash
[ec2-user@ip-172-31-38-144 ~]$ ping google.com
PING google.com (142.250.195.142) 56(84) bytes of data.
64 bytes from tzsyda-ab-in-f14.1e100.net (142.250.195.142): icmp_seq=1 ttl=116 time=0.558 ms
64 bytes from maa03s40-in-f14.1e100.net (142.250.195.142): icmp_seq=2 ttl=116 time=0.604 ms
64 bytes from maa03s40-in-f14.1e100.net (142.250.195.142): icmp_seq=3 ttl=116 time=0.584 ms
64 bytes from maa03s40-in-f14.1e100.net (142.250.195.142): icmp_seq=4 ttl=116 time=0.579 ms
64 bytes from maa03s40-in-f14.1e100.net (142.250.195.142): icmp_seq=5 ttl=116 time=0.585 ms
64 bytes from maa03s40-in-f14.1e100.net (142.250.195.142): icmp_seq=6 ttl=116 time=0.575 ms
^C
--- google.com ping statistics ---
6 packets transmitted, 6 received, 0% packet loss, time 5061ms
rtt min/avg/max/mdev = 0.558/0.580/0.604/0.013 ms


# How to read above data :

# • icmp_seq=1 (Sequence Number): The tracking number of the packet. If numbers are missing (e.g., jumping from 1 to 4), it means packets are dropping.

# • ttl=116 (Time to Live): A safety counter that drops by 1 for every router or network hop the packet passes through. It prevents packets from getting trapped in infinite loops.

# • time=12.4 ms (Latency/Round Trip Time): The exact time in milliseconds it took for the packet to go to the server and come back. Lower is faster.

# • 0% packet loss (Network Health): Tells you if any data disappeared. Any packet loss above 0% indicates a faulty connection or server congestion.
```

Common DevOps Use Cases & Options :

```bash

# 1. Limit the number of pings (-c)
# To prevent the terminal from running forever, use the -c (count) flag to stop automatically after a set number of attempts:
ping -c 5 google.com




# 2. Change the time interval between packets (-i)
# By default, ping waits 1 second between attempts. You can speed it up or slow it down.
# • Ping every 5 seconds: ping -i 5 google.com
# • Fast check (every 0.2 seconds - requires sudo): sudo ping -i 0.2 google.com



# 3. Test IPv6 addresses exclusively (-6)
# If you are working on modern cloud architectures that utilize IPv6 networking protocols:
ping -6 google.com

```
