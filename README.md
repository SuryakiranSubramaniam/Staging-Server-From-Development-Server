# Staging-Server-From-Development-Server
Staging-Server-From-Development-Server

![alt text](https://github.com/SuryakiranSubramaniam/Staging-Server-From-Development-Server/blob/main/image/Staging.png)

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

## Log into Development-Server and take wordpress backup

~]# mysqldump -u root -p123 wordpress > wordpress.sql

## Staging Database Server

scp -r root@172.31.38.154:/root/wordpress.sql .

yum install mariadb-server -y ; systemctl restart mariadb.service ; systemctl enable mariadb.service
    
mysql_secure_installation

mysql -u root -p123

>create database wordpress;

mysql -u root -p123 wordpress < wordpress.sql

## Setup the Home url and Site url(There are two options)

#### Option 1 (By updating wp-config.php)

vim /var/www/html/wp-config.php

define('WP_HOME','https://staging.suryakiran.online' );
define('WP_SITEURL','https://staging.suryakiran.online' );

#### Option 2 (By updating wordpress database)

UPDATE wp_options SET option_value = replace(option_value, 'dev.suryakiran.online', 'staging.suryakiran.online') WHERE option_name = 'home' OR option_name = 'siteurl';UPDATE wp_posts SET guid = replace(guid, 'dev.suryakiran.online','staging.suryakiran.online');UPDATE wp_posts SET post_content = replace(post_content, 'dev.suryakiran.online', 'staging.suryakiran.online'); UPDATE wp_postmeta SET meta_value = replace(meta_value,'dev.suryakiran.online','staging.suryakiran.online');

