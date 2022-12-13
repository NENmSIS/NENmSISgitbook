# Passive Reconnaisance

### Passive Reconnaisance[​](broken-reference) <a href="#passive-reconnaisance" id="passive-reconnaisance"></a>

#### WHOIS[​](broken-reference) <a href="#whois" id="whois"></a>

Is a request and response protocol that follows the [RFC3912](https://www.ietf.org/rfc/rfc3912.txt) specification. The WHOIS server replies with various information related to the domain requested. The syntax is:

#### Nslookup and dig[​](broken-reference) <a href="#nslookup-and-dig" id="nslookup-and-dig"></a>

**Nslookup (Name Server Look Up)**[**​**](broken-reference)

To find the IP adress of a domain name.

```
nslookup DOMAIN_NAME
snlookup OPTIONS DOMAIN_NAME SERVER #OPTIONS: contains the query type, SERVER: The DNS server that you want to query, example 8.8.8.8, 1.1.1.1...
nslookup -type=A tryhackme.com 1.1.1.1
```

* Query type: A(ipv4), AAAA(ipv6), CNAME(Canonical name), MX(Mail servers), SOA(Start of authority), TXT(TXT records)

**Dig (Domain Information Groper)**[**​**](broken-reference)

For more advanced DNS queries and additional information.

```
dig DOMAIN_NAME
dig DOMAIN_NAME TYPE
dig @SERVER DOMAIN_NAME TYPE
```

#### DNSDumpster[​](broken-reference) <a href="#dnsdumpster" id="dnsdumpster"></a>

In this website, we can also find subdomains. [DNSDumpster](https://dnsdumpster.com/)

#### Shodan[​](broken-reference) <a href="#shodan" id="shodan"></a>

Learn information about the client's network without actively connecting to it. [Shodan.io](https://www.shodan.io/)

### Active Reconnaissance[​](broken-reference) <a href="#active-reconnaissance" id="active-reconnaissance"></a>

#### Web browser[​](broken-reference) <a href="#web-browser" id="web-browser"></a>

**FoxyProxy**[**​**](broken-reference)

Lets change the proxy server

**User-Agent Switcher and Manager**[**​**](broken-reference)

Access the webpage from a diffeernt OS or different web browser

**Wappalyzer**[**​**](broken-reference)

Provides indights about the technologies used on the visited websites

#### Ping[​](broken-reference) <a href="#ping" id="ping"></a>

Sends an ICMP Echo packet to a remote system.

#### Traceroute[​](broken-reference) <a href="#traceroute" id="traceroute"></a>

```
traceroute HOST # tracert in MS Windows
```

The TTL is decremented by one before passing it to the next router. If the TTL reaches 0, it will be dropped.

#### Telnet (Teletype Network)[​](broken-reference) <a href="#telnet-teletype-network" id="telnet-teletype-network"></a>

Protocol to comunicate with a remote system. It's usefull to collect a server's banner.

#### Netcat[​](broken-reference) <a href="#netcat" id="netcat"></a>

Has different applications. Can function as a client and as a server. It can be use also to collect a server's banner.

```
nc HOST PORT    
nc -nlvp 1234 # Opens a port and listen
```

* **-l** listen mode
* **-p** Port. Should appear just before the port number
* **-n** Numeric only, no resolution
* **-v** Verbose
* **-k** Keep listening after client disconnects
