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

• `` 1?: [LOCALHOST] pmtu 1500 `` : This first line defines your own computer's settings. It shows that your network card is set to a Maximum Transmission Unit **(MTU)** of `` 1500 `` bytes (the maximum size a single data packet can be).

• `` 1:  192.168.1.1  0.854ms `` : The first hop router IP address and the round-trip latency time. Notice tracepath only shows one time metric instead of three.

• `` no reply `` : This is tracepath's version of `` * * * `` . It means the router timed out or ignored the packet.

---

## mtr (My Traceroute)

It is an advanced network diagnostic utility that combines the real-time tracking of `` traceroute `` with the continuous performance testing of `` ping  `` into a single live-updating interface.

Syntax :

```bash
mtr [OPTIONS] HOSTNAME_OR_IP
```

---

## nslookup (Name Server Lookup)

It is a classic network administration tool used to query **DNS (Domain Name System)** servers. It helps you discover the IP address associated with a domain name, or perform a reverse lookup to find the domain name associated with an IP address.

Syntax :

```bash
nslookup [DOMAIN_NAME]
```

**Example Output and Breakdown**


If you run nslookup google.com in your terminal, the output looks like this:

```text
Server:		172.31.0.2
Address:	172.31.0.2#53

Non-authoritative answer:
Name:	google.com
Address: 142.250.71.46
```

__Here is what the information means line by line:__

• `` Server & Address `` **(Top Section)** : This is the IP address of the local DNS server your computer asked to find the answer (e.g., `` 172.31.0.2 `` ). The `` #53 `` indicates it connected over standard network port 53, which is reserved for DNS traffic.

• `` Non-authoritative answer `` : This means the DNS server you asked doesn't actually own the original domain records for Google. Instead, it pulled a copy of the answer out of its temporary memory cache to give it to you quickly.

• `` Name & Address `` **(Bottom Section)** : The final resolved answer. It tells you that the website domain google.com points directly to the physical server IP address `` 142.250.71.46 `` .

---

## telnet

It is a legacy network utility used to establish a raw, text-based TCP connection to a specific IP address and port on a remote server.

While it was originally designed for remote administrative login, it transmits all data and passwords in plain text. Because this is a major security risk, modern systems use ssh for remote logins instead. However, DevOps engineers still use telnet as a quick troubleshooting tool to test whether a specific network port is open and accepting traffic.

Syntax :

```bash 
telnet [host] [port]
```

**Common Examples & Use Cases**

```bash
# 1. Test if a specific TCP port is open

# If your application cannot connect to a database on port 5432 or a web service on port 80, you can run telnet followed by the host and port number:
telnet 172.31.38.144 80

# • Success Output: If the screen clears or shows a connection message like Connected to 172.31.38.144, the port is open and your network path is clear.

# • Failure Output: If it hangs on Trying 172.31.38.144... and eventually says Connection timed out or Connection refused, a firewall or security group is blocking the traffic, or the service is offline.



# 2. Interact with a service manually (HTTP Example)

# You can type raw HTTP commands directly into an active telnet session to talk to a web server:
telnet example.com 80

# Once connected, type GET / HTTP/1.1, press Enter twice, and the server will return the raw HTML headers and page code. Press Ctrl + ] and then type quit to exit the telnet prompt.
```

---

## hostname 

It is a core utility used to display or change the system's network name. The system uses this name to identify itself to other computers on the local network or over the internet.

Syntax :

```bash
hostname [OPTIONS] [NEW_HOSTNAME]
```

**Common Examples & Use Cases**

```bash
# 1. Display the current hostname
hostname

# 2. Get the Fully Qualified Domain Name (FQDN)
hostname -f

# 3. Display the machine's local IP address
hostname -I #here we can use -i samall i aslo

# 4. Change the hostname temporarily (requires sudo)
sudo hostname new-server-name
```

> How to Change the Hostname Permanently

```bash
sudo hostnamectl set-hostname my-production-webserver
```

---

## ip 

It is a powerful command-line utility used to display and configure network interfaces, IP addresses, routing tables, and neighbor (ARP) tables.


Syntax :

```bash
ip [OPTIONS] OBJECT { COMMAND | help }
```

Some Common `` ip `` command :

