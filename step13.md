# Step 13

## trouver la derniere machine

faut se rappeler que sur le schema réseau il y a une machine en todo, pour la trouver on regarde dans le cache arp de la machone qu'on vien tde root :
 
```bash
root@SRV02-BACKUP:~# cat /proc/net/arp
IP address       HW type     Flags       HW address            Mask     Device
172.16.69.23     0x1         0x2         ce:d3:94:6c:38:3f     *        ens18
172.16.69.78     0x1         0x2         7a:0a:61:1a:36:65     *        ens18
172.16.69.254    0x1         0x2         3e:20:13:a5:09:49     *        ens18
```

## setup proxychains

A FAIRE POUR PAS FAIRE COMME UN DEGUEU

nmap à travers le proxychains de cette nouvelle machine:

```bash
PORT     STATE SERVICE REASON         VERSION
22/tcp   open  ssh     syn-ack ttl 64 OpenSSH 7.4p1 Debian 10+deb9u6 (protocol 2.0)
| ssh-hostkey: 
|   2048 a5:59:a8:46:b6:f1:8a:0d:5a:09:e0:13:cf:c3:1a:0e (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDcQiERvcKTr1SWBWbGch4ehJtBwSCeKExtK7lYJ2wFgi2lVibaAB28Yd/ONXcx3tRRqzilJkPoHjfAv4tbVQVxlHF8ZTr/OPxnkrf4nq78zpgHKYIc0yaeY6ith3Wy6Ot+l3Yw06OH6Q1wU06SVlmn2VBeppWCfVXHZlw50AMr1j2tkqlOQBUaBgP1BVos2e5aYP5BDYNwJ9OYHjHMFjUUN923aTgBvEVmvJot5F6zlR1WVDJAf8D0BftmHH5bAsZxb2YoYMbIoDBIQY8YtKe3CIJChWgNiUWvwvr0w3WWC0JdAcKioTYlCgipai2n5lhWyALdoLH9Bbtjf9KkE7rJ
|   256 35:be:9a:51:fc:d0:24:73:68:49:21:ba:b5:b1:4e:b2 (ECDSA)
|_ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBF2Xtktd+G+fVqMGQFxwSYvECDEdUWl4HC5NQfSOe66RvfEMI3hqvgj/UaDA/uAJ14RJy2Se0wo13g7GzionwAg=
111/tcp  open  rpcbind syn-ack ttl 64 2-4 (RPC #100000)
| rpcinfo: 
|   program version   port/proto  service
|   100000  2,3,4        111/tcp  rpcbind
|   100000  2,3,4        111/udp  rpcbind
|   100003  3,4         2049/tcp  nfs
|   100003  3,4         2049/udp  nfs
|   100005  1,2,3      41925/udp  mountd
|   100005  1,2,3      55855/tcp  mountd
|   100021  1,3,4      36477/tcp  nlockmgr
|   100021  1,3,4      46593/udp  nlockmgr
|   100227  3           2049/tcp  nfs_acl
|_  100227  3           2049/udp  nfs_acl
2049/tcp open  nfs_acl syn-ack ttl 64 3 (RPC #100227)
```

## monter le nfs

```bash
mkdir /tmp/a 

mount -t nfs 172.16.69.23:/ /tmp/a
```

## list les fichiers du nfs

