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

#### Config Files

```
/etc/ftpusers
/etc/ftp.conf
/etc/proftpd.conf
/etc/vsftpd.conf
```

#### Post Exploitation

The default configuration of vsFTPd can be found in `/etc/vsftpd.conf`. In here, you could find some dangerous settings:

* `anonymous_enable=YES`
* `anon_upload_enable=YES`
* `anon_mkdir_write_enable=YES`
* `anon_root=/home/username/ftp` - Directory for anonymous.
* `chown_uploads=YES` - Change ownership of anonymously uploaded files
* `chown_username=username` - User who is given ownership of anonymously uploaded files
* `local_enable=YES` - Enable local users to login
* `no_anon_password=YES` - Do not ask anonymous for password
* `write_enable=YES` - Allow commands: STOR, DELE, RNFR, RNTO, MKD, RMD, APPE, and SITE



### SNMP

```
snmpwalk -v2c -c internal mentorquotes.htb
```

## IMAP

Interesting links with commands:

* &#x20;[https://www.atmail.com/blog/imap-commands/](https://www.atmail.com/blog/imap-commands/)
* [https://vk9-sec.com/25110143-tcp-smtppop3imap-enumeration/](https://vk9-sec.com/25110143-tcp-smtppop3imap-enumeration/)

## IPMI

* [https://www.rapid7.com/blog/post/2013/07/02/a-penetration-testers-guide-to-ipmi/](https://www.rapid7.com/blog/post/2013/07/02/a-penetration-testers-guide-to-ipmi/)



