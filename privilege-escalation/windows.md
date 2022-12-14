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