``` bash

# Show all IP addresses:
ip address show


# Show addresses in a clean, brief summary:
ip -br a


# Show only IPv4 addresses:
bash ip -4 a


# Assign an IP address to an interface:
bash sudo ip addr add 192.168.1.50/24 dev eth0


# Remove an IP address from an interface:
bash sudo ip addr del 192.168.1.50/24 dev eth0

```

---

## iwconfig

It is used to view and configure wireless network interface parameters (such as ESSID, frequency, transmit power, and encryption).

Syntax :

```bash
iwconfig [INTERFACE] [PARAMETERS]
```

> Running `` iwconfig `` with no arguments lists all wireless network interfaces on the system.

---

## ss  (Socket Statistics)

It display detailed information about active network connections, listening ports, and routing states. It work similiar as `` netstat ``.

Syntax :

``` bash
ss [options] [FILTER]
```

---

## dig

It is a flexible and comprehensive command-line tool used to query DNS (Domain Name System) servers.

Syntax :
```bash
dig [DOMAIN_NAME] [RECORD_TYPE]
```

**Example Output and Breakdown***

If you run `` dig google.com  `` in your terminal, the output looks like this:

``` text
; <<>> DiG 9.18.28 <<>> google.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 32415
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;google.com.			IN	A

;; ANSWER SECTION:
google.com.		237	IN	A	142.250.71.46

;; Query time: 4 msec
;; SERVER: 172.31.0.2#53(172.31.0.2) (UDP)
;; WHEN: Wed Oct 07 13:51:30 UTC 2026
;; MSG SIZE  rcvd: 55
```

__Here are the critical sections to focus on:__

• `` status: NOERROR ``: Indicates the DNS query succeeded. If it says `` NXDOMAIN `` , the domain name does not exist.

• `` QUESTION SECTION ``: Shows exactly what your terminal asked the server ( `` google.com. IN A `` means you requested the main IPv4 address record).

• `` ANSWER SECTION `` **(The Most Important Part)** : Shows the final result. It tells you `` google.com `` points to the physical IP address `` 142.250.71.46 `` . The number `` 237 `` is the **TTL (Time to Live)** in seconds, representing how long this answer can stay cached in memory before expiring.

• `` SERVER: 172.31.0.2#53 `` : The IP address of the local nameserver that resolved this question for you over port `` 53 `` .


**Common DevOps Use Cases & Options**

```bash
# 1. Get just the clean, short answer (+short)
# The default output is very noisy. If you are writing a script and only want the raw IP address without any metadata, append +short:
dig google.com +short # Output: 142.250.71.46



# 2. Query a specific DNS server
# To see if an updated domain record has propagated globally, you can bypass your internal server and force your query directly through a public resolver like Cloudflare (1.1.1.1):
dig @1.1.1.1 google.com



# 3. Request specific record types
# You can verify custom infrastructure records by appending the type keyword at the end:

# • Find Mail Servers: 
dig google.com MX

# • Find Text/Verification Records (DKIM/SPF):
dig google.com TXT

#• Find Canonical Aliases (CDN routes): 
dig ://example.com CNAME



# 4. Trace the full DNS path (+trace)
# To debug a broken domain routing configuration, use +trace. This forces dig to walk through the entire hierarchical internet lookup path, starting at the root nameservers down to the authoritative host:
dig google.com +trace

```

---

## arp  (Address Resolution Protocol) 

It is used to view and manage the system's local **ARP** cache. The ARP cache is a temporary lookup table that maps **IP addresses** (Layer 3 software addresses) directly to their physical **MAC addresses** (Layer 2 hardware addresses) on your local area network (LAN).

Syntax :

```bash
arp [OPTIONS]
```


**Example Output and Breakdown**

If you type `` arp `` by itself or `` arp -e `` in your terminal, it displays a snapshot of the local network mappings:

```text
Address                  HWtype   HWaddress           Flags Mask            Iface
172.31.0.1               ether    06:7d:ef:12:34:56   C                     eth0
```

• `` Address `` : The IPv4 address of another device or gateway on your local network.


• `` HWtype `` : The hardware protocol being used (usually `` ether `` for standard Ethernet connections).


• `` HWaddress `` : The unique physical MAC address belonging to that device's network card.


• `` Flags Mask `` : The type of entry. `` C `` means a Complete entry that was learned dynamically over the network. `` M `` means a Manual/Permanent entry.


• `` Iface `` : The local network interface card (like `` eth0 `` ) used to communicate with that device.


