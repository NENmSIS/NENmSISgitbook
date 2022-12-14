# Photobomb

After Nmap (22,80), edit the /etc/hosts file and inspect the web application.

<figure><img src="../../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

Inspecting the code of `photobomb.htb`, there is this line: `http://pH0t0:b0Mb!@photobomb.htb/printer`

It logs in automatically.

Inspect the POST with burp to download a PNG photo and find diferrent parameteres, so lets use an encoded URL with the parameter to obtain a reverse shell:

<figure><img src="../../.gitbook/assets/image (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

