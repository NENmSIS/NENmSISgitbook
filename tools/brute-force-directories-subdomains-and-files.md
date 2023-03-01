# Brute force directories,subdomains and files

## Active

### Gobuster

```bash
gobuster vhost dir -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 50 -u http://soccer.htb
# -c <Cookie:> Cookie header from burp. Between quotation marks
```

Find subdomains

```bash
gobuster vhost -w /usr/share/seclists/Discovery/DNS/bitquark-subdomains-top100000.txt -t 60 -u shoppy.htb
```

### Wfuzz

```bash
wfuzz -c -z file,/usr/share/seclists/Discovery/Web-Content/raft-large-directories.txt --hc 404 "http://shoppy.htb/FUZZ/"
```

Subdomains

```
wfuzz -c -f subdomains.txt -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -u "http://mentorquotes.htb/" -H "Host: FUZZ.mentorquotes.htb" --hl 9
```

```bash
wfuzz -c -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt --hc 400,404,403,301 -H "Host: FUZZ.shoppy.htb" -u http://shoppy.htb -t 100
```

#### Dirsearch

```
dirsearch -u http://api.mentorquotes.htb/
```

### ffuf

[https://github.com/ffuf/ffuf](https://github.com/ffuf/ffuf)

#### Virtual Hosts Discovery

1. Figure out the response lengt of false positive

```
curl -s H "Host: noexiste.inlanefreight.com" http://<IP> |wc -c
# 10986
```

2. Filter out the responses of lengt 10986

```bash
ffuf -c -w /path/to/wordlist -u http://inlanefreight.com "Host: FUZZ.inlanefreight.com" -fs 10986
```

### Active Subdomain Enumeration

#### 1. Identifying Nameservers

```bash
nslookup -type=NS zonetransfer.me
#response: zonetransfer.me nameserver = nsztm1.digi.ninja
```

#### 2. Testing fo ANY and AXFR Zone Transfer

```
nslookup -type=any -query=AXFR zonetransfer.me nsztm1.digi.ninja
```

## Passive

### sublist3r

Python tool designed to enumerate subdomains of websites using OSINT

[https://github.com/aboul3la/Sublist3r](https://github.com/aboul3la/Sublist3r)

```
sublist3r -d https://<FQDN>
```

### crt.sh

Extract subdomains with SSL/TLS certificates

```
https://crt.sh/?q=<FQDN>
```

### TheHarvester

[https://github.com/laramies/theHarvester](https://github.com/laramies/theHarvester)

```
theHarvester -d <TARGET> -b <SOURCE>
```
