---
title: 搭建flarum论坛
categories: 教程
tags: 
- 论坛
---


本篇文章将介绍如何安装Flarum论坛。我的环境：Ubuntu 16.04.2  x64
php7

依次安装apache2、php、mysql-server、php-mysql

```
sudo apt-get install apache2
sudo apt-get install php(ubunut14用apt-get install php5)
sudo apt-get install mysql-server
sudo apt-get install php-mysql(ubunut14用php5-mysql)
```

下载Flarum中文版
```
wget http://oyy9gorf8.bkt.clouddn.com/Flarum.zip
```

安装unzip
```
sudo apt-get install unzip
```

将Flarum解压
```
sudo unzip Flarum.zip
```

将Flarum文件夹移动到/var/www下面
```
sudo mv Flarum /var/www
```

添加权限
```
sudo chmod -R 777 /var/www/Flarum
```

开启rewrite
```
cd /etc/apache2/mods-enabled
sudo ln -s ../mods-available/rewrite.load
```

重定向
```
sudo vi /etc/apache2/sites-available/000-default.conf
```
找到DocumentRoot /var/www/html 

其中/var/www/html 就是你的网站根目录

把/var/www/html 改为  /var/www/Flarum

并在该文件中的VirtualHost节点之间添加如下代码:
```
<Directory /var/www/Flarum>
AllowOverride All
</Directory>
```

编辑apache2
```
vi /etc/apache2/apache2.conf
```
找到
```
 <Directory />
 Options FollowSymLinks
 AllowOverride None
 Require all denied
 </Directory>
```

将将AllowOverride None改为AllowOverride All ，Require all denied改为Require all granted.

###创建数据库
```
mysql -u root -p
```
输入密码，然后创建一个名为Flarum的数据库
```
create database Flarum;
```
输入exit退出数据库

重启apache2
```
sudo service /etc/init.d/apache2 restart
ubuntu14执行：service apache2 restart
```

如果在重启apache2出现以下错误
```
 sudo service apache2 restart
 * Restarting web server apache2     [fail]
 * The apache2 configtest failed.
Output of config test was:
apache2: Syntax error on line 140 of /etc/apache2/apache2.conf: Could not open configuration file /etc/apache2/mods-enabled/rewrite.load: No such file or directory
Action 'configtest' failed.
The Apache error log may have more information.
```

执行加载Rewrite模块：
```
a2enmod rewrite
```

在浏览器输入IP地址，可以看到，遇到了三个问题![此处输入图片的描述][1]

解决办法：

第一个问题
```
sudo apt-get install  php-xml
```
第二个问题
```
sudo apt-get install php-gd
```
第三个问题
```
sudo apt-get install php-mbstring
```
有的是只有第二个问题The PHP extension 'gd' is required.则输入
```
sudo apt-get install php-gd
```

ubuntu14则执行
```
sudo apt-get install php5-gd
```

重启apache
```
sudo service /etc/init.d/apache2 restart
```

打开浏览器输入ip地址就能正常安装啦

如果要绑定域名，要修改Flarum目录下的config.php
```
sudo vi /var/www/Flarum/config.php
```
找到 'url' => 'http://？？？？？'  改成
```
'url' => '//' . $_SERVER['HTTP_HOST'],
```
执行命令
```php flarum cache:clear && rm -f assets/rev-manifest.json```

[主机优惠链接](https://m.do.co/c/d9d85e31324b)


  [1]: http://oyy9gorf8.bkt.clouddn.com/Flarum%E5%AE%89%E8%A3%85%E9%97%AE%E9%A2%98.png