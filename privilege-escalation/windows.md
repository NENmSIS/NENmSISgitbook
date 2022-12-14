# Windows

## Windows Enumeration

### System enumeration

```bash
systeminfo | findstr /B /C:"OS Name" /C:"OS Version" #Operating system's name versions and architecture
systeminfo #Determine what Windows hotfixes or patches have been installed
wmic qfe #To see in deatail the hotfixes or patches
wmic logicaldisk get caption #Enumerate the drives attached
tasklist /SVC #The process that are running
```

### User and group enumeration

```bash
whoami #The current user
whoami /priv #Privileges of the current user
whoami /groups #Groups that our account is part of
net user #User accounts that are active on the system
net user <username> #Additional information about a particular user
net localgroup administrators #"Administradores" en Español. Users that are part of the administrative group 
```

### Network enumeration

```bash
ipconfig /all #Enumerate target network interfaces and their details
route print #Routing table
netstat -ano #Active connections with their respective ports and process IDs
```

### Password enumeration

```bash
findstr /si password *.doc *.txt *.ini *.config #Locate string in files
dir /s *pass* == *cred* == *ssh* == *.config* #Strings in specific services
reg query HKLM /f password /t REG_SZ /s #Password within the registry. HKEY_LOCAL_MACHINE in this example
reg query HKCU /f password /t REG_SZ /s #HKEY_CURRENT_USER
```

### Firewall and antivirus enumeration

