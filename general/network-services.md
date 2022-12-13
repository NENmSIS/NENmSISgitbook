# Network Services

### SMB (Server Message Block) 445/TCP[​](broken-reference) <a href="#smb-server-message-block-445tcp" id="smb-server-message-block-445tcp"></a>

Microsoft Windows OS have client and server SMB protocol support. Samba, an open source server that supports the SMB protocol, was released for Unix systems.

#### Enumerating SMB[​](broken-reference) <a href="#enumerating-smb" id="enumerating-smb"></a>

**enum4linux \[options] IP**[**​**](broken-reference)

* **-U** get Userlist
* **-M** get machine list
* **-N** get namelist dump (different from -U and -M)
* **-S** get sharelist
* **-P** get password policy information
* **-G** get group and member list
* **-a** all of the above (full basic enumeration)

#### Exploiting SMB[​](broken-reference) <a href="#exploiting-smb" id="exploiting-smb"></a>

**smbclient //\[IP]/\[SHARE]**[**​**](broken-reference)

* **-U \[name]** to specify the user
* **-p \[port]** to specify the port

Sometimes is enough trying with Anonymous username and no password.

### Telnet 23/TCP[​](broken-reference) <a href="#telnet-23tcp" id="telnet-23tcp"></a>

Allows with the use of a telnet client, to connect to and execute commands on a remote machine that's hosting a telnet server.

#### Enumerating Telnet[​](broken-reference) <a href="#enumerating-telnet" id="enumerating-telnet"></a>

* **nmap -n -sV -Pn --script "**_**telnet**_** and safe" -p 23 \\\<IP>**

#### Exploiting SMB[​](broken-reference) <a href="#exploiting-smb-1" id="exploiting-smb-1"></a>

**Bruteforce (Hacktricks)**[**​**](broken-reference)

**telnet \[IP]\[Port]**[**​**](broken-reference)

### FTP (File Transfer Protocol) 21/TCP[​](broken-reference) <a href="#ftp-file-transfer-protocol-21tcp" id="ftp-file-transfer-protocol-21tcp"></a>

#### Enumerating FTP[​](broken-reference) <a href="#enumerating-ftp" id="enumerating-ftp"></a>

With nmap

#### Exploiting FTP[​](broken-reference) <a href="#exploiting-ftp" id="exploiting-ftp"></a>

**Bruteforce (Hacktricks)**[**​**](broken-reference)

* Ex: **nmap --script ftp-\* -p 21 \\\<ip>**

**Anonymous login**[**​**](broken-reference)

* **ftp IP** (anonymous, anonymous)

### NFS (Network File System) 2049/TCP[​](broken-reference) <a href="#nfs-network-file-system--2049tcp" id="nfs-network-file-system--2049tcp"></a>

#### Enumerating NFS[​](broken-reference) <a href="#enumerating-nfs" id="enumerating-nfs"></a>

**Useful nmap scripts**[**​**](broken-reference)

```
nfs-ls #List NFS exports and check permissions
nfs-showmount #Like showmount -e
nfs-statfs #Disk statistics and info from NFS share
```

**Mounting**[**​**](broken-reference)

To know **which folder** has the server available to mount you ask it:

Then mount it using:

```
mount -t nfs [-o vers=2] <ip>:<remote_folder> <local_folder> -o nolock
```

#### Exploiting NFS[​](broken-reference) <a href="#exploiting-nfs" id="exploiting-nfs"></a>

[NSFShell](https://www.pentestpartners.com/security-blog/using-nfsshell-to-compromise-older-environments/)

### SMTP 25,465,587/TCP[​](broken-reference) <a href="#smtp-25465587tcp" id="smtp-25465587tcp"></a>

#### Enumerating SMTP[​](broken-reference) <a href="#enumerating-smtp" id="enumerating-smtp"></a>

#### Exploiting SMTP[​](broken-reference) <a href="#exploiting-smtp" id="exploiting-smtp"></a>

### Mysql 3306/TCP[​](broken-reference) <a href="#mysql-3306tcp" id="mysql-3306tcp"></a>

#### Connect[​](broken-reference) <a href="#connect" id="connect"></a>

**Local**[**​**](broken-reference)

```
mysql -u root # Connect to root without password
mysql -u root -p # A password will be asked (check someone)
```

**Remote**[**​**](broken-reference)

```
mysql -h <Hostname> -u root
mysql -h <Hostname> -u root@localhost
```

#### Enumerating Mysql[​](broken-reference) <a href="#enumerating-mysql" id="enumerating-mysql"></a>

```
nmap -sV -p 3306 --script mysql-audit,mysql-databases,mysql-dump-hashes,mysql-empty-password,mysql-enum,mysql-info,mysql-query,mysql-users,mysql-variables,mysql-vuln-cve2012-2122 <IP>
```

**Brute force (Hacktricks)**[**​**](broken-reference)

#### Exploiting Mysql[​](broken-reference) <a href="#exploiting-mysql" id="exploiting-mysql"></a>

### Tcpdump[​](broken-reference) <a href="#tcpdump" id="tcpdump"></a>

### SSH[​](broken-reference) <a href="#ssh" id="ssh"></a>

```
ssh username@MACHINE_IP
scp username@MACHINE_IP:/path/archive ~ # To transfer files securely
scp fileto transfer username@MACHINE_IP:/destinypath
```
