# The Bash Environment

### The Bash Environment

### Envoronment Variables[​](broken-reference) <a href="#envoronment-variables" id="envoronment-variables"></a>

To view the contents of a given environment variable:

```
echo $PATH
echo $USER
echo $HOME
```

To define a variable, we can use `export` and use it after:

To view the defaul environment variables, we can use `env`

### Bash History[​](broken-reference) <a href="#bash-history" id="bash-history"></a>

To display the bash history, we can use the `history` command. After using it, to re-type automatically one line, is possible to use `!` followed by the target line.

And with `!!`, is possible to repeat the last command. Is also possible to use the _reverse-i-search_ facility to get the most recent command that contains one string. For that, we should hold down `Ctrl` and pressing `R` will invoke the facility. We just need to write one word and press `Enter` to execute it.

### Piping and Redirection[​](broken-reference) <a href="#piping-and-redirection" id="piping-and-redirection"></a>

Every program run from the command line has three data streams connected to it that serve as communication channels with the external environment.

| Stream Name              | Description                                    |
| ------------------------ | ---------------------------------------------- |
| Standard Input (STDIN)   | Data fed into the program                      |
| Standard Output (STDOUT) | Output from the program (defaults to terminal) |
| Standard Error (STDERR)  | Error messages (defaults to terminal)          |

Piping (using the | operator) and redirection (using the > and < operators) connects these streams between program and files.

#### Redirecting to a New File[​](broken-reference) <a href="#redirecting-to-a-new-file" id="redirecting-to-a-new-file"></a>

If the file doesn't exists, it will be created and the output redirected. If the file already exists, that file's content will be replaced.

```
echo "test" > redirection_file.txt
```

#### Redirecting to a Existing File[​](broken-reference) <a href="#redirecting-to-a-existing-file" id="redirecting-to-a-existing-file"></a>

To append additional data to an existing file.

```
echo "test" >> redirection_file.txt
```

#### Redirecting from a File[​](broken-reference) <a href="#redirecting-from-a-file" id="redirecting-from-a-file"></a>

Using the `<` operator, we can send data "other way". In the example, we count the number of characters.

```
wc -m < redirection_file.txt
```

#### Piping[​](broken-reference) <a href="#piping" id="piping"></a>

It is used to redirect the output of one command as input of another.

```
cat redirection_file.txt | wc -m
```

### Text Searching and Manipulation[​](broken-reference) <a href="#text-searching-and-manipulation" id="text-searching-and-manipulation"></a>

#### grep[​](broken-reference) <a href="#grep" id="grep"></a>

`grep` searches text files for the occurrence of a given regular expression and outputs any line containing a match to the standard output, the screen. Some of the most commonly used switches are `-r` for recursive searching and `-i` to ignore text case.

```
ls -la /usr/bin | grep x86
```

