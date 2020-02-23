# 安装LAMP(个人使用环境是Ubuntu-16.04-LTS server)
&ensp;&ensp;LAMP也是较为经典的一个部署环境，尤其是对于部署Web服务器，当然也有LNMP。当我们需要LAMP环境部署的时候，会有类似RPM这样打包，会减轻我们很多的部署成本file:///home/ez/Desktop/Test/Myblog/posts/2017-11-07-Mysql中文乱码.md。但个人更加倾向于手动配置LAMP的环境，话不都说，我们开始环境部署<br>

&ensp;&ensp;1.为了保险起见，推荐首先查看系统更新 <br>

```scss
sudo apt-get update
```

&ensp;&ensp;2.确定是最新的系统，首先开始安装Apahe<br>
```scss
sudo apt-get install apache2
```
&ensp;&ensp;安装完成后，在浏览器中输入服务器的iP，可以看到Apache安装成功的提示<br>

&ensp;&ensp;3.然后开始安装mysql+php<br>
```scss
sudo apt-get install php7.0
sudo apt-get install mysql-server
```
&ensp;&ensp;安装Mysql过程中，会要求输入两次用户名密码<br>

&ensp;&ensp;4.现在可以通过简单的PHP代码测试PHP对应的环境是否安装成功<br>
```scss
#新建一个php文件，比如 vim /test.php
#输入以下内容：
<?php
phpinfo()
?>
#然后执行php /test.php
#检查返回结果，如果有以下关键字，基本就是成功了
phpinfo()
PHP Version => 7.0.22-0ubuntu0.16.04.1
```

&ensp;&ensp;5.再安装phpmyadmin之前，建议安装整个Mysql和php关联包<br>
```scss
apt-get install php7.0-cgi
apt-get install libapache2-mod-php7.0 (这个包十分关键！请务必安装！)
apt-get install phpmyadmin

#然后就可以在浏览器中进入phpmyadmin
http://ip+phpmyadmin like http://192.168.1.104/phpmyadmin/
```
&ensp;&ensp;6.这样就完成了LAMP+phpmyadmin部署<br>
***********************************************************************************
&ensp;&ensp;防止浏览网页目录<br>
```scss
<Directory “/var/www/html”>
     Options Indexes FollowSymLinks
     AllowOverride All
     Order allow,deny
     Allow from all
</Directory>
```
其实就是将Indexes去掉，Indexes表示若当前目录没有index.html就会显示目录结构。<br>
***********************************************************************************
##  PS.如何在阿里云Ubuntu服务器中添加SSL

&ensp;&ensp;曾经在阿里云申请到了一个免费的个人域名SSL证书，在SSL证书安装到Apache服务器的时候，遇到了很多问题，而且阿里云关于Apache服务器的介绍也并不是很完整。下面是自己探索得出来的方法，并不是完全准确，但是基本上能够正常使用，如果有什么不足之处，请多多指教。<br>

&ensp;&ensp;首先确保ssl-cert证书已经安装成功<br>
```scss
apt-get install ssl-cert
```
&ensp;&ensp;然后开启SSL模块和加入监听端口<br>
```scss
a2enmod ssl
a2ensite default-ssl
```
&ensp;&ensp;然后监听443端口<br>file:///home/ez/Desktop/Test/Myblog/posts/2017-11-07-Mysql中文乱码.md
```scss
sudo vim /etc/apache2/ports.conf
```
&ensp;&ensp;加入下面这一行<br>
```scss
Listen 443
```
&ensp;&ensp;然后打开ssl配置文件，这里网上有些文章介绍打开的是default-ssl.conf开头的文件，而经过测试，即使成功配置，有些浏览器会提示SSL报错，所以建议打开以000为开头的配置文件，以上两个文件都在同一个目录下<br>
```scss
sudo vim /etc/apache2/site-enabled/000-default.conf
```
&ensp;&ensp;同样的虚拟主机那里也要改为443<br>
```scss
VirtualHost *:443
```
&ensp;&ensp;加入下面内容<br>
```scss
SSLEngine on
SSLCertificateFile    /etc/ssl/certs/ssl-cert-snakeoil.pem
SSLCertificateKeyFile /etc/ssl/private/ssl-cert-snakeoil.key
SSLCertificateChainFile /etc/ssl/certs/server-ca.crt
```
&ensp;&ensp;至于以上三个文件作用，这里有Apache官方的连接http://httpd.apache.org/docs/2.4/zh-cn/ssl/ssl_howto.html<br>

&ensp;&ensp;基本上到这里，重启服务器，就能通过HTTPS访问网站了。<br>

##  MySQL遇到中文乱码

### Mysql中文
&ensp;&ensp;其实就只需要更改配置文件就可以增加Mysql对中文的输出输入<br>

### LAMP环境

打开Mysql配置文件
```scss
sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
```
在配置文件添加下列内容<br>
```scss
[mysqld]
character-set-server=utf8
 #增加这个设置
[mysql]
default-character-set=utf8
#增加这个设置
```
 记得重启Mysql
 ```scss
 sudo /etc/init.d/mysql restart
 ```

### Xampp环境

打开Xampp控制面板,找到“Config”，然后找到“my.ini”<br>

添加以下内容<br>
```scss
[client]
default_character_set=utf8
[mysqld]
character-set-server=utf8
collation-server=utf8_general_ci
[mysql]
default_character_set=utf8
```

完成之后记得保存，并重启Mysql

