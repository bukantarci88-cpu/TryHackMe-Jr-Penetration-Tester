# 2. Nmap Live Host Discovery

#### **1. Subnetworks**

A subnet is a smaller logical division of a network. It is used to organize devices efficiently, improve security, and reduce network traffic. Subnetting helps manage IP addresses better by splitting a large network into smaller networks using a subnet mask or CIDR notation, such as `/24` or `/25`.”

- Subnets with `/16`, which means that the subnet mask can be written as `255.255.0.0`. This subnet can accommodate around 65,000 hosts.
- Subnets with `/24`, which indicates that the subnet mask can be expressed as `255.255.255.0`. This subnet can have around 250 hosts.

#### **2. Enumerating Targets**

> Q. What is the first IP address Nmap would scan if you provided 10.10.12.13/29 as your target?
> 

Ans. 10.10.12.8

> Q. How many IP addresses will Nmap scan if you provide the following range: 10.10.0-255.101-125?6400 (nmap -sL -n 10.10.0–255.101–125)
> 

The `-n` flag in Nmap disables DNS resolution, meaning Nmap will not try to translate IP addresses into hostnames. This speeds up scans and reduces DNS-related network traffic.

#### **3. Nmap Host Discovery Using ARP**

Nmap follows the following approaches to discover live hosts:

1. When a privileged user tries to scan targets on a local network (Ethernet), Nmap uses ARP requests. A privileged user is `root` or a user who belongs to `sudoers` and can run `sudo`.
2. When a privileged user tries to scan targets outside the local network, Nmap uses ICMP echo requests, TCP ACK (Acknowledge) to port 80, TCP SYN (Synchronise) to port 443, and ICMP timestamp requests.
3. When an unprivileged user tries to scan targets outside the local network, Nmap resorts to a TCP 3-way handshake by sending SYN packets to ports 80 and 443.

> Q. How many hosts are found alive after scanning the CONNECTION_IP/24? 1
> 

#### **4. Nmap Host Discovery Using ICMP**

ICMP stands for Internet Control Message Protocol. It is used in networking for error reporting and connectivity testing. Tools like ping and traceroute use ICMP to check whether hosts are reachable and to diagnose network issues.

**A. ICMP echo request:** It is to discover live hosts, add the option `-PE` option. (Remember to add `-sn` if you don’t want to follow that with a port scan.) ICMP Echo Request is a network message used to test connectivity between devices. It is commonly used by the ping command to determine whether a host is reachable on the network.

Using the AB, we will scan the target's subnet using `nmap -PE -sn 10.200.6.0/24`. This scan will send ICMP echo packets to every IP address on the subnet. Again, we expect live hosts to reply; however, it is wise to keep in mind that many firewalls block ICMP. The output below shows the result of scanning the virtual machine’s class C subnet using `sudo nmap -PE -sn 10.200.6.0/24` from the AttackBox. This command checks which devices are online in the `10.200.6.0/24` subnet by sending ICMP echo requests without performing a full port scan.

A /24 subnet means: 255.255.255.0=/24, So the scan range is typically: 10.200.6.1 → 10.200.6.254

**B. ICMP Timestamp:** Nmap uses a timestamp request (ICMP Type 13) and checks whether it will get a Timestamp reply (ICMP Type 14). Adding the `-PP` option tells Nmap to use ICMP timestamp requests. We run `nmap -PP -sn 10.200.6.0/24` to discover the online computers on the target machine subnet. The command performs host discovery on the `10.200.6.0/24` network using ICMP Timestamp Requests without scanning ports.

ICMP Timestamp Request is an ICMP message type used to query a device’s system time. It can also be used in network scanning and diagnostics to identify active hosts.

**C. ICMP Type 17:** ICMP Type 17 is Address Mask Request, used by a host to request its subnet mask from a network device. It is largely obsolete today and replaced by DHCP. Similarly, Nmap uses address mask queries (ICMP Type 17) and checks whether it gets an address mask reply (ICMP Type 18). This scan can be enabled with the option `-PM`.

> Q1. What is the option required to tell Nmap to use ICMP Timestamp to discover live hosts? PP
> 

> Q2. What is the option required to tell Nmap to use ICMP Address Mask to discover live hosts? PM
> 

> Q3. What is the option required to tell Nmap to use ICMP Echo to discover live hosts? PE
> 

#### **5. Nmap Host Discovery Using TCP and UDP**