[https://quickref.me/grep](https://quickref.me/grep)

#### sed[​](broken-reference) <a href="#sed" id="sed"></a>

It performs text editing on a stream of text, either a set of specific files or standard output.

```
echo "That is so easy" | sed 's/easy/difficult/'
        That is so difficult
```

[https://quickref.me/sed](https://quickref.me/sed)

#### cut[​](broken-reference) <a href="#cut" id="cut"></a>

It is used to extract a section of text from a line and output it to the standard output. Some switches are: `-f` for the field number we're cutting and `-d` for the field delimiter.

```
echo "This is a text, but also a string" | cut -f 2 -d "," 
        but also a string

cut -f 1 -d ":" /etc/passwd
        root
        daemon
        bin
```

[https://bencane.com/2012/10/22/cheat-sheet-cutting-text-with-cut/](https://bencane.com/2012/10/22/cheat-sheet-cutting-text-with-cut/)

#### awk[​](broken-reference) <a href="#awk" id="awk"></a>

Is a programming language designed for text precessing and is typically used as a data extraction and reporting tool. The difference with `cut` it that `awk` can accept more than a single character.

```
echo "hello::there::friend" | awk -F "::" '{print $1, $3}'
        hello friend
```

[https://quickref.me/awk](https://quickref.me/awk)

#### Other commands[​](broken-reference) <a href="#other-commands" id="other-commands"></a>

There are other useful commands that we can use to filter the data: `head`, `tail`, `sort`, `uniq`

### Editing Files from the Command Line[​](broken-reference) <a href="#editing-files-from-the-command-line" id="editing-files-from-the-command-line"></a>

#### nano[​](broken-reference) <a href="#nano" id="nano"></a>

[https://www.nano-editor.org/dist/latest/cheatsheet.html](https://www.nano-editor.org/dist/latest/cheatsheet.html)

#### vi[​](broken-reference) <a href="#vi" id="vi"></a>

[https://quickref.me/vim](https://quickref.me/vim) [https://en.wikibooks.org/wiki/Learning\_the\_vi\_Editor/vi\_Reference](https://en.wikibooks.org/wiki/Learning\_the\_vi\_Editor/vi\_Reference)

### Comparing Files[​](broken-reference) <a href="#comparing-files" id="comparing-files"></a>

#### comm[​](broken-reference) <a href="#comm" id="comm"></a>

Compares two text files, displaying the lines that are unique to each one, as well as the lines they have in common. It outputs three space-offset columns: the first contains lines that are unique to the first file or argument; the second contains lines that are unique to the second file or argument; and the third column contains lines that are shared by both files. The -nswitch, where “n” is either 1, 2, or 3, can be used to suppress one or more columns, depending on the need.

```
comm 1.txt 2.txt
                            10.10.10.1
                            10.10.10.2
10.10.10.3
10.10.10.4
                10.10.10.5
                10.10.10.6

```

#### diff[​](broken-reference) <a href="#diff" id="diff"></a>

Is used to detect differences between files, similar to the comm command. Some switches are context format `-c` and the unified format `-u`.

```
diff 1.txt 2.txt
    3,4c3,4
    < 10.10.10.3
    < 10.10.10.4
    ---
    > 10.10.10.5
    > 10.10.10.6
```

#### vimdiff[​](broken-reference) <a href="#vimdiff" id="vimdiff"></a>

Opens vim with multiple files, one in each window.

### Managin Processes[​](broken-reference) <a href="#managin-processes" id="managin-processes"></a>

#### Background Processes (bg)[​](broken-reference) <a href="#background-processes-bg" id="background-processes-bg"></a>

The quickest way to background a process is to append an ampersand `&` to the end of the command to send it to the background immediately after it starts. The job is now running in the background and we can continue using the terminal as we wish.

```
ping -c 400 localhost > ping_results.txt &
```

Once a job has been suspended, we can resume it in the background by using the `bg` command.

#### Jobs Control: jobs and fg[​](broken-reference) <a href="#jobs-control-jobs-and-fg" id="jobs-control-jobs-and-fg"></a>

To cancel a command, we use `Ctrl` `C` and to suspend a job, `Ctrl` `Z`. The built-in `jobs` utility lists the jobs that are running in the current terminal session, while `fg` returns a job to the foreground.

```
ping -c 400 localhost > ping_results.txt
^Z
find / -name  sbd.exe
^Z
jobs    
        [1]  - suspended  ping -c 400 localhost > ping_results.txt
        [2]  + suspended  find / -name sbd.exe
fg %1
        [1]  - continued  ping -c 400 localhost > ping_results.txt
^C
jobs
        [2]  + suspended  find / -name sbd.exe
fg
        [2]  - continued  find / -name sbd.exe
```

There are various ways to refer to a job in the shell.

* **%Number** : Refers to a job number such as %1 or %2
* **%String** : Refers to the beginning of the suspended command’s name such as
* %commandNameHere or %ping
* **%+ OR %%** : Refers to the current job
* **%-** : Refers to the previous job

#### Process Control: ps and kill[​](broken-reference) <a href="#process-control-ps-and-kill" id="process-control-ps-and-kill"></a>

One of the most useful commands to monitor processes on mostly any Unix-like operating systemis `ps` (short for process status). Unlike the `jobs` command, `ps` lists processes system-wide, not only for the current terminal session. This utility is considered a standard on Unix-like OSes and its name is so well-recognized that even on Windows PowerShell, `ps` is a predefined command alias for the Get-Process cmdlet, which essentially serves the same purpose.

As an example, let’s start the vi text editor and then try to find its process ID (PID) from the command line by using the `ps` command:

```
vi
^Z
ps -ef 
        UID          PID    PPID  C STIME TTY          TIME CMD
        kali      763720  482169  0 14:11 pts/3    00:00:00 vim
# The -ef options we used above stand for:
#   e:select all processes
#   f: display full format listing (UID, PID, PPID, etc.)
# for more info man ps
ps -fC vim
```

Let’s say we now want to stop the vi process without interacting with the GUI. In order to use kill, we need the PID of the process we want to send the signal to.

### File and Command Monitoring[​](broken-reference) <a href="#file-and-command-monitoring" id="file-and-command-monitoring"></a>

#### tail[​](broken-reference) <a href="#tail" id="tail"></a>

The most common use of `tail` is to monitor log file entries as they are being written. The `-f` option (follow) is very useful as it continuously updates the output as the target file grows. Another convenient switch is `-nX`, which outputs the last “X” number of lines, instead of the default value of 10.

#### watch[​](broken-reference) <a href="#watch" id="watch"></a>

The `watch` command is used to run a designated command at regular intervals. By default, it runsevery two seconds but we can specify a different interval by using the `-n X` option to have it run every “X” number of seconds.

### Downloading Files[​](broken-reference) <a href="#downloading-files" id="downloading-files"></a>

#### wget[​](broken-reference) <a href="#wget" id="wget"></a>

The wget command, which we will use extensively, downloads files using the HTTP/HTTPS and FTP protocols.

```
# The switch -O save the destination file with a different name on the local machine
wget -O new_name.pdf https://www.testweb.com/reports/document.pdf
```

#### curl[​](broken-reference) <a href="#curl" id="curl"></a>

`curl` is a tool to transfer data to or from a server using a host of protocols including IMAP/S, POP3/S, SCP, SFTP, SMB/S, SMTP/S, TELNET, TFTP, and others. A penetration tester can use this to download or upload files and build complex requests. Its most basic use is very similar to wget.

```
curl -o new_name.pdf https://www.testweb.com/reports/document.pdf
```

#### axel[​](broken-reference) <a href="#axel" id="axel"></a>

`axel` is a download accelerator that transfers a file from a FTP or HTTP server through multiple connections. This tool has a vast array of features, but the most common is `-n`, which is used to specify the number of multiple connections to use. In the following example, we are also using the `-a` option for a more concise progress indicator and -o to specify a different file name for the downloaded file.

```
axel -a -n 20 -o new_name.pdf https://www.testweb.com/reports/document.pdf
```