```bash
root@SRV02-BACKUP:/tmp/a/DATA# ls -laR .
.:
total 44
drwxr-xr-x 11 root     root     4096 juin  24 17:43 .
drwxr-xr-x 23 root     root     4096 juin  24 17:41 ..
drwxr-x--x  2 georgina georgina 4096 juil.  4 21:28 backup
drwxr-x--x  2 nobody   nogroup  4096 juin  24 17:43 databases
drwxr-x--x  2 nobody   nogroup  4096 juin  24 17:43 documents
drwxr-x--x  2 nobody   nogroup  4096 juin  24 17:43 downloads
drwxr-x--x  7 nobody   nogroup  4096 juin  24 17:45 home
drwxr-x--x  2 nobody   nogroup  4096 juin  24 17:43 logs
drwxr-x--x  2 nobody   nogroup  4096 juil.  4 10:51 pictures
drwxr-x--x  2 nobody   nogroup  4096 juin  24 17:43 videos
drwxr-x--x  2 nobody   nogroup  4096 juil.  5 16:59 VIP

./backup:
total 12
drwxr-x--x  2 georgina georgina 4096 juil.  4 21:28 .
drwxr-xr-x 11 root     root     4096 juin  24 17:43 ..
-rw-r--r--  1 root     root     2281 juil.  4 21:28 out.zip

./databases:
total 8
drwxr-x--x  2 nobody nogroup 4096 juin  24 17:43 .
drwxr-xr-x 11 root   root    4096 juin  24 17:43 ..

./documents:
total 8
drwxr-x--x  2 nobody nogroup 4096 juin  24 17:43 .
drwxr-xr-x 11 root   root    4096 juin  24 17:43 ..

./downloads:
total 8
drwxr-x--x  2 nobody nogroup 4096 juin  24 17:43 .
drwxr-xr-x 11 root   root    4096 juin  24 17:43 ..

./home:
total 28
drwxr-x--x  7 nobody nogroup 4096 juin  24 17:45 .
drwxr-xr-x 11 root   root    4096 juin  24 17:43 ..
drwxr-x--x  2 nobody nogroup 4096 juin  24 17:45 georgina
drwxr-x--x  2 nobody nogroup 4096 juin  24 17:45 mrbucket
drwxr-x--x  2 nobody nogroup 4096 juin  24 17:45 oompaloompa
drwxr-x--x  2 nobody nogroup 4096 juin  24 17:45 veruca
drwxr-x--x  2 nobody nogroup 4096 juin  24 17:45 violet

./home/georgina:
total 8
drwxr-x--x 2 nobody nogroup 4096 juin  24 17:45 .
drwxr-x--x 7 nobody nogroup 4096 juin  24 17:45 ..

./home/mrbucket:
total 8
drwxr-x--x 2 nobody nogroup 4096 juin  24 17:45 .
drwxr-x--x 7 nobody nogroup 4096 juin  24 17:45 ..

./home/oompaloompa:
total 8
drwxr-x--x 2 nobody nogroup 4096 juin  24 17:45 .
drwxr-x--x 7 nobody nogroup 4096 juin  24 17:45 ..

./home/veruca:
total 8
drwxr-x--x 2 nobody nogroup 4096 juin  24 17:45 .
drwxr-x--x 7 nobody nogroup 4096 juin  24 17:45 ..

./home/violet:
total 8
drwxr-x--x 2 nobody nogroup 4096 juin  24 17:45 .
drwxr-x--x 7 nobody nogroup 4096 juin  24 17:45 ..

./logs:
total 8
drwxr-x--x  2 nobody nogroup 4096 juin  24 17:43 .
drwxr-xr-x 11 root   root    4096 juin  24 17:43 ..

./pictures:
total 11652
drwxr-x--x  2 nobody nogroup    4096 juil.  4 10:51 .
drwxr-xr-x 11 root   root       4096 juin  24 17:43 ..
-rw-r--r--  1 nobody nogroup  131823 juil.  4 10:50 ascenseurRedTeam.png
-rw-r--r--  1 nobody nogroup 1901579 juil.  4 10:50 cinemaRedTeam.png
-rw-r--r--  1 nobody nogroup  196371 juil.  4 10:50 croissantageSaif.png
-rw-r--r--  1 nobody nogroup  130839 juil.  4 10:50 demenagement.png
-rw-r--r--  1 nobody nogroup  153995 juil.  4 10:50 glace.png
-rw-r--r--  1 nobody nogroup   37247 juil.  4 10:50 kenkenken.png
-rw-r--r--  1 nobody nogroup  138224 juil.  4 10:50 keskiamaintenant.png
-rw-r--r--  1 nobody nogroup  190794 juil.  4 10:50 miam.png
-rw-r--r--  1 nobody nogroup  116749 juil.  4 10:50 newyork.png
-rw-r--r--  1 nobody nogroup   86819 juil.  4 10:50 notredame.png
-rw-r--r--  1 nobody nogroup  965251 juil.  4 10:50 onPartEnRestitution.jpg
-rw-r--r--  1 nobody nogroup 2196730 juil.  4 10:50 onVeutDuPain.png
-rw-r--r--  1 nobody nogroup 1039727 juil.  4 10:50 plage.png
-rw-r--r--  1 nobody nogroup  104879 juil.  4 10:50 redabogoss.png
-rw-r--r--  1 nobody nogroup  137275 juil.  4 10:50 redaSport.png
-rw-r--r--  1 nobody nogroup 2043318 juil.  4 10:50 rootagedADsurDouchette.png
-rw-r--r--  1 nobody nogroup  182331 juil.  4 10:50 rootagedemeres.png
-rw-r--r--  1 nobody nogroup 1968672 juil.  4 10:50 soireeAvantPentest.png
-rw-r--r--  1 nobody nogroup   69753 juil.  4 10:50 soireepicol.png
-rw-r--r--  1 nobody nogroup   93357 juil.  4 10:50 toiletteAmehdi.png

./videos:
total 8
drwxr-x--x  2 nobody nogroup 4096 juin  24 17:43 .
drwxr-xr-x 11 root   root    4096 juin  24 17:43 ..

./VIP:
total 408
drwxr-x--x  2 nobody nogroup   4096 juil.  5 16:59 .
drwxr-xr-x 11 root   root      4096 juin  24 17:43 ..
-rw-r--r--  1 nobody nogroup  88168 juil.  5 16:58 flag_lol.jpg
-rw-r--r--  1 nobody nogroup  30234 juil.  3 19:15 INVOICE.docx
-rw-r--r--  1 nobody nogroup 127983 juil.  3 19:15 INVOICE.pdf
-rw-r--r--  1 nobody nogroup 152189 juil.  5 16:58 whiteboard.jpg
```

