# 3. Nmap Basic Port Scans

#### **1. TCP and UDP Ports**

At the risk of oversimplification, we can classify ports into two states:

1. An open port indicates that a service is listening on that port.
2. A closed port indicates that no service is listening on that port.

However, in practical situations, we need to consider the impact of firewalls. For instance, a port might be open, but a firewall might be blocking the packets. Therefore, Nmap considers the following six states:

1. An **open** port indicates that a service is listening on the specified port.
2. A **closed** port indicates that no service is listening on the specified port, although the port is accessible. By accessible, we mean that it is reachable and is not blocked by a firewall or other security appliances/programs.
3. **Filtered means that Nmap cannot determine whether the port is open or closed because it** is not accessible. This state is usually due to a firewall preventing Nmap from reaching that port. Nmap’s packets may be blocked from reaching the port; alternatively, responses may be blocked from reaching Nmap’s host.
4. **Unfiltered means that Nmap cannot determine whether the port is open or closed, even though** the port is accessible. This state is encountered when using an ACK scan `sA`.
5. **Open|Filtered**: This means that Nmap cannot determine whether the port is open or filtered.
6. **Closed|Filtered**: This means that Nmap cannot decide whether a port is closed or filtered.

> Which service uses UDP port 53 by default? DNS
> 

> Which service uses TCP port 22 by default? SSH
> 

> How many port states does Nmap consider? 6
> 

> Which port state is the most interesting to discover as a pentester? Open
> 

### **2. TCP Flags**

the TCP header flags are:

1. **URG**: Urgent flag indicates that the urgent pointer field is significant. The urgent pointer indicates that the incoming data is urgent, and that a TCP segment with the URG flag set is processed immediately, without waiting for previously sent TCP segments.
2. **ACK**: Acknowledgement flag indicates that the acknowledgement number is significant. It is used to acknowledge the receipt of a TCP segment.
3. **PSH**: Push flag asking TCP to pass the data to the application promptly.
4. **RST**: The reset flag is used to reset the connection. Another device, such as a firewall, might send it to tear a TCP connection. This flag is also used when data is sent to a host, and there is no service on the receiving end to answer.
5. **SYN**: The synchronise flag is used to initiate a TCP 3-way handshake and synchronise sequence numbers with the other host. The sequence number should be set randomly during TCP connection establishment.
6. **FIN**: The sender has no more data to send.

What 3 letters represent the Reset flag? RST

Which flag needs to be set when you initiate a TCP connection (first packet of TCP 3-way handshake)? SYN

#### **3. TCP Connect Scan**

TCP connect scan works by completing the TCP 3-way handshake. In standard TCP connection establishment, the client sends a TCP packet with the SYN flag set, and the server responds with SYN/ACK if the port is open; finally, the client completes the 3-way handshake by sending an ACK. whether the TCP port is open, not in establishing a TCP connection. Hence, the connection is torn as soon as its state is confirmed by sending a RST/ACK. You can choose to run a TCP connect scan using `-sT`. It is important to note that if you are not a privileged user (root or sudoer), a TCP connect scan is the only possible option to discover open TCP ports. To illustrate the `-sT` (TCP connect scan), Run the following command in the AB to get a detailed list of the open ports.

Note that we can use `-F` to enable fast mode and decrease the number of scanned ports from 1000 to 100 most common ports. It is worth mentioning that the `-r` option can also be added to scan the ports in consecutive order instead of random order. This option is useful for testing whether ports open consistently, for instance, when a target boots up.

> What is the state of the FTP service running on port 21? open
> 

> What is Nmap’s guess about the service running on port 53? domain
> 

#### **4. TCP SYN Scan**

Unprivileged users are limited to the connect scan. However, the default scan mode is a SYN scan, and it requires a privileged (root or sudo) user to run. SYN scan does not need to complete the TCP 3-way handshake; instead, it tears down the connection after receiving a response from the server. Because we didn’t establish a TCP connection, the scan is less likely to be logged. We can select this scan type by using the -sS option. The figure below shows how the TCP SYN scan works without completing the TCP 3-way handshake.we can see TCP connect scan `-sT` traffic. Any open TCP port will require Nmap to complete the TCP 3-way handshake before closing the connection. In the lower half of the following figure, we see how a SYN scan `-sS` does not need to complete the TCP 3-way handshake; instead, Nmap sends an RST packet once a SYN/ACK packet is received.

