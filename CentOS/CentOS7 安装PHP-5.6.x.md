# CentOS7 安装PHP-5.6.x

### 安装YUM-EPEL源及remi源

```
wget http://dl.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7-5.noarch.rpm
wget http://rpms.famillecollet.com/enterprise/remi-release-7.rpm
rpm -Uvh remi-release-7*.rpm epel-release-7*.rpm
```

**安装EPEL源时需要注意其版本号，这关系到下载链接的正确性，具体安装方法可查阅相关资料**

### 如果已经安装EPEL则只需安装remi源

```
wget http://rpms.famillecollet.com/enterprise/remi-release-7.rpm
rpm -Uvh remi-release-7*.rpm
```

### 编辑remi.repo中的php56 enable为1

`$ sudo vi /etc/yum.repos.d/remi.repo`

    [remi-php56]
    name=Les RPM de remi de PHP 5.6 pour Enterprise Linux 6 - $basearch
    #baseurl=http://rpms.famillecollet.com/enterprise/6/php56/$basearch/
    mirrorlist=http://rpms.famillecollet.com/enterprise/6/php56/mirror
    # WARNING: If you enable this repository, you must also enable "remi"
    enabled=1
    gpgcheck=1
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-remi

### yum安装php

`$ sudo yum install php`

### 查看php版本

`$ php -v`

    PHP 5.6.31 (cli) (built: Jul  6 2017 08:06:11)
    Copyright (c) 1997-2016 The PHP Group
    Zend Engine v2.6.0, Copyright (c) 1998-2016 Zend Technologies
