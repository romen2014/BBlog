# CentOS7 部署Tomcat服务器

### 安装说明
- 环境：CentOS 7.0
- 软件：apache-tomcat-8.0.36.tar.gz [官网下载](http://tomcat.apache.org/)

### 前置
- Tomcat 8.0 至少需要 Java SE 7 或更高版本。
- 参考CentOS7 安装与配置JDK8

### 安装Tomcat

`$ sudo tar -zxvf apache-tomcat-8.0.36.tar.gz -C /opt/`

### 启动Tomcat

`$ su root`

`# /opt/apache-tomcat-8.0.36/bin/startup.sh`

### 本地访问Tomcat

浏览器中直接访问 http://localhost:8080

### 外网访问Tomcat

开启防火墙8080端口

`$ sudo firewall-cmd --zone=public --add-port=8080/tcp --permanent`

重新加载防火墙设置

`$ sudo firewall-cmd --reload`

查看端口开启状态

`$ sudo firewall-cmd --zone=public --list-all`

    public (default, active)
    interfaces: enp4s0
    sources:
    services: dhcpv6-client ssh
    ports: 8080/tcp
    masquerade: no
    forward-ports:
    icmp-blocks:
    rich rules:

浏览器中直接访问 http://:8080
服务器可以通过ifconfig命令查看

### 关闭Tomcat

`$ su root`

`# /opt/apache-tomcat-8.0.36/bin/shutdown.sh`
