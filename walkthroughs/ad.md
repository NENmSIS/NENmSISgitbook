# AD

## Nmap

```
80/tcp    open     http         syn-ack ttl 127
135/tcp   open     msrpc        syn-ack ttl 127
139/tcp   open     netbios-ssn  syn-ack ttl 127
445/tcp   open     microsoft-ds syn-ack ttl 127
5985/tcp  open     wsman        syn-ack ttl 127
47001/tcp open     winrm        syn-ack ttl 127

```

#### Kerberoasting for Windows

Because we don't know the winrm credentials, we use the web shell provided to create a revershe shell. On the listener machine:

```
nc -lvnp 8888
```

And on the webshell we paste the revershe shell 'Powershell#3 base64' from [https://www.revshells.com/](https://www.revshells.com/)

And now we use the commands from the section.

