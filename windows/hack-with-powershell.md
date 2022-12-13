# Hack with Powershell

First, we connect to the machine: `xfreerdp /u:Administrator /p:BHN2UVw0Q /cert:ignore /v:10.10.60.147`

#### Basic Powershell commands[​](broken-reference) <a href="#basic-powershell-commands" id="basic-powershell-commands"></a>

* Find a file with a particular name

```
Get-ChildItem -Path C:\ -Include *nameofthefile* -File -Recurse -ErrorAction SilentlyContinue
```

* Specify the contents of the file

```
Get-Content "C:\Program Files\interesting-file.txt.txt"
```

* Get the MD5 hash of a file

```
Get-FileHash -Path "C:\Program Files\interesting-file.txt.txt" -Algorithm MD5
```

* Get the current directory
* Make a request to a web server
* Base64 decode

```
Get-ChildItem -Path C:/ -Include b64.txt -Recurse -File
```

#### Enumeration[​](broken-reference) <a href="#enumeration" id="enumeration"></a>

* List users
* Get the User with an specific SID

```
Get-LocalUser -SID "S-1-5-21-1394777289-3961777894-1791813945-501"
```

* Get users without passwords

```
Get-LocalUser | Where-Object -Property PasswordRequired -Match false
```

* Get Localgroups
* Get the IP address info
* Count listening ports

```
Get-NetTCPConnection | Where-Object -Property State -Match Listen | measure
```

* List listening ports

```
Get-NetTCPConnection | Where-Object -Property State -Match Listen
```

* Show the number of patches
* Find the contents of a backup file

```

Get-ChildItem -Path C:\ -Include *.bak* -File -Recurse -ErrorAction SilentlyContinue

Get-Content "C:\Program Files (x86)\Internet Explorer\passwords.bak.txt"
```

* Find a file containing a string
* List processes
* Get scheduled task

```
Get-ScheduledTask -TaskName new-sched-task
```

* Know the owner of a path/directory

#### Resources[​](broken-reference) <a href="#resources" id="resources"></a>

* [Powershell verbs](https://docs.microsoft.com/en-us/powershell/scripting/developer/cmdlet/approved-verbs-for-windows-powershell-commands?view=powershell-7)
* [Operators](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/where-object?view=powershell-7.2\&viewFallbackFrom=powershell-6)
* [Scripting Guide](https://learnxinyminutes.com/docs/powershell/)
