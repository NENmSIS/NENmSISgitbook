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
*
