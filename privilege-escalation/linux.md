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

Enumerate information from cron:

```
crontab -l
ls -al /var/spool/cron
ls -al /etc/ | grep cron
ls -al /etc/cron*
cat /etc/cron*
cat /etc/at.allow
cat /etc/at.deny
cat /etc/cron.allow
cat /etc/cron.deny
cat /etc/crontab
cat /etc/anacrontab
cat /var/spool/cron/crontabs/root
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

