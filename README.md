# Reverse Proxy Docker

Install Apache2 and Docker :
---------------
```
#Install Apache2 and Docker
apt update
apt install apache2
apt install docker.io
a2enmod proxy proxy_http proxy_balancer lbmethod_byrequests

#Verification Apache2
systemctl restart apache2
systemctl status apache2
```

Install Shell In A Box :
---------------
```
#Pull Image
docker pull sspreitzer/shellinabox:latest

#Running Docker
docker run -d -p 31100:4200 -p 41100:41100 -p 51100:51100 -e SIAB_USER=[USERNAME] -e SIAB_PASSWORD=[PASSWORD] -e SIAB_HOME=/home/[DIR_USER] -e SIAB_SSL=false -e SIAB_SUDO=true sspreitzer/shellinabox:latest

#Verification Shell In A Box
docker container ls
```

Configuration Virtual Host :
---------------
```
#Edit Directory
cd /etc/apache2/sites-available
nano [SITE_DOMAIN].conf

#Configuration Virtual Host
 <VirtualHost *:80>
    ServerAdmin admin@[SITE_DOMAIN]
    ServerName [SITE_DOMAIN]
    ServerAlias [SITE_DOMAIN]
    ProxyPreserveHost On
    ProxyPass / http://[PUBLIC_IP]:51100/
    ProxyPassReverse / http://[PUBLIC_IP]:51100/
 </VirtualHost>
 <VirtualHost *:443>
    ServerName [SITE_DOMAIN]
    SSLEngine on
    SSLCertificateFile /etc/apache2/ssl/[SITE_DOMAIN].pem
    SSLCertificateKeyFile /etc/apache2/ssl/[SITE_DOMAIN].key
    ProxyPreserveHost On
    ProxyPass / http://[PUBLIC_IP]:51100/
    ProxyPassReverse / http://[PUBLIC_IP]:51100/
 </VirtualHost>

#Verification Virtual Host
a2ensite [SITE_DOMAIN].conf
service apache2 restart
```

Setting Cloudflare and SSL :
---------------
```
#Settings Cloudflare
Setting DNS Management
Setting SSL/TLS Encryption in Mode Full (Strict)
Create Certificate Origin Server

#Settings in Server :
mkdir /etc/apache2/ssl
nano /etc/apache2/ssl/[SITE_DOMAIN].pem
nano /etc/apache2/ssl/[SITE_DOMAIN].key

#Verification SSL
sudo a2enmod ssl
service apache2 restart
```

Docker Command :
---------------
```
#Check Container List
docker container ls

#Running All Container
docker start $(docker ps -a -q)

#Stop Container
docker container stop (CONTAINER_NAME)

#Delete Container
docker container rm (CONTAINER_NAME)

#Start Container
docker container start (CONTAINER_NAME)

#Remote Container
docker exec -t -i (CONTAINER_NAME) /bin/bash
```


