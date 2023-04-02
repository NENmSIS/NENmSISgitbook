# Kioptrix 1

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

[https://github.com/exploit-inters/OpenFuck](https://github.com/exploit-inters/OpenFuck)

The VM can't access to internet, so we replace this line in the OpenFuck.c file:

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

With this one:

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

Now we download the squared file and serve int with `python -m http.server`

After that, we compile the file and execute it.
