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

---

## netstat (Network Statistics)

It is a classic command-line tool used to print network connections, routing tables, and interface statistics.

Syntax :

```bash
netstat [OPTIONS]
```

The Most Famous Combination: `` netstat -tulnp ``:

DevOps engineers usually combine letters to find out which services or applications are listening on specific ports of a server.

```bash
netstat -tulnp
```

Breakdown of Flags in `` netstat -tulnp `` :

```text
• -t (TCP): Shows only TCP network connections.

• -u (UDP): Shows only UDP network connections.

• -l (Listening): Shows only ports that are actively waiting for incoming connections (like a web server or database).

• -n (Numeric): Shows numerical IP addresses and port numbers instead of resolving hostnames or service names.

• -p (Process/Program): Displays the Process ID (PID) and the name of the program that owns the network port (requires sudo).
```

Example :

When we run `` netstat -tulnp ``, We see a table of active listeners:

```text
Proto Recv-Q Send-Q Local Address    Foreign Address    State       PID/Program name
tcp        0      0 0.0.0.0:80        0.0.0.0:*         LISTEN      1234/nginx
```

Breakdown :

• `` Proto ``: The protocol used (TCP or UDP).
• `` Local Address ``: The IP address and port number the server is listening on (`` 0.0.0.0:80 `` means it listens on port 80 across all network interfaces).
•`` State ``: The connection state, where `` LISTEN `` means the service is ready and waiting for clients to connect.
• `` PID/Program name ``: The application ID and name (e.g., `` 1234/nginx ``), telling you exactly which process is using that port.


---

## ifconfig (Interface Configuration)

It is a traditional networking utility used to view and change IP addresses, netmasks, and active states of network interface cards.

Syntax :

```bash
ifconfig [interface] [options]
```

• Running `` ifconfig `` by itself will display all active network interfaces currently connected to your system.
• Running `` ifconfig eth0 `` will show details for just that specific interface `` (eth0).


**Example Output and Breakdown**
When you run `` ifconfig ``, you see blocks of text for each interface:

```text
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.31.38.144  netmask 255.255.0.0  broadcast 172.31.63.255
        inet6 fe80::8cff:fe04:1234  prefixlen 64  scopeid 0x20<link>
        ether 06:11:22:33:44:55  txqueuelen 1000  (Ethernet)
        RX packets 12450  bytes 1420500 (1.4 MB)
        RX/TX stats here...
```

__Here is what the core lines mean:__

• `` eth0 ``: The name of the network interface card (Ethernet 0).

• `` UP,BROADCAST,RUNNING ``: Shows that the interface is active and ready to send data.

• `` inet 172.31.38.144 ``: The IPv4 address assigned to this interface.

• `` netmask 255.255.0.0 ``: The subnet mask defining your local network range.

• `` ether 06:11:22... ``: The physical MAC address of the network card.

• `` RX / TX packets ``: Shows the count of received (RX) and transmitted (TX) data packets.


**Common Use Cases & Options**

```bash
# 1. Enable or disable a network interface (requires sudo)
# If you need to restart a flaky network card, you can turn it off and on:
# • Bring down the interface: 
sudo ifconfig eth0 down
# • Bring up the interface: 
sudo ifconfig eth0 up


# 2. Assign a temporary IP address
# You can manually force an IP address onto an interface on the fly:
sudo ifconfig eth0 192.168.1.50 netmask 255.255.255.0
# (Note: This change is temporary and will disappear the next time the server reboots).

```

---

## tracepath vs traceroute

`` tracepath `` runs for normal users without needing administrator (root) privileges and checks the maximum packet size (Path MTU), whereas `` traceroute `` requires root permissions on many systems and offers more advanced protocol choices like `` TCP `` or `` ICMP ``.

1. `` traceroute `` **Output Explained (Line-by-Line)**

When you run `` traceroute google.com ``, the output looks like this:

```text
hop count ──► 1  192.168.1.1 (192.168.1.1)  2.124 ms  1.854 ms  1.720 ms
              2  10.0.0.1 (10.0.0.1)  5.412 ms  4.910 ms  4.850 ms
              3  * * *
```

• `` 1  ``**(Hop Number)**: This is the first device your data hit (usually your local Wi-Fi router).

• `` 192.168.1.1 (192.168.1.1) `` **(Router Identity)**: The hostname and the IP address of that router.

• `` 2.124 ms  1.854 ms  1.720 ms `` **(Three Response Times)**: By default, traceroute sends 3 separate test packets to each hop. These numbers show how many milliseconds each packet took to go there and back.

•``  * * *  ``**(The Mystery Hop)**: If you see asterisks, it means that specific router did not reply. This is usually because a firewall at that hop is configured to ignore trace requests for security reasons. It doesn't always mean the network is broken.


2. `` tracepath `` **Output Explained (Line-by-Line)**

When you run `` tracepath google.com ``, the output looks slightly different because it tracks packet size **(PMTU)**:

```text
hop count ──► 1?: [LOCALHOST]                      pmtu 1500
              1:  192.168.1.1                      0.854ms 
              2:  10.0.0.1                         3.421ms 
              3:  no reply
```

• `` 1?: [LOCALHOST] pmtu 1500 ``: This first line defines your own computer's settings. It shows that your network card is set to a Maximum Transmission Unit **(MTU)** of `` 1500 `` bytes (the maximum size a single data packet can be).

• `` 1:  192.168.1.1  0.854ms ``: The first hop router IP address and the round-trip latency time. Notice tracepath only shows one time metric instead of three.

• `` no reply ``: This is tracepath's version of `` * * * ``. It means the router timed out or ignored the packet.