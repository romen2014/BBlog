# CentOS7 安装配置JDK8

### 安装说明
- 系统环境：centos7
- 安装方式：tar.gz安装
- 软件：jdk-8u91-linux-x64.tar.gz [官网下载](http://www.oracle.com/technetwork/java/javase/downloads/index.html)

### 检验系统原版本

`$ java -version`

    java version “1.8.0_65”
    OpenJDK Runtime Environment (build 1.8.0_65-b17)
    OpenJDK 64-Bit Server VM (build 25.65-b01, mixed mode)

### 进一步查看JDK信息：

`$ rpm -qa | grep openjdk`

    java-1.8.0-openjdk-1.8.0.65-3.b17.el7.x86_64
    java-1.7.0-openjdk-headless-1.7.0.91-2.6.2.3.el7.x86_64
    java-1.8.0-openjdk-headless-1.8.0.65-3.b17.el7.x86_64
    java-1.7.0-openjdk-1.7.0.91-2.6.2.3.el7.x86_64

### 卸载OpenJDK，执行以下操作：

`$ sudo rpm -e --nodeps java-1.7.0-openjdk-headless-1.7.0.91-2.6.2.3.el7.x86_64`

`$ sudo rpm -e --nodeps java-1.7.0-openjdk-1.7.0.91-2.6.2.3.el7.x86_64`

`$ sudo rpm -e --nodeps java-1.8.0-openjdk-headless-1.8.0.65-3.b17.el7.x86_64`

`$ sudo rpm -e --nodeps java-1.8.0-openjdk-1.8.0.65-3.b17.el7.x86_64`

### 安装JDK

`$ sudo mkdir /usr/java`

`$ sudo tar -zxvf jdk-8u91-linux-x64.tar.gz -C /usr/java`

### 配置环境变量

`$ sudo vi /etc/profile`

追加以下内容

    JAVA_HOME=/usr/java/jdk1.8.0_91
    JRE_HOME=/usr/java/jdk1.8.0_91/jre
    PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin
    CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib
    export JAVA_HOME JRE_HOME PATH CLASSPATH

### 使修改生效
`$ source /etc/profile`

### 验证安装

`$ java -version`

    java version “1.8.0_91”
    Java(TM) SE Runtime Environment (build 1.8.0_91-b14)
    Java HotSpot(TM) 64-Bit Server VM (build 25.91-b14, mixed mode)

安装完毕！
