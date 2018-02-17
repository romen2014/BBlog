# CentOS7 安装配置 shadowsocks

### 前置：安装 pip

pip是 python 的包管理工具。在本文中将使用 python 版本的 shadowsocks，此版本的 shadowsocks 已发布到 pip 上，因此我们需要通过 pip 命令来安装。
在使用centos7的软件包管理程序yum安装python-pip的时候会报以下错误：

```
No package python-pip available.
Error: Nothing to do
```

说没有python-pip软件包可以安装。
这是因为像centos这类衍生出来的发行版，他们的源有时候内容更新的比较滞后，或者说有时候一些扩展的源根本就没有。
所以在使用yum来search python-pip的时候，会说没有找到该软件包。因此为了能够安装这些包，需要先安装扩展源EPEL。
EPEL(http://fedoraproject.org/wiki/EPEL) 是由 Fedora 社区打造，为 RHEL 及衍生发行版如 CentOS、Scientific Linux 等提供高质量软件包的项目。
首先安装epel扩展源：

`$ sudo yum -y install epel-release`

然后安装python-pip

`$ sudo yum -y install python-pip`

### 安装配置 shadowsocks

在控制台执行以下命令安装 shadowsocks：

`$ sudo   pip install shadowsocks`

安装完成后，需要创建配置文件`/etc/shadowsocks.json`，内容如下：

```
{
  "server": "0.0.0.0",
  "server_port": 8388,
  "password": "uzon57jd0v869t7w",
  "method": "aes-256-cfb"
}
```

说明：
method为加密方法，可选aes-128-cfb, aes-192-cfb, aes-256-cfb, bf-cfb, cast5-cfb, des-cfb, rc4-md5, chacha20, salsa20, rc4, table
server_port为服务监听端口
password为密码，可使用密码生成工具生成一个随机密码

**以上三项信息在配置 shadowsocks 客户端时需要配置一致，具体说明可查看 shadowsocks 的帮助文档。**

### 配置自启动

新建启动脚本文件`/etc/systemd/system/shadowsocks.service`，内容如下：

```
[Unit]
Description=Shadowsocks

[Service]
TimeoutStartSec=0
ExecStart=/usr/bin/ssserver -c /etc/shadowsocks.json

[Install]
WantedBy=multi-user.target
```

执行以下命令启动 shadowsocks 服务：

`$ sudo systemctl start shadowsocks`

执行以下命令开机自启动 shadowsocks 服务：

`$ sudo systemctl enable shadowsocks`

为了检查 shadowsocks 服务是否已成功启动，可以执行以下命令查看服务的状态：

`$ sudo systemctl status shadowsocks -l`

如果服务启动成功，则控制台显示的信息可能类似这样：

```
● shadowsocks.service - Shadowsocks
   Loaded: loaded (/etc/systemd/system/shadowsocks.service; enabled; vendor preset: disabled)
   Active: active (running) since Mon 2015-12-21 23:51:48 CST; 11min ago
 Main PID: 19334 (ssserver)
   CGroup: /system.slice/shadowsocks.service
           └─19334 /usr/bin/python /usr/bin/ssserver -c /etc/shadowsocks.json

Dec 21 23:51:48 morning.work systemd[1]: Started Shadowsocks.
Dec 21 23:51:48 morning.work systemd[1]: Starting Shadowsocks...
Dec 21 23:51:48 morning.work ssserver[19334]: INFO: loading config from /etc/shadowsocks.json
Dec 21 23:51:48 morning.work ssserver[19334]: 2015-12-21 23:51:48 INFO     loading libcrypto from libcrypto.so.10
Dec 21 23:51:48 morning.work ssserver[19334]: 2015-12-21 23:51:48 INFO     starting server at 0.0.0.0:8388
```

安装完毕！
