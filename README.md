# Staging-Server-From-Development-Server
Staging-Server-From-Development-Server

![alt text](https://github.com/SuryakiranSubramaniam/test/blob/main/image/Screenshot%20from%202022-03-12%2016-44-39.png)

## Apache PHP Server

amazon-linux-extras install php7.4 -y

yum install php-xmlwriter -y

yum install git -y

yum install php-gd -y

yum install httpd -y ; systemctl restart httpd.service ; systemctl enable httpd.service

vim new

chmod 400 new

eval $('ssh-agent')

ssh-add -k /root/new

mkdir /var/site

yum install git -y

git clone git@github.com:SuryakiranSubramaniam/dev.suryakiran.online.git /var/site

ls /var/site/

cp -r /var/site/* /var/www/html/

chown -R apache:apache /var/www/html/*

vim /var/www/html/wp-config.php

## Database Server


