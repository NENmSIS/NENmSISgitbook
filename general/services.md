# Services

## FTP

#### Banner Grabbing

```bash
nc -vn <IP> 21
openssl s_client -connect crossfit.htb:21 -starttls ftp #Get certificate if any
```

#### Anonymous Login

_anonymous : anonymous_\
_anonymous :_\
_ftp : ftp_

```bash
ftp <IP>
```

#### Nmap scripts

```bash
nmap --script ftp-* -p 21 <ip>
```