**Common DevOps Use Cases & Options**


### 1. Show all entries numerically (-n)

By default, `` arp `` tries to find the computer names for those IPs, which can cause delays. To force it to display clean numerical IP addresses instantly:

```bash
arp -n
```

### 2. Delete a stale entry (-d) (Requires sudo)

If a server on your local network changed its network card and has a new MAC address, its old entry in your cache can cause connection failures. You can manually remove the stale IP mapping:

```bash
sudo arp -d 172.31.0.1
```


### 3. Add a permanent static mapping (-s) (Requires sudo)

To prevent network tampering or hacking vectors like ARP Spoofing/Poisoning, you can hardcode a trusted device's IP and MAC address so it never changes dynamically:

```bash
sudo arp -s 172.31.0.1 06:7d:ef:12:34:56
```


---

## nc (netcat)

DevOps engineers and system administrators rely on `` nc `` for quick tasks like scanning open ports, debugging socket connections, transferring files between servers, or setting up quick, temporary test listeners.

Syntax : 
```bash
nc [OPTIONS] HOST PORT
```


**Key Flags You Need to Know**

• `` -z `` (Zero-I/O / Scan Mode)
Tells Netcat to report connection status without actually sending any data. It is the primary flag used for port scanning.

• `` -v `` (Verbose)
Provides detailed diagnostic messages on your screen about the connection state.

• `` -l `` (Listen)
Makes Netcat act as a server, binding to a local port and waiting for incoming client connections.

• `` -u `` (UDP)
Forces Netcat to use UDP instead of the default TCP protocol.


*Common Examples & Use Cases*

#### 1. Scan if a TCP port is open (Port Check)

To quickly verify whether an application port (like port 80 for Nginx or 5432 for PostgreSQL) is reachable on a target server:

```bash
nc -zv 172.31.38.144 80
```

• Success Output: `` Connection to 172.31.38.144 80 port [tcp/http] succeeded! ``
• Failure Output: `` Connection to 172.31.38.144 port 80 [tcp/http] failed: Connection refused ``

#### 2. Open a temporary listening server (Catch incoming traffic)

If you want to test if a remote server can reach your local machine on a specific port, start a listener on your terminal:

```bash
nc -l 8080
```

> Any text sent from another machine to your IP on port 8080 will now print directly onto your screen.


#### 3. Transfer files between two servers

You can stream raw file data across the network instantly using Netcat.

* On the receiving server (Set up the listener to save the file) :

```bash
nc -l 9000 > received_backup.tar.gz
```

* On the sending server (Push the file into the connection):

```bash
nc 172.31.38.144 9000 < backup.tar.gz
```


#### 4. Grab a service banner (HTTP Check)

You can connect directly to a web server and request raw headers to see what software version it runs:

```bash
nc 142.250.71.46 80
```


> Once connected, type GET / HTTP/1.0 and press Enter twice to see the server response.


---

## whois

It is used to search public registries for the ownership, registration details, expiration dates, and contact information of a domain name or IP address.

Syntax :

```bash
whois [DOMAIN_NAME_OR_IP]
```

**Example Output and Breakdown**

If you run whois `` google.com `` in your terminal, it connects to a domain registry server and prints a large block of text containing registry records:

```text
Domain Name: GOOGLE.COM
Registry Domain ID: 213851_DOMAIN_COM-VRSN
Registrar WHOIS Server: ://markmonitor.com
Registrar URL: http://markmonitor.com
Updated Date: 2019-09-09T15:39:04Z
Creation Date: 1997-09-15T04:00:00Z
Registry Expiry Date: 2028-09-13T04:00:00Z
Registrar: MarkMonitor, Inc.
Name Server: ://google.com
Name Server: ://google.com
```

__Here are the essential data points you look for:__

• `` Registrar `` : The commercial company where the domain was purchased (e.g., MarkMonitor, GoDaddy, Namecheap).

• `` Creation Date `` : The exact date and time when the domain name was first registered on the internet.

• `` Registry Expiry Date `` : When the domain registration will lapse unless the owner pays to renew it.

• `` Name Server `` : The specific authoritative DNS servers responsible for handling traffic routing requests for that domain.

---

## iplugstatus

---

## curl vs wget

---

## route

---

## nmap

---

## wget

---

## watch

---

## iptables

---