**A. TCP SYN Ping:** We can send a packet with the SYN (Synchronise) flag set to TCP port 80 (80 by default) and wait for a response. An open port should reply with a SYN/ACK (Acknowledge); a closed port would result in an RST (Reset). In this case, we only check whether we receive a response to infer whether the host is up. We will run `nmap -PS -sn 10.200.6.0/24` to scan the target VM subnet.

**B. TCP ACK Ping:** As you have guessed, this sends a packet with an ACK flag set. You must run Nmap as a privileged user to accomplish this. If you try it as an unprivileged user, Nmap will attempt a 3-way handshake. By default, port 80 is used. The syntax is similar to TCP SYN ping. `-PA` should be followed by a port number, a port number range, a list, or a combination of them. For example, consider `-PA21`, `-PA21-25` and `-PA80,443,8080`. If no port is specified, port 80 will be used. In this example, we run `sudo nmap -PA -sn 10.200.6.0/24` to discover the online hosts on the target’s subnet.

**C. UDP Ping:** Finally, we can use UDP to discover if the host is online. Unlike a TCP SYN ping, sending a UDP packet to an open port is not expected to elicit a reply. However, if we send a UDP packet to a closed UDP port, we expect to get an ICMP port-unreachable packet; this indicates that the target system is up and available. The syntax to specify the ports is similar to that of TCP SYN ping and TCP ACK ping; Nmap uses `-PU` for UDP ping.

Masscan is a high-speed port scanner used to scan large networks very quickly. It is often used for reconnaissance because it can scan huge IP ranges in a short time, but it provides less detail compared to tools like Nmap.

On a side note, Masscan uses a similar approach to discover the available systems. However, to complete its network scan quickly, Masscan is quite aggressive in the rate at which it generates packets. The syntax is quite similar: -p can be followed by a port number, list, or range. Consider the following examples:

masscan 10.200.6.0/24 -p443

masscan 10.200.6.0/24 -p80,443

masscan 10.200.6.0/24 -p22-25

> Which TCP ping scan does not require a privileged account? TCP SYN Ping
> 

> Which TCP ping scan requires a privileged account? TCP ACK Ping
> 

> What option do you need to add to Nmap to run a TCP SYN ping scan on the telnet port? PS23
> 

#### **6. Using Reverse-DNS Lookup**

Reverse DNS (rDNS) is the process of resolving an IP address to a hostname (the opposite of normal DNS). Instead of asking “What is the IP of [example.com?”](http://example.com/?%E2%80%9D), you ask “What domain name is assigned to 10.200.6.15?”

In Nmap, using -R forces reverse lookups for all discovered hosts, while -n disables them. This helps identify system roles (e.g., mail.company.local, [dc01.domain.com](http://dc01.domain.com/)) and understand network structure during recon. However, rDNS records aren’t always configured or accurate, so results may be missing or misleading. It can also slightly slow scans due to additional DNS queries. By default, Nmap will look up online hosts; however, you can use the option -R to query the DNS server even for offline hosts. If you want to use a specific DNS server, you can add the --dns-servers DNS_SERVER option.

We want Nmap to issue a reverse DNS lookup for all the possible hosts on a subnet, hoping to get some insights from the names. What option should we add? R (A reverse DNS lookup is the process of finding the domain name (hostname) associated with an IP address.)

#### **7. Summary-4**

You have learned how ARP, ICMP, TCP, and UDP can detect live hosts by completing this room.

| **Scan Type** | **Example Command** |
| --- | --- |
| ARP Scan | sudo nmap -PR -sn 10.200.6.0/24 |
| ICMP Echo Scan | sudo nmap -PE -sn 10.200.6.0/24 |
| ICMP Timestamp Scan | sudo nmap -PP -sn 10.200.6.0/24 |
| ICMP Address Mask Scan | sudo nmap -PM -sn 10.200.6.0/24 |
| TCP SYN Ping Scan | sudo nmap -PS22,80,443 -sn 10.200.6.0/30 |
| TCP ACK Ping Scan | sudo nmap -PA22,80,443 -sn 10.200.6.0/30 |
| UDP Ping Scan | sudo nmap -PU53,161,162 -sn 10.200.6.0/30 |

Remember to add `-sn` if you are only interested in host discovery without port-scanning. Omitting `-sn` will let Nmap default to scanning live hosts for ports.

| **Option** | **Purpose** |
| --- | --- |
| -n | no DNS lookup |
| -R | reverse-DNS lookup for all hosts |
| -sn | host discovery only |

Knowing which hosts are online ensures accurate scoping and efficient follow-up scans. A host is a device on a network identified by an IP address, and a port is a logical endpoint on that host used to access specific services. Together, they define where network communication is directed.
