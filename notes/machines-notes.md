# 🌴 Machine's notes

### WEB:

* Always find subdomains: gobuster with `--append-domain`
* Look always for metadata in .pdf files `exiftool file.pdf` to discover versions and creator information
* After obtaining any hashes, is compulsory to crack them with john (using rockou) or hashcat
* After `sudo -l` , we can find something  like this:&#x20;

<figure><img src="../.gitbook/assets/imagen (11).png" alt=""><figcaption></figcaption></figure>

Execute it: `sudo -u deploy /home/deploy/password-manager` to have deploy's privileges

* If any user has docker acces, try `docker run -v /:/mnt --rm -it alpine chroot /mnt sh`as said in Gtfobins to obtain a root shell

#### Pending tasks

* Nosqli
