# Kioptrix 1

<figure><img src="../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

[https://github.com/exploit-inters/OpenFuck](https://github.com/exploit-inters/OpenFuck)

The VM can't access to internet, so we replace this line in the OpenFuck.c file:

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

With this one:

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

Now we download the squared file and serve int with `python -m http.server`

After that, we compile the file and execute it.

Now that we are root, we need to add persistency in the victim machine, so we edit the /etc/crontab file to generate a reverse shell each 5 minutes:

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

So in order to obtain a reverse shell, we just need to start a listener on our attacker machine with `nc -nlvp 4441` and the magic is done.