> After launching a TCP SYN scan, how many SYN-ACK packets are successfully received in AttackBox? 4
> 

> How many ports are open on the target machine? 4
> 

#### **5. UDP Scan**

UDP is a connectionless protocol; hence, it does not require a handshake for connection establishment. If a UDP packet is sent to a closed port, an ICMP port unreachable error (type 3, code 3) is returned. You can select UDP scan using the `-sU` option; moreover, you can combine it with another TCP scan.

> What is the state of port number 161 over UDP in the target machine? closed
> 

> What is the service name according to Nmap on port 161? snmp
> 

#### **6. Fine-Tuning Scope and Performance**

You can specify the ports you want to scan instead of the default 1000 ports. Specifying the ports is intuitive by now.

- port list: `p22,80,443` will scan ports 22, 80 and 443.
- port range: `p1-1023` will scan all ports between 1 and 1023 inclusive, while `p20-25` will scan ports between 20 and 25 inclusive.

You can request the scan of all ports by using `-p-`, which will scan all 65535 ports. If you want to scan the most common 100 ports, add `-F`. Using `--top-ports 10` will check the ten most common ports. You can control the scan timing using `-T<0-5>`. `-T0` is the slowest (paranoid), while `-T5` is the fastest. According to the Nmap manual page, there are six templates:

- paranoid (0)
- sneaky (1)
- polite (2)
- normal (3)
- aggressive (4)
- insane (5)

To avoid IDS alerts, you might consider `-T0` or `-T1`. For instance, `-T0` scans one port at a time and waits 5 minutes between sending each probe, so you can guess how long scanning one target would take to finish. If you don’t specify any timing, Nmap uses normal `-T3`. Note that `-T5` is the most aggressive in terms of speed; however, this can affect the accuracy of the scan results due to the increased likelihood of packet loss. Note that `-T4` is often used during CTFs and when learning to scan on practice targets, whereas `-T1` is often used during real engagements where stealth is more important. Alternatively, you can choose to control the packet rate using `--min-rate <number>` and `--max-rate <number>`. For example, `--max-rate 10` or `--max-rate=10` ensures that your scanner is not sending more than ten packets per second.

Moreover, you can control probing parallelisation using `--min-parallelism <numprobes>` and `--max-parallelism <numprobes>`. Nmap probes targets to discover which hosts are live and which ports are open; the probing parallelisation parameter specifies the number of such probes that can run in parallel. For instance, `--min-parallelism=512` pushes Nmap to maintain at least 512 probes in parallel; these 512 probes are related to host discovery and open ports.

> What is the option to scan all the TCP ports between 5000 and 5500? -p 5000-5500
> 

> How can you ensure that Nmap will run at least 64 probes in parallel? --min-parallelism 64
> 

> What option would you add to make Nmap very slow and paranoid? -T0
> 

#### **7. Summary-5**

This room covered three types of scans.

| **Port Scan Type** | **Example Command** |
| --- | --- |
| TCP Connect Scan | nmap -sT MACHINE_IP |
| TCP SYN Scan | sudo nmap -sS MACHINE_IP |
| UDP Scan | sudo nmap -sU MACHINE_IP |

These scan types should get you started discovering running TCP and UDP services on a target host.

| **Option** | **Purpose** |
| --- | --- |
| -p- | all ports |
| -p1-1023 | scan ports 1 to 1023 |
| -F | 100 most common ports |
| -r | scan ports in consecutive order |
| -T<0-5> | -T0 being the slowest and T5 the fastest |
| --max-rate 50 | rate <= 50 packets/sec |
| --min-rate 15 | rate >= 15 packets/sec |
| --min-parallelism 100 | at least 100 probes in parallel |

# 

####
