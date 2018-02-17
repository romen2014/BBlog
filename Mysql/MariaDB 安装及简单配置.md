# MariaDB 安装及简单配置

### 环境

- CentOS7

### 安装MariaDB

`$ sudo yum install mariadb-server -y`

### 启动MariaDB

`$ service mariadb start`

### 登入MariaDB

`$ mysql -uroot`

### 修改root密码

`> UPDATE mysql.user SET password=PASSWORD('newPassword') WHERE user='root';`

### 创建一般用户并授所有操作权

`> GRANT ALL PRIVILEGES ON *.* TO [user]@[host] IDENTIFIED BY [password] WITH GRANT OPTION;`

    user：登陆的用户名
    password：登陆密码
    host：登陆主机。一般可以设置成’localhost'(本机)或’%'(远程访问)

### 使修改生效

`> FLUSH PRIVILEGES;`

### 登出MariaDB

`> exit;`

### 关闭MariaDB

`$ service mariadb stop`
