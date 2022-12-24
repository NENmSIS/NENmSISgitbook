# Brute force directories and files

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

```bash
wfuzz -c -f subdom -w Documents/Shoppy/subdomain -u 'http://shoppy.htb' -H "Host: FUZZ.shoppy.htb" --sc 200
```
