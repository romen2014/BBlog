# CentOS7 魔力宝贝服务器架设

### 准备篇
- 服務器:64位CentOS7

更新服務器環境(可省略)

`$ sudo yum update -y`

安裝LAMP+Ruby環境

`$ sudo yum install httpd mariadb-server php php-mysql ruby -y`

設置開機啟動apache服務器及mariadb數據庫

`$ sudo systemctl enable httpd`

`$ sudo systemctl enable mariadb`

重啟服務器(或直接啟動httpd及mariadb數據庫也可以，在此不做詳細說明)

`$ reboot`

**PS：其中httpd(Apache)環境為必須。**

**PS：mariadb為必要數據庫環境，也可使用mysql數據庫替代，其他數據庫未測試。**

**PS：PHP環境主要用於註冊頁面使用，如果可以直接操作數據庫則非必須。**

**PS：Ruby環境為服務端內核中的日誌依賴，但非必須。**

以上服务器基本准备已完成。

新手可以先在虚拟机中进行模拟试验，但不一定可以公网访问，具体要看宽带接入需求。

如有公网开放需求，则需要相应的公网IP或者采用内网映射操作，此操作独立于服务器架设以外且有大量相关教程，故不赘述。

- 素材

Server：包括gmsv服务器文件、html网页文件、Dump数据库文件、lib驱动文件。

Client：包括柳叶山庄登录器。

### 服务器篇

##### 1、上傳服务端相关文件

通过Xftp等ftp客户端上传软件将所需文件上传。

1.1

上傳gmsv文件夾至CentOS7服務器任意位置。如：/home/romen/Documents/gmsv。

此为魔力宝贝服务端文件，必须上传。

1.2

上傳html文件夾至CentOS7服務器/var/www/html。

盡量與httpd默認啟動位置一致，并設置html文件夾權限可供當前用戶所有（root用戶可無視）。

1.3

上传lib文件夹至CentOS7服务器任意位置，如：/home/romen/Documents/lib。

此为相关驱动文件，目前CentOS7服务器相关的驱动较新，以不再采用当年的驱动，但魔力宝贝服务端加载时要检验老款驱动，故上传以备用。

##### 2、啟動gmsv

進入/home/romen/gmsv

`$ cd /home/romen/Documents/gmsv`

授權gmsv啟動項

`$ chmod +x ./gmsv`

啟動gmsv

`$ ./gmsv`

**PS：一定要到gmsv跟目錄下再用./gmsv啟動服務器，否則會報基於setup.cf的錯**

##### 3、相關問題

启动gmsv后，控制台会提示以下内容报错，请根据不同情况做相应的处理。
以下内容可能并不会全部暴露出来，具体根据服务端gmsv的不同而不同。

3.1

    [romen@romen Gmsv]$ ./gmsv
    -bash: ./gmsv: /lib/ld-linux.so.2: bad ELF interpreter: No such file or directory

原因：缺少32位相關庫文件。

解決方案：安裝32位相關庫文件。

`$ sudo yum install glibc.i686`

3.2

    [romen@romen Gmsv]$ ./gmsv
    ./gmsv: error while loading shared libraries: libz.so.1: cannot open shared object file: No such file or directory

原因：缺少32位相關庫文件。

解決方案：安裝32位相關庫文件。

`$ sudo yum install zlib.i686`

3.3

    [romen@romen gmsv]$ ./gmsv
    ./gmsv: error while loading shared libraries: libmysqlclient_r.so.10: cannot open shared object file: No such file or directory

原因：缺少相關庫驱动文件。

解決方案：将之前上傳的lib文件夾下的所有文件，悉数cp或mv至/usr/lib/目錄下。

`$ sudo cp /home/romen/Documents/lib/* /usr/lib/`

3.4

    initilize database...connecting database server...127.0.0.1
    root
    abcd
    db.c:1364  database connect error

原因：數據密碼不正確或未建立相應的database。

解決方案：

將數據庫用戶名密碼改成相關提示信息(如上提示用户名root/密码abcd)。

建立database，命名為rogue。以下命令為sql命令：

CREATE SCHEMA rogue DEFAULT CHARACTER SET utf8 ;

