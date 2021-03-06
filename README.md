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

```
/** Database hostname */
define( 'DB_HOST', '172.31.0.28' ); # ***172.31.0.28*** Is the IP Address of DB Server

```

## Log into Development-Server and take wordpress backup

~]# mysqldump -u root -p123 wordpress > wordpress.sql

## Staging Database Server

scp -r root@172.31.38.154:/root/wordpress.sql .

yum install mariadb-server -y ; systemctl restart mariadb.service ; systemctl enable mariadb.service
    
mysql_secure_installation

mysql -u root -p123

>create database wordpress;
>create user 'wordpress'@'%' identified by 'wordpress';
>grant all privileges on wordpress.* to 'wordpress'@'%';
>flush privileges;


mysql -u root -p123 wordpress < wordpress.sql

## Setup the Home url and Site url(There are two options)

#### Option 1 (By updating wp-config.php)

vim /var/www/html/wp-config.php

define('WP_HOME','https://staging.suryakiran.online' );

define('WP_SITEURL','https://staging.suryakiran.online' );

#### Option 2 (By updating wordpress database)

UPDATE wp_options SET option_value = replace(option_value, 'dev.suryakiran.online', 'staging.suryakiran.online') WHERE option_name = 'home' OR option_name = 'siteurl';UPDATE wp_posts SET guid = replace(guid, 'dev.suryakiran.online','staging.suryakiran.online');UPDATE wp_posts SET post_content = replace(post_content, 'dev.suryakiran.online', 'staging.suryakiran.online'); UPDATE wp_postmeta SET meta_value = replace(meta_value,'dev.suryakiran.online','staging.suryakiran.online');

## Setting Application Load Balancer

![alt text](https://github.com/SuryakiranSubramaniam/Staging-Server-From-Development-Server/blob/main/image/alb1.png)

![alt text](https://github.com/SuryakiranSubramaniam/Staging-Server-From-Development-Server/blob/main/image/alb2.png)

![alt text](https://github.com/SuryakiranSubramaniam/Staging-Server-From-Development-Server/blob/main/image/alb3.png)

![alt text](https://github.com/SuryakiranSubramaniam/Staging-Server-From-Development-Server/blob/main/image/alb4.png)

## Route 53

![alt text](https://github.com/SuryakiranSubramaniam/Staging-Server-From-Development-Server/blob/main/image/R53.png)

![alt text](https://github.com/SuryakiranSubramaniam/Staging-Server-From-Development-Server/blob/main/image/R53-2.png)


## Attach IAM Role

Atthach the Apache PHP Server with an IAM Role with ***AmazonS3FullAccess*** permission

![alt text](https://github.com/SuryakiranSubramaniam/Staging-Server-From-Development-Server/blob/main/image/IamRole.png)

## Note (Warning : Dont use if this is not nesassary:)

#### Most probably uploading media files to S3 maybe not working

This is due to bug in the ***Really Simple SSL*** for solving this

###### In Apache PHP Server

~]#rm -rf /var/www/html/wp-content/plugins/really-simple-ssl

Now login to http://staging3.suryakiran.online/wp-login.php and install ***Really Simple SSL*** once again

###### For removing the site and database easily

 ~]# rm -rf /var/www/html/*

 ~]# mysql -u root -p123

MariaDB [(none)]> drop database wordpress;

