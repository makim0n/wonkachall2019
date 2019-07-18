### 172.16.42.11

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


### 172.16.42.5

```bash
sudo masscan -e tun0 -p0-65535,U:0-65535 --rate 1000 172.16.42.5

Discovered open port 49669/tcp on 172.16.42.5                                  
Discovered open port 445/tcp on 172.16.42.5                                    
Discovered open port 53/udp on 172.16.42.5                                     
Discovered open port 53/tcp on 172.16.42.5                                     
Discovered open port 3268/tcp on 172.16.42.5                                   
Discovered open port 50206/tcp on 172.16.42.5                                  
Discovered open port 593/tcp on 172.16.42.5                                    
Discovered open port 636/tcp on 172.16.42.5                                    
Discovered open port 49687/tcp on 172.16.42.5       

cat out_mass_5 | cut -d ' ' -f4 | sed 's/\/.*$//' | tr '\n' ','
49669,445,53,53,3268,50206,593,636,49687

sudo nmap -sT -sV -O -T4 -vvv --version-intensity=8 -sC -p49669,445,53,53,3268,50206,593,636,49687 172.16.42.5

PORT      STATE SERVICE       REASON  VERSION
53/tcp    open  domain        syn-ack Microsoft DNS
445/tcp   open  microsoft-ds? syn-ack
593/tcp   open  ncacn_http    syn-ack Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped    syn-ack
3268/tcp  open  ldap          syn-ack Microsoft Windows Active Directory LDAP (Domain: factory.lan0., Site: Default-First-Site-Name)
49669/tcp open  ncacn_http    syn-ack Microsoft Windows RPC over HTTP 1.0
49687/tcp open  msrpc         syn-ack Microsoft Windows RPC
50206/tcp open  msrpc         syn-ack Microsoft Windows RPC

Host script results:
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 40427/tcp): CLEAN (Timeout)
|   Check 2 (port 46791/tcp): CLEAN (Timeout)
|   Check 3 (port 49455/udp): CLEAN (Timeout)
|   Check 4 (port 14211/udp): CLEAN (Timeout)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled and required
| smb2-time: 
|   date: 2019-07-11 13:39:58
|_  start_date: 1601-01-01 00:09:21
```

### 172.16.42.101

```bash
sudo masscan -e tun0 -p0-65535,U:0-65535 --rate 1000 172.16.42.101

Discovered open port 135/tcp on 172.16.42.101                                  
Discovered open port 49712/tcp on 172.16.42.101                                
Discovered open port 445/tcp on 172.16.42.101                                  
Discovered open port 5040/tcp on 172.16.42.101                                 
Discovered open port 49669/tcp on 172.16.42.101                  

sudo nmap -sT -sV -O -T4 -vvv --version-intensity=8 -sC -p135,49712,445,5040,49669 172.16.42.101

PORT      STATE    SERVICE REASON      VERSION
135/tcp   open     msrpc   syn-ack     Microsoft Windows RPC
554/tcp   filtered rtsp    no-response
5040/tcp  open     unknown syn-ack
49669/tcp open     msrpc   syn-ack     Microsoft Windows RPC
49712/tcp open     msrpc   syn-ack     Microsoft Windows RPC
```


## smb / rpc

* anon co

```bash
➜  step5 git:(master) ✗ python /opt/t/pentest/recona/smbmap/smbmap.py -u '' -p '' -H 172.16.42.101            
[+] Finding open SMB ports....
[!] Authentication error occured
[!] SMB SessionError: STATUS_ACCESS_DENIED({Access Denied} A process has requested access to an object but has not been granted those access rights.)
[!] Authentication error on 172.16.42.101
➜  step5 git:(master) ✗ python /opt/t/pentest/recona/smbmap/smbmap.py -u '' -p '' -H 172.16.42.11 
[+] Finding open SMB ports....
➜  step5 git:(master) ✗ python /opt/t/pentest/recona/smbmap/smbmap.py -u '' -p '' -H 172.16.42.5 
[+] Finding open SMB ports....
[+] User SMB session establishd on 172.16.42.5...
[+] IP: 172.16.42.5:445	Name: 172.16.42.5                                       
	Disk                                                  	Permissions
	----                                                  	-----------
[!] Access Denied
➜  step5 git:(master) ✗ rpcclient -U '' -N 172.16.42.5              
rpcclient $> enumdomusers
result was NT_STATUS_ACCESS_DENIED
rpcclient $> exit
➜  step5 git:(master) ✗ rpcclient -U '' -N 172.16.42.101
Cannot connect to server.  Error was NT_STATUS_ACCESS_DENIED
➜  step5 git:(master) ✗ rpcclient -U '' -N 172.16.42.11 
Cannot connect to server.  Error was NT_STATUS_IO_TIMEOUT
```