on cherche le patron de willy wonka, y a du invoice, probablement une histoire de metadata.

EXTRACT DES FICHIERS À FAIRE ZIP + EXTRACT À LA VOLÉE

## metadata des fichiers

```bash
root@SRV02-BACKUP:/tmp/a/DATA/VIP# exiftool *
perl: warning: Setting locale failed.
perl: warning: Please check that your locale settings:
	LANGUAGE = (unset),
	LC_ALL = (unset),
	LC_MEASUREMENT = "fr_FR.UTF-8",
	LC_PAPER = "fr_FR.UTF-8",
	LC_MONETARY = "fr_FR.UTF-8",
	LC_NAME = "fr_FR.UTF-8",
	LC_CTYPE = "en_US.UTF-8",
	LC_ADDRESS = "fr_FR.UTF-8",
	LC_NUMERIC = "fr_FR.UTF-8",
	LC_TELEPHONE = "fr_FR.UTF-8",
	LC_IDENTIFICATION = "fr_FR.UTF-8",
	LC_TIME = "fr_FR.UTF-8",
	LANG = "fr_FR.UTF-8"
    are supported and installed on your system.
perl: warning: Falling back to a fallback locale ("fr_FR.UTF-8").
======== flag_lol.jpg
ExifTool Version Number         : 10.40
File Name                       : flag_lol.jpg
Directory                       : .
File Size                       : 86 kB
File Modification Date/Time     : 2019:07:05 16:58:34+02:00
File Access Date/Time           : 2019:07:19 18:56:47+02:00
File Inode Change Date/Time     : 2019:07:05 17:01:25+02:00
File Permissions                : rw-r--r--
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
JFIF Version                    : 1.01
Resolution Unit                 : None
X Resolution                    : 1
Y Resolution                    : 1
Image Width                     : 1024
Image Height                    : 768
Encoding Process                : Progressive DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:2:0 (2 2)
Image Size                      : 1024x768
Megapixels                      : 0.786
======== INVOICE.docx
ExifTool Version Number         : 10.40
File Name                       : INVOICE.docx
Directory                       : .
File Size                       : 30 kB
File Modification Date/Time     : 2019:07:03 19:15:17+02:00
File Access Date/Time           : 2019:07:04 15:06:00+02:00
File Inode Change Date/Time     : 2019:07:05 17:01:25+02:00
File Permissions                : rw-r--r--
File Type                       : DOCX
File Type Extension             : docx
MIME Type                       : application/vnd.openxmlformats-officedocument.wordprocessingml.document
Zip Required Version            : 20
Zip Bit Flag                    : 0x0006
Zip Compression                 : Deflated
Zip Modify Date                 : 1980:01:01 00:00:00
Zip CRC                         : 0x1dbbefa3
Zip Compressed Size             : 357
Zip Uncompressed Size           : 1362
Zip File Name                   : [Content_Types].xml
Title                           : INVOICE wilbur Wonka
Subject                         : 
Creator                         : Grandma Josephine
Keywords                        : G.J.
Description                     : 
Last Modified By                : Utilisateur Windows
Revision Number                 : 6
Last Printed                    : 2019:07:03 15:27:00Z
Create Date                     : 2019:07:03 14:16:00Z
Modify Date                     : 2019:07:03 16:53:00Z
Template                        : Normal.dotm
Total Edit Time                 : 1.9 hours
Pages                           : 1
Words                           : 70
Characters                      : 405
Application                     : Microsoft Office Word
Doc Security                    : None
Lines                           : 3
Paragraphs                      : 1
Scale Crop                      : No
Heading Pairs                   : Titre, 1
Titles Of Parts                 : INVOICE wilbur Wonka
Company                         : 
Links Up To Date                : No
Characters With Spaces          : 474
Shared Doc                      : No
Hyperlinks Changed              : No
App Version                     : 16.0000
======== INVOICE.pdf
ExifTool Version Number         : 10.40
File Name                       : INVOICE.pdf
Directory                       : .
File Size                       : 125 kB
File Modification Date/Time     : 2019:07:03 19:15:17+02:00
File Access Date/Time           : 2019:07:04 21:42:41+02:00
File Inode Change Date/Time     : 2019:07:05 17:01:25+02:00
File Permissions                : rw-r--r--
File Type                       : PDF
File Type Extension             : pdf
MIME Type                       : application/pdf
PDF Version                     : 1.7
Linearized                      : No
Page Count                      : 1
Language                        : en-US
Tagged PDF                      : Yes
XMP Toolkit                     : 3.1-701
Producer                        : Microsoft® Word pour Office 365
Keywords                        : G.J.
Title                           : INVOICE wilbur Wonka
Creator                         : Grandma Josephine
Creator Tool                    : Microsoft® Word pour Office 365
Create Date                     : 2019:07:03 18:54:43+02:00
Modify Date                     : 2019:07:03 18:54:43+02:00
Document ID                     : uuid:C57ADFEC-F481-4843-AC93-342A8CB17B01
Instance ID                     : uuid:C57ADFEC-F481-4843-AC93-342A8CB17B01
Author                          : Grandma Josephine
======== whiteboard.jpg
ExifTool Version Number         : 10.40
File Name                       : whiteboard.jpg
Directory                       : .
File Size                       : 149 kB
File Modification Date/Time     : 2019:07:05 16:58:34+02:00
File Access Date/Time           : 2019:07:05 16:58:34+02:00
File Inode Change Date/Time     : 2019:07:05 17:01:25+02:00
File Permissions                : rw-r--r--
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
JFIF Version                    : 1.01
Resolution Unit                 : None
X Resolution                    : 1
Y Resolution                    : 1
Image Width                     : 1600
Image Height                    : 900
Encoding Process                : Progressive DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:2:0 (2 2)
Image Size                      : 1600x900
Megapixels                      : 1.4
    4 image files read
```

