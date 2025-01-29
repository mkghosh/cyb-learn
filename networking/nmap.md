What is nmap?
- nmap has the elaboration of Network Mapper is an open source tool for network exploration and security auditing. It was designed to rapidly scan large networks, although it works fine against single hosts. Nmap uses raw IP packets in novel ways to determine what hosts are available on the network, what services (application name and version) those hosts are offering, what operating systems (and OS versions) they are running, what type of packet filters/firewalls are in use, and dozens of other characteristics. While Nmap is commonly used for security audits, many systems and network administrators find it useful for routine tasks such as network inventory, managing services upgrade schedules, and monitoring host or service uptime. 

The output from Nmap is a list of scanned targets, with supplemental infomation is the "interesting ports table". That table lists the port number and protocol, service name, and state. The state is either open, filtered, closed, or unfiltered. Open means that an application on the target machine is listening for connections/packets on that port. Filtered means that a firewall, filter, or other network obstacle is blocking the port so that Nmap cannot tell whether it is open or closed. Closed ports have no application listening on them, though tehy could open up at any time. 

The help file for nmap is :

Nmap 7.92 ( https://nmap.org )
Usage: nmap [Scan Type(s)] [Options] {target specification}
TARGET SPECIFICATION:
  Can pass hostnames, IP addresses, networks, etc.
  Ex: scanme.nmap.org, microsoft.com/24, 192.168.0.1; 10.0.0-255.1-254
  -iL <inputfilename>: Input from list of hosts/networks
  -iR <num hosts>: Choose random targets
  --exclude <host1[,host2][,host3],...>: Exclude hosts/networks
  --excludefile <exclude_file>: Exclude list from file
HOST DISCOVERY:
  -sL: List Scan - simply list targets to scan
  -sn: Ping Scan - disable port scan
  -Pn: Treat all hosts as online -- skip host discovery
  -PS/PA/PU/PY[portlist]: TCP SYN/ACK, UDP or SCTP discovery to given ports
  -PE/PP/PM: ICMP echo, timestamp, and netmask request discovery probes
  -PO[protocol list]: IP Protocol Ping
  -n/-R: Never do DNS resolution/Always resolve [default: sometimes]
  --dns-servers <serv1[,serv2],...>: Specify custom DNS servers
  --system-dns: Use OS's DNS resolver
  --traceroute: Trace hop path to each host
SCAN TECHNIQUES:
  -sS/sT/sA/sW/sM: TCP SYN/Connect()/ACK/Window/Maimon scans
  -sU: UDP Scan
  -sN/sF/sX: TCP Null, FIN, and Xmas scans
  --scanflags <flags>: Customize TCP scan flags
  -sI <zombie host[:probeport]>: Idle scan
  -sY/sZ: SCTP INIT/COOKIE-ECHO scans
  -sO: IP protocol scan
  -b <FTP relay host>: FTP bounce scan
PORT SPECIFICATION AND SCAN ORDER:
  -p <port ranges>: Only scan specified ports
    Ex: -p22; -p1-65535; -p U:53,111,137,T:21-25,80,139,8080,S:9
  --exclude-ports <port ranges>: Exclude the specified ports from scanning
  -F: Fast mode - Scan fewer ports than the default scan
  -r: Scan ports consecutively - don't randomize
  --top-ports <number>: Scan <number> most common ports
  --port-ratio <ratio>: Scan ports more common than <ratio>
SERVICE/VERSION DETECTION:
  -sV: Probe open ports to determine service/version info
  --version-intensity <level>: Set from 0 (light) to 9 (try all probes)
  --version-light: Limit to most likely probes (intensity 2)
  --version-all: Try every single probe (intensity 9)
  --version-trace: Show detailed version scan activity (for debugging)
SCRIPT SCAN:
  -sC: equivalent to --script=default
  --script=<Lua scripts>: <Lua scripts> is a comma separated list of
           directories, script-files or script-categories
  --script-args=<n1=v1,[n2=v2,...]>: provide arguments to scripts
  --script-args-file=filename: provide NSE script args in a file
  --script-trace: Show all data sent and received
  --script-updatedb: Update the script database.
  --script-help=<Lua scripts>: Show help about scripts.
           <Lua scripts> is a comma-separated list of script-files or
           script-categories.
OS DETECTION:
  -O: Enable OS detection
  --osscan-limit: Limit OS detection to promising targets
  --osscan-guess: Guess OS more aggressively
TIMING AND PERFORMANCE:
  Options which take <time> are in seconds, or append 'ms' (milliseconds),
  's' (seconds), 'm' (minutes), or 'h' (hours) to the value (e.g. 30m).
  -T<0-5>: Set timing template (higher is faster)
  --min-hostgroup/max-hostgroup <size>: Parallel host scan group sizes
  --min-parallelism/max-parallelism <numprobes>: Probe parallelization
  --min-rtt-timeout/max-rtt-timeout/initial-rtt-timeout <time>: Specifies
      probe round trip time.
  --max-retries <tries>: Caps number of port scan probe retransmissions.
  --host-timeout <time>: Give up on target after this long
  --scan-delay/--max-scan-delay <time>: Adjust delay between probes
  --min-rate <number>: Send packets no slower than <number> per second
  --max-rate <number>: Send packets no faster than <number> per second
FIREWALL/IDS EVASION AND SPOOFING:
  -f; --mtu <val>: fragment packets (optionally w/given MTU)
  -D <decoy1,decoy2[,ME],...>: Cloak a scan with decoys
  -S <IP_Address>: Spoof source address
  -e <iface>: Use specified interface
  -g/--source-port <portnum>: Use given port number
  --proxies <url1,[url2],...>: Relay connections through HTTP/SOCKS4 proxies
  --data <hex string>: Append a custom payload to sent packets
  --data-string <string>: Append a custom ASCII string to sent packets
  --data-length <num>: Append random data to sent packets
  --ip-options <options>: Send packets with specified ip options
  --ttl <val>: Set IP time-to-live field
  --spoof-mac <mac address/prefix/vendor name>: Spoof your MAC address
  --badsum: Send packets with a bogus TCP/UDP/SCTP checksum
OUTPUT:
  -oN/-oX/-oS/-oG <file>: Output scan in normal, XML, s|<rIpt kIddi3,
     and Grepable format, respectively, to the given filename.
  -oA <basename>: Output in the three major formats at once
  -v: Increase verbosity level (use -vv or more for greater effect)
  -d: Increase debugging level (use -dd or more for greater effect)
  --reason: Display the reason a port is in a particular state
  --open: Only show open (or possibly open) ports
  --packet-trace: Show all packets sent and received
  --iflist: Print host interfaces and routes (for debugging)
  --append-output: Append to rather than clobber specified output files
  --resume <filename>: Resume an aborted scan
  --noninteractive: Disable runtime interactions via keyboard
  --stylesheet <path/URL>: XSL stylesheet to transform XML output to HTML
  --webxml: Reference stylesheet from Nmap.Org for more portable XML
  --no-stylesheet: Prevent associating of XSL stylesheet w/XML output
MISC:
  -6: Enable IPv6 scanning
  -A: Enable OS detection, version detection, script scanning, and traceroute
  --datadir <dirname>: Specify custom Nmap data file location
  --send-eth/--send-ip: Send using raw ethernet frames or IP packets
  --privileged: Assume that the user is fully privileged
  --unprivileged: Assume the user lacks raw socket privileges
  -V: Print version number
  -h: Print this help summary page.
EXAMPLES:
  nmap -v -A scanme.nmap.org
  nmap -v -sn 192.168.0.0/16 10.0.0.0/8
  nmap -v -iR 10000 -Pn -p 80
SEE THE [MAN PAGE](https://nmap.org/book/man.html) FOR MORE OPTIONS AND EXAMPLES

## Nmap Live host discovery

ARP (Address resolution protocol) request is bound within the subnet it can't be routed to other subnet.

## Scanning using nmap

We can use lists of hosts seperated by space to specify target hosts like below

- list: $nmap Machine_ip scanme.nmap.org example.com
- range: 10.10.10.12-18 will scan 7 ip addresses
- subent: ip/30 will scan 4 ips

We can also provide a file as input for our list of targets **$nmap -iL list.txt**. To check the list of hosts that nmap will scan, we can use **$nmap -sL targets**. This option will give us a detialed list of the hosts that nmap will scan without scanning them; however, nmap will attempt a reverse-DNS resolution on all the targets to obtain their names. Names might reveal various information to the pentester.

### Discover live host in a network

1. When a privileged user tries to scan targets on a local network (Ethernet), Namap uses ARP requests.
2. When a privileged user tries to scan targets outside the local network, nmap uses ICMP echo requests, TCP ACK (Acknowledgte) to port 80, TCP SYN (SYnchronize) to port 443, and ICMP timestamp request.
3. When an unprivileged user tries to scan targets outhside the local network, nmap resorts to a TCP 3-way handshake by sending SYN packets to ports 80 and 443.

Nmap, by default, uses a ping scan to find live hosts, then proceeds to scan live hosts only. If we want to use nmap to discover online hosts without port-scanning the live systems, we can issue **$nmap -sn targets**. 

If we want nmap to perform an ARP scan without port-scanning, we can use **$nmap -PR -sn targets**

## ARP-SCAN

arp-scan is an utility built around arp queries; it provides many options to customize our scan. One popular choice is **arp-scan --localhost or simply arp-scan -l**. This command will send ARP queries to all valid IP addresses on the local network. If your system has more than one interface and you are interested in discovering the live hosts on one of them, you can specify the interface using **-I**. For instance, **sudo arp-scan -I eth0 -l** will send ARP queries for all valid IP addresses on the eth0 interface.


## ICMP host discovery

To use ICMP echo we should add the switch **-PE** to nmap like **$sudo nmap -PE -sn target** (-sn for not to start port scanning). As, ICMP echo may be blocked in many system we can use ICMP Timestamp (ICMP Type 13) and check whether it will get a Timestamp reply (ICMP Type 14). Adding **-PP** option tells nmap to use ICMP timestamp requests like **$sudo nmap -PP -sn target**

Similarly, namp uses address mask queries (ICMP Type 17) and checks whether it gets an address mask reply (ICMP Type 18). This scan can be enabled with the option **-PM** like **$sudo nmap -PM -sn target** 

To know more about ICMP hope over [here](https://www.iana.org/assignments/icmp-parameters/icmp-parameters.xhtml)

## TCP UDP host discovery

If you want to nmap to use TCP SYN ping, you can do so via the option **-PS** followed by the port number, range, list, or a combination of them. For example **-PS80** will target port 80 while **-PS21-25** will target ports 21 through 25. Finally **-PS80,443,8080** will target the three ports.

Privileged users (root or sudoers) can send TCP SYN packets and don't need to complete the TCP 3-way handshake even if the port is open.

To send an ACK packet we can use **-PA** flag to nmap, but we must use sudo or root to run nmap to be able to send ACK packet to the targets, otherwise it will do a 3-way handshake.

We can also send an UDP Ping packet to discover live hosts on a network. Contrary to the TCP SYN ping, sending a UDP packet to an open port is not expected to lead to any reply. However, if we send a UDP packet to a closed port, we expect to get an ICMP port unreachable packet; this indicates that the target system is up and available. We can send UDP Ping like **$nmap -PU -sn target**


### Masscan
Masscan uses a similar approach to discover the available systems. However, to finish its network scan quickly, Masscan is quite aggressive with the rate of packtes it generates. The syntax is auite similar: **-p** can be followed by a port number, list, or range. consider the following EXAMPLES

- masscan Target/24 -p443
- masscan Target/24 -p80,443
- masscan Target/24 -p22-30
- masscan Target/24 --tcp-ports 100

### Reverse-DNS lookup

Nmap's default behaviour is to use reverse-DNS online hosts. Because the hostnames can reveal a lot, this can be a helpful step. However, if you don't want to send such DNS queries, you use -n to skip this step. By default, Namp will look up online hosts; however, you can use the option **-R** to query the DNS server even for offline hosts. If you want to use a specific DNS server, you can add the **--dns-servers DNS_SERVER** option.