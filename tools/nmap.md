# Nmap

#### Common switches[​](broken-reference) <a href="#common-switches" id="common-switches"></a>

[Nmap Cheat Sheet](https://www.stationx.net/nmap-cheat-sheet/)

```bash
nmap -p- --open -sS --min-rate 5000 -vvv -n -Pn <IP>
```

### Live Host Discovery and Port Scan[​](broken-reference) <a href="#live-host-discovery-and-port-scan" id="live-host-discovery-and-port-scan"></a>

#### Basisc scan types[​](broken-reference) <a href="#basisc-scan-types" id="basisc-scan-types"></a>

* **-sT** TCP Connect Scans (Default)
* **-sS** SYN "Half-open" Scans. With sudo (Default when using sudo)
* **-sU** UDP Scans. With sudo
* **-sN** TCP Null Scans. No response, port open or filtered.
* **-sF** TCP FIN Scans. No response, port open or filtered.
* **-sX** TCP Xmas Scans. No response, port open or filtered.
* **-sA** TCP ACK Scan. The target would respond to the ACK with RST regardless of the port. FIREWALL EVASION. We can learn wich ports are not filtered.

#### ICMP Network Scanning[​](broken-reference) <a href="#icmp-network-scanning" id="icmp-network-scanning"></a>

* **nmap -sn 10.10.0.(1-254 or /24)**
  * The **-sn** switch tells Nmap not to scan any ports

#### Port specification[​](broken-reference) <a href="#port-specification" id="port-specification"></a>

* **-p 21** Port 21
* **-p 21-100** Port range
* **-p-** Scan all ports (The 65535)
* **--top-ports 2000** Scan the top 200 ports
* **-F** Most commont 100 ports

#### Service and Version Detection[​](broken-reference) <a href="#service-and-version-detection" id="service-and-version-detection"></a>

* **-sV** Attempts to determine the version of the service running on port
* **-A** Enable OS detection, version detection, script scanning, and traceroute

#### OS Detection[​](broken-reference) <a href="#os-detection" id="os-detection"></a>

* **-O** Remote OS detection using TCP/IP stack fingerprinting

#### Timing and Performance (0-5) Default 3[​](broken-reference) <a href="#timing-and-performance-0-5-default-3" id="timing-and-performance-0-5-default-3"></a>

* **-T0** Paranoid IDS evasion. 5 minutes between each probe.
* **-T4** Often used during CTFs
* **-T5** Insane speeds scan (extraordinarily fast network)
* **--min-rate 15**. Rate >= 15 packets/sec
* **--max-rate 50**. Rate <= 50 packets/sec
* **--min-parallelism 100** At least 100 probes in parallel

#### Getting More Details[​](broken-reference) <a href="#getting-more-details" id="getting-more-details"></a>

* **--reason**. Gives us the explicit reason why Nmap concluded that the system is up or a particular port is open.
* **-v** or **-vv** verbosity
* **-d** or **-dd** debuggin

#### Firewall Evasion[​](broken-reference) <a href="#firewall-evasion" id="firewall-evasion"></a>

* **-f** Fragment packets in 8 Bytes
* **--mtu number** Must be a multiple of 8
* **--scan-delay "time"ms** Used to add a delay between packets sent
* **--badsum** Used to generate an invalid checksum for packets. Instead of dropping it, firewalls may respond automatically, without bothering to check the checksum of the packet. Can be used to determine the presence of a firewall/IDS.

#### Spoofing and Decoys[​](broken-reference) <a href="#spoofing-and-decoys" id="spoofing-and-decoys"></a>

* **nmap -S SPOOFED\_IP 10.10.208.223**. Only works if we can monitor the network for responses.
* **nmap -e NET\_INTERFACE -S SPOOFED\_IP 10.10.208.223**
* **nmap --spoof-mac SPOOFED\_MAC -e NET\_INTERFACE -S SPOOFED\_IP 10.10.208.223** Only works in the same subnet.
* **nmap -D 10.10.0.1,10.10.0.2,RND,RND,ME 10.10.208.223** -D is decoy ip(another IP that send the packages). RND is random IP. ME is my ip. The last one is the destination IP.
* **nmap -sI ZOMBIE\_IP 10.10.208.223** To use an Idle/Zombie Scan

#### Detect live hosts[​](broken-reference) <a href="#detect-live-hosts" id="detect-live-hosts"></a>

* **-PR -sn** ARP Scan. In the same subnet
* **-PE -sn** ICMP Echo Scan. Usually blocked
* **-PP -sn** ICMP Timestamp Scan. Expect ICMP Type 14. Usefull to bypass the Echo block.
* **-PM -sn** ICMP Adress Mask Scan. Similar to -PP, type 18.
* **-PS22,80,443 -sn** TCP SYN Ping Scan. Use with sudo to avoid 3-way handshake. GOOD RESULTS
* **-PA22,80,443 -sn** TCP ACK Ping Scan. Expect RST.
* **-PU53,161,162 -sn** UDP Ping Scan. Expect the response of a closed port from an active host.
  * **-sn** Host discovery only
  * **-n** no DNS lookup
  * **-R** reverse-DNS lookup for all hosts

***

#### Disable Host discovery[​](broken-reference) <a href="#disable-host-discovery" id="disable-host-discovery"></a>

* **-Pn** Host discovery disabled

### Post Port Scan[​](broken-reference) <a href="#post-port-scan" id="post-port-scan"></a>

#### Service Detection[​](broken-reference) <a href="#service-detection" id="service-detection"></a>

* **-sV** Service and version information for open ports. -sS is not possible when using -sV. TCP 3-WHandshake must be done
  * **--version-intensity LEVEL** Between 0 and 9.

#### OS Detection[​](broken-reference) <a href="#os-detection-1" id="os-detection-1"></a>

* **-O**

#### Traceroute[​](broken-reference) <a href="#traceroute" id="traceroute"></a>

* **--traceroute** Starts with a packet of low TTL and keeps increasing until it reaches the target.

#### NSE Scripts[​](broken-reference) <a href="#nse-scripts" id="nse-scripts"></a>

There are many categories available: [NSE](https://nmap.org/book/nse-usage.html)

* **-sC** or **--script=default** Run the scripts in default

Usage: --script=script-name

* **--script=safe**
* **--script=smb-enum-users, smb-enum-shares**
* **--script "ftp\*"** patern

Where can i find the scripts? /usr/share/nmap/scripts and /usr/share/nmap/scripts/script.db

Also, we can find the scripts information with `nmap --script-help "ftp*"`

#### Saving the Output[​](broken-reference) <a href="#saving-the-output" id="saving-the-output"></a>

There are 3 formats: normal, grepable and XML.

* **-oN FILENAME** Normal format
* **-oG FILENAME** Grepable format.
  * **grep KEYWORD TEXT\_FILE**
* **-oX** XML Format. To process the output in other programs.
* **-oA** All three formats

#### Examples[​](broken-reference) <a href="#examples" id="examples"></a>

* **sudo nmap -Pn -A -T5 IPDEST**
