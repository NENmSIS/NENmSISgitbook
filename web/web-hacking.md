# Web hacking

* Always try a URL encoded reverse shell with Burp in all the parameters send by POST. Use `;` between the legitimate parameter and the payload.

<figure><img src="../.gitbook/assets/image (4) (1).png" alt=""><figcaption><p>URL encoded payload after the filetype=jpg parameter</p></figcaption></figure>

* [File upload bypass](https://vulp3cula.gitbook.io/hackers-grimoire/exploitation/web-application/file-upload-bypass)
* Search for subdomains in the /etc/nginx folder. If found, add it to the /etc/hosts file.
* If the wep application has any mail service (with own domain), try snmpwalk. Even if is closed and there is nothing with nmap

#### Websocket

If we find an input box to enter anything in a web and in the source code is something like this `ws://soc-player.soccer.htb:9091` and the indicated port is opened we need to use a Websocket conection.

Here we have little information: [https://www.geeksforgeeks.org/what-is-web-socket-and-how-it-is-different-from-the-http/](https://www.geeksforgeeks.org/what-is-web-socket-and-how-it-is-different-from-the-http/)&#x20;

And here we have a post that explains how to exploit it in case of sql Websocket: [https://rayhan0x01.github.io/ctf/2021/04/02/blind-sqli-over-websocket-automation.html](https://rayhan0x01.github.io/ctf/2021/04/02/blind-sqli-over-websocket-automation.html) (where the data "id" is because we find in the source code  `var input = document.getElementById('id')`)



