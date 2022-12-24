# Brute force directories and files

### Gobuster

```bash
gobuster vhost dir -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 50 -u http://soccer.htb
```

### Wfuzz

```bash
wfuzz -c -z file,/usr/share/seclists/Discovery/Web-Content/raft-large-directories.txt --hc 404 "http://shoppy.htb/FUZZ/"
```