**PS：新手对于sql命令不熟悉的可以使用MySQL-workbench之类的数据库客户端软件进行相应的操作，目的是要创建rogue库。**

**PS：部分gmsv內核在設置數據庫密碼后可能會導致內核gmsv啟動失敗，建議將數據庫密碼滯空。會留下安全隱患，請自行補習數據庫賬戶安全知識。**

3.5

出現與setup.cf相關的錯誤。

原因：setup.cf沒有配置正確。

解決方案：修改setup.cf內的相關內容，如服務器地址等。例如：

    extraipaddress=192.168.1.108
    port=9030

具体的setup.cf相应的配置内容请自行查阅。

**PS：setup.cf文件中的blserv、mlserv、acserv及相應的端口並非服務器地址及端口，不配置也不影響服務端啟動及工作。**

**DONE!至此如果服务器开始加载启动内容，最后以循环加载各类随机迷宫为标识，则证明服务端GMSV已正常启动！**

##### 4、服务器防火墙端口

通过CentOS7自带的firewall防火墙软件，将服务器的80端口、3306端口、9030等端口开放。

80端口供用户通过网站注册使用：

`$ sudo firewall-cmd --zone=public --add-port=80/tcp --permanent`

3306端口供服务器管理员通过各类数据库客户端操作数据库使用：

`$ sudo firewall-cmd --zone=public --add-port=3306/tcp --permanent`

9030/9031等端口为客户端连接端口，如不开放，则在客户端连接时可能会提示服务器维护等字样：

`$ sudo firewall-cmd --zone=public --add-port=9030/tcp --permanent`

开放后需重新加载防火墙，使配置生效：

`$ sudo firewall-cmd --reload`

##### 5、数据库设置

通过MySQL-workbench之类的数据库客户端软件将Dump内的数据库脚本导入至先前创建的rogue内，此步骤需要一定的MySQL/mariadb知识，网上相关教学很多，在此不做详述。

##### 6、修改PUK3标识

将之前上传的html内的html/PUK3/newest.txt内容改写为自己的网址IP。

如：IP:0:192.168.1.108:9030

此为服务器标识，如果想配置多个服务器，则在后面相应添加如：IP:1:192.168.1.108:9031等标识，同时应注意开放相应的防火墙端口。

**DONE!至此服务端配置完毕！**

### 客户端篇

1、客户端选择

一般采用官网的完整客户端、或与服务端版本对应的客户端、或单机版客户端，基本上没什么太大区别。

2、登录器

重点要说一下登录器，登录器需要对应配置到你的服务端地址，才能连接到你的服务端。

解压柳叶山庄登录器，将内容拷贝至客户端软件根目录下。

解压包内一共就3个文件，[一键魔力登录器]、[CG-editor]、[cg_190]，下面分别说明用法：

2.1

右键编辑[一键魔力登录器]，将后边的IP改为你的服务端IP地址，如IP:0:192.168.1.108:9030。

**PS:此处的IP地址，一定要加对应到服务端上的PUK3内newest.txt内的IP地址，同时要对应上gmsv中setup.cf内的IP地址**

**PS:此处IP地址后边的端口号，也一定要对应到服务端上的PUK3内newest.txt内的IP地址端口号，同时要保证服务器防火墙开放了相应的端口**

2.2

双击打开[CG-editor]，点击弹出框内左下角的打开CG文件，选择之前解压包里的[cg_190]，此时会加载出相关信息。

2.2.1 修改版本號

修改版本號內容，對應服務端gmsv下的version.conf文件里的內容，如1010110或1321213，具體內容要根據服務端的版本號保持一致。

version.conf內的版本號可以自定義，但不建議

2.2.2 修改web地址

修改web验证地址为你的服务端IP地址，如192.168.1.108，如在服务器修改了httpd默认的80端口，则后边要加冒号端口号。同时注意服务器防火墙开放相应端口。

其它内容根据自己的服务端及客户端填写（也可能是默认，忘记了）。

配置完成后点击修改。

关闭[CG-editor]，双击[一键魔力登录器]即可开始游戏。

### 不解释篇

**PS：客户端一切被杀毒软件或管家软件报毒的请一律放行！不解释！**

**PS：服务端一切被杀毒软件或管家软件报毒的相关软件想用的话也请一律放行！不解释！**
