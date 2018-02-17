# CentOS7 部署LAMP环境

### 安装LAMP

`$ sudo yum install httpd mariadb-server php php-mysqlnd -y`

### 启动Apache

`$ sudo apachectl start`

### 查看Apache运行状态

`$ apachectl status`

    httpd.service - The Apache HTTP Server
    Loaded: loaded (/usr/lib/systemd/system/httpd.service; disabled; vendor preset: disabled)
    Active: active (running) since Sun 2016-07-03 10:19:17 CST; 4s ago
    Docs: man:httpd(8)
    man:apachectl(8)
    Main PID: 26132 (httpd)
    Status: "Processing requests..."
    CGroup: /system.slice/httpd.service
    |-26132 /usr/sbin/httpd -DFOREGROUND
    |-26210 /usr/sbin/httpd -DFOREGROUND
    |-26211 /usr/sbin/httpd -DFOREGROUND
    |-26212 /usr/sbin/httpd -DFOREGROUND
    |-26216 /usr/sbin/httpd -DFOREGROUND
    \`-26218 /usr/sbin/httpd -DFOREGROUND

    Jul 03 10:19:16 romen.home.centos systemd[1]: Starting The Apache HTTP Server...
    Jul 03 10:19:17 romen.home.centos systemd[1]: Started The Apache HTTP Server.

### 检验LAMP环境

编写info.php文件

`$ sudo vi /var/www/html/info.php`

添加如下内容

`<?php phpinfo(); ?>`

在浏览器访问http://localhost/info.php

如能正确看到PHP相关信息则配置成功

### 防火墙设置

开启防火墙http服务端口

`$ sudo firewall-cmd --zone=public --add-service=http --permanent`

重新加载防火墙设置

`$ sudo firewall-cmd --reload`

### SELinux设置

关闭SELinux

`$ sudo setenforce 0`

查看SELinux设置

`$ getenforce`

    Permissive

### 关闭Apache服务器

`$ sudo apachectl stop`
