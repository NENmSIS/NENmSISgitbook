# Brute force directories,subdomains and files

### Gobuster

```bash
gobuster vhost dir -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 50 -u http://soccer.htb
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
