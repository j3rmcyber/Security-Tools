# Nmap Commands and Brief Descriptions
## Basics:
- nmap -F 192.168.0.1 (fast scan, top 100 ports)
- nmap -p *##* 192.168.0.1 (scan specific port)
- nmap -p 80,443,50-1000 (multiple ports separate by comma)
- nmap -p msrpc,http,ftp 192.168.0.1 (scans by port name)
- nmap -p “*” 192.168.0.1 (wildcard: invocation to explore all port landscape)
- nmap -sU -sT -p U:53,T:25 192.168.0.1 (UDP and TCP port scan)
- nmap --top-ports *10* 192.168.0.1 (scan top ports, here top 10 ports)
- nmap -v -r 192.168.0.1 (sequential port scan)
- nmap -sn (only ping, no port. Online only)

## Foundational Scanning:
- nmap -PN 192.168.0.1 (do not ping)
- nmap -sP 192.168.0.0/24 (ping only also ARP scan returning MACs)
- nmap -PS *port#s* 192.168.0.1 (tcp syn ping: syn packet waits for response) Use if blocked ICMP
port# is optional

- nmap -PA 192.168.0.1 (tcp ack ping: discover hosts)
- nmap -PU *port#s* 192.168.0.1 (udp ping) port# optional
- nmap -PY *port#s* 192.168.0.1 (sctp init ping) IP telephony systems, port# optional
- nmap -PE 192.168.0.1 (icmp echo ping) Local networks, internet hosts do not respond
PE is automatic if no other P method is chosen
- nmap -PP 192.168.0.1 (ICMP timestamp ping) improperly configured systems may respond
- nmap -PM 192.168.0.1 (icmp address mask ping) alternative registers.
bypass firewalls blocking standard requests
- nmap -PO *protocols* 192.168.0.1 (ip protocol ping) diff protocols sent
protocols optional
- nmap -PR 192.168.0.1 (arp ping) faster, increased accuracy, noting blocks ARP. Subent only
- nmap --traceroute 192.168.0.1 (traceroute, shows path to target)
- nmap -R 192.168.0.1 (force reverse dns resolution) recon on block, resolve dns info for IPs
- nmap -n 192.168.0.1 (disable reverse dns resolution) speeds up for a lot of targets
not required DNS names
- nmap --system-dns 192.168.0.1 (alternative dns lookup methods) slower,
good for troubleshooting dns problems
- nmap - -dns-servers 8.8.8.8,8.8.4.4 192.168.0.0/24 (manually specify DNS server) dns not config on system
avoid logged scans in DNS servers

## Advanced Scanning
- nmap -sS 192.168.0.1 (tcp syn scan) does not log connection attempts: not guaranteed
- nmap -sT 192.168.0.1 (tcp connect scan) Establish direct connect, use sudo
- nmap -sU 192.198.0.1 (upd scan) layer of depth to scans
- nmap -sN 192.168.0.1 (tcp null scan) no tcp flags enabled response from behind a firewall
- nmap -sF 192.168.0.1 (tcp fin scan) sets fin active, illicit tcp ack
- nmap -sX 192.168.0.1 (xmas scan) urg fin psh flags set
- nmap -sA 192.168.0.1 (tcp ack scan) lookout for rst response. unfiltered is like a firewall
this is for getting the filtering status on a target
- nmap --scanflags *setflag* 192.168.0.1 (custom tcp scan)
- nmap -sO 192.168.0.1 (ip protocol scan) shows protocols used by the target
- nmap --send-eth 192.168.0.1 (send raw ethernet packets) sidesteps IP layer
- nmap --secnd-ip 192.168.0.1 (send ip packets) integrates with IP stack

## OS and Service Detection
- Nmap –O 192.168.0.1 (OS fingerprinting)
- Nmap –v –O 192.168.0.1 (verbose)
-- --osscan-guess (shows confidence %)
- Nmap –sV 192.168.0.1 (service detection) vendor and software version #s
-- --version-trace (shows details of activity)

## Time options
Timing Templates:
- T4 (milliseconds)
- T4s (seconds)
- T4m (minutes
- T4h (hours)
- 0 -5 (zero being slowest 5 being fastest)
- nmap -T5 192.168.0.1

- nmap --min-parallelism 100 192.168.0.1 (100 operations are performed at a given time) enhancing performance
- nmap --max-parallelism 5 192.168.0.1 (only 5 operations at a time) minimizes risk of packet flood
- nmap --min-hostgroup 20 192.168.0.0/24 (20 hosts at once)
- nmap --max-hostgroup 5 1952.168.0.0/24 (max targets scanning within the group) control network load
- nmap --initial-rtt-timeout 1000ms 192.168.0.1 (reduce packet retransmission) speed up scans
- nmap --max-rtt-timeout  100ms 192.168.0.1 (prevent from giving up on slow or unreliable connections)
- nmap --max-retries 5 192.168.0.1 (probe retransmissions) override default and troubleshoot connectivity
- nmap --ttl 2s 192.168.0.1 (set time to live) for slower connection, prevention expiration
- nmap --host-timeout 1s 192.168.0.1 (good for across WAN or distance)
- nmap --scan-delay 10ms 192.168.0.1 (helps with rate limiting and IDS)
- nmap --min-rate 50 192.168.0.1 (min packets per second)
- nmap --max-rate 50 192.168.0.1 (max packets per second)
- nmap --defeat-rst-ratelimit 192.168.0.1 (accelerate scans) less accurate 0.1 max can help with stealth
- 
## NMAP Scripting Engine (NSE)
[Nmap Scripting Engine NSE](https://nmap.org/book/nse.html) 
- nmap --script-help="all" (shows the scripts and categories) very long, use website below to search for what you need
- nmap --script-help-"*http* (wildcard ability to search)
[Scripts](https://nmap.org/nsedoc/scripts)
- nmap --script script.name 192.168.0.1
- 
- nmap -sn --script whois-* 192.168.0.1 (whois on the target)
- nmap --traceroute --script traceroute-geolocation domain.com (trace and geolocation on target)

## Output Options
[Output](https://nmap.org/book/man-output.html#:~:text=In%20addition%20to%20offering%20different%20output)
-oN text
-oX XML
-oG grepable
-oA all formats
-v verbose
-d debugging level

Nmap  --stats-every 2s 192.168.0.1 (shows updates every 2 seconds)
