# 🌴 Machine's notes

### WEB:

* Always find subdomains: gobuster with `--append-domain`
* Look always for metadata in .pdf files `exiftool file.pdf` to discover versions and creator information
* After obtaining any hashes, is compulsory to crack them with john (using rockou) or hashcat
* After `sudo -l` , we can find something  like this:&#x20;

<figure><img src="../.gitbook/assets/imagen (11).png" alt=""><figcaption></figcaption></figure>

Execute it: `sudo -u deploy /home/deploy/password-manager` to have deploy's privileges

* If any user has docker acces, try `docker run -v /:/mnt --rm -it alpine chroot /mnt sh`as said in Gtfobins to obtain a root shell
* Always scan UDP ports with `-sU` in `nmap`
* Check the reverse connections (ping or revershe shell) with tcpdump. `sudo tcpdump -i tun0 port 4444`or `icmp` instead of `port 4444` &#x20;
* If there are special permissions to execute a file observed in `sudo -l` is importan to use it with the exact path



#### Pending tasks

* Nosqli

cat aliased con bat. se mira en which cat. para verlo es `cat -l java` o `cat -l bash`

#### S4vitar 27/12/22

* flameshot to capture the screen with tools
* launchpad.net to now the versions of ubuntu, apache... Siendo focal&#x20;
* cat targeted | grep http | grep -oP '\d{1,5}/tcp | awk '{print $1}' FS="/" | while read prot; do echo -e "\n\[+] Escaneo en http://192.168.111.36:$port:\n"; whatweb http:192.168.111.36:$port; done
* whatweb http://192.168.111.36:13380
* `curl -s -X GET "HTTP://LEEROY.htb:13389/" | grep -oP 'plugins/\K[^/]+' | sort -u`
* `searchsploit bbpress` \`searchsploit&#x20;
* [https://chat.openai.com/auth/login](https://chat.openai.com/auth/login)
* infosecmachines.io
* Importante como mejora la shell.
* println contraseñas jenkin
* `hashid` to know hash types

## Hacktricks&#x20;

root:$6$XePuRx/4eO0WuuPS$a0t5vIuIrBDFx1LyxAozOu.cVaww01u.6dSvct8AYVVI6ClJmY8ZZuPDP7IoXRJhYz4U8.DJUlilUw2EfqhXg.:0:0:root:/root:/bin/bash



john aes256 ticket: 581c7f88dc74998ed360fd0f9a7840a36a5c2dd78db363813677dd79f92215d2



/opt/specialfiles/carlos.keytab

/etc/krb5.keytab  carlos Password5



/home/carlos@inlanefreight.htb/.scripts/kerberos\_script\_test.sh

```
kinit svc_workstations@INLANEFREIGHT.HTB -k -t /home/carlos@inlanefreight.htb/.scripts/svc_workstations.kt
smbclient //dc01.inlanefreight.htb/svc_workstations -c 'ls'  -k -no-pass > /home/carlos@inlanefreight.htb/script-test-results.txt
```

svc\_workstations@inlanefreight.htb Password4



&#x20;zip Destiny2022!

docx 987654321

jason:C4mNKjAtL2dydsYa6

dennis  7AUgWWQEiMPdqx









&#x20;

/opt/specialfiles/carlos.keytab



\[+] Keytab File successfully imported.&#x20;

REALM : INLANEFREIGHT.HTB&#x20;

SERVICE PRINCIPAL : carlos/&#x20;

NTLM HASH : a738f92b3c08b424ec2d99589a9cce60&#x20;

AES-256 HASH : 42ff0baa586963d9010584eb9590595e8cd47c489e25e82aae69b1de2943007f AES-128 HASH : fa74d5abf4061baa1d4ff8485d1261c4

kira L0vey0u1!

login: mike password: 7777777

