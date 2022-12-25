# Soccer

<figure><img src="../../.gitbook/assets/imagen (9).png" alt=""><figcaption></figcaption></figure>

With gobuster, discover http://soccer.htb/tiny

<figure><img src="../../.gitbook/assets/imagen (1).png" alt=""><figcaption></figcaption></figure>

Searching in google `H3K Tiny File Manager` discover the default admin credentials `admin:admin@123` so we can exploit it:&#x20;

Upload a reverse shell

<figure><img src="../../.gitbook/assets/imagen (10).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/imagen (1) (2).png" alt=""><figcaption></figcaption></figure>

And now let's escalate privileges

<figure><img src="../../.gitbook/assets/imagen (5) (2).png" alt=""><figcaption></figcaption></figure>

In the nginx/sites-available folder there is a file `soc-player.htb` with this content:

```
server {
        listen 80;
        listen [::]:80;

        server_name soc-player.soccer.htb;

        root /root/app/views;

        location / {
                proxy_pass http://localhost:3000;
                proxy_http_version 1.1;
                proxy_set_header Upgrade $http_upgrade;
                proxy_set_header Connection 'upgrade';
                proxy_set_header Host $host;
                proxy_cache_bypass $http_upgrade;
        }

}
```

That includes a new subdomain `soc-player.soccer.htb`

``

<figure><img src="../../.gitbook/assets/imagen (8).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/imagen (7).png" alt=""><figcaption></figcaption></figure>

After creating an account and login, find and input box, inspect the source code and find a websocket, so try this post to sqli: [https://rayhan0x01.github.io/ctf/2021/04/02/blind-sqli-over-websocket-automation.html](https://rayhan0x01.github.io/ctf/2021/04/02/blind-sqli-over-websocket-automation.html)