## flag

```bash
echo -n 'Grandma Josephine' | sha256sum      
b8a3ef108d0c3fac75f3f99f4d6465db8b85b29f41edcfb419a986ca861239f9  -
```

---

funny mais ça sert à rien, l'histoire de récup les ilage d'un truc dans le groupe video

```bash
ls -la /dev | grep video 
crw-rw----  1 root video    29,   0 juil.  4 14:55 fb0
```

* recup le bordel

```bash
dd if=/dev/fb0 of=/tmp/fb0_dmp
```

* recup la size de l'écran

```bash
cat /sys/class/graphics/fb0/virtual_size
1024,768
```

* utiliser ce script 

```perl
#!/usr/bin/perl -w
 
$w = shift || 240;
$h = shift || 320;
$pixels = $w * $h;
 
open OUT, "|pnmtopng" or die "Can't pipe pnmtopng: $!\n";
 
printf OUT "P6%d %d\n255\n", $w, $h;
 
while ((read STDIN, $raw, 2) and $pixels--) {
   $short = unpack('S', $raw);
   print OUT pack("C3",
      ($short & 0xf800) >> 8,
      ($short & 0x7e0) >> 3,
      ($short & 0x1f) << 3);
}
 
close OUT;
```

```bash
./a.pl 1024 768 < fb0_dmp > screen.png
```

c'est marrant mais ça sert à rien
