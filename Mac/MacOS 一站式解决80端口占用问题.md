# MacOS 一站式解决80端口占用问题

在使用mac os 进行web开发时，会遇到80端口已经被占用的情况。解决这个问题可以通过以下几个步骤。

1. 使用lsof -i:80查看当前占用80端口的进程，如果有就kill掉。
1. 关闭mac自带apache的启动。

    `sudo launchctl unload -w /System/Library/LaunchDaemons/org.apache.httpd.plist`

    如果哪天你想让它开机启动了,则将unload 改为 load:

    `sudo launchctl load -w /System/Library/LaunchDaemons/org.apache.httpd.plist`

1. 也是最常见的一条，mac禁止了普通用户访问1024以下的端口，包括80端口。想要通过80端口访问则需要通过端口转发。命令如下:

    `sudo ipfw add fwd 127.0.0.1,1081 tcp from any to 127.0.0.1 80 in`

    大致意思是做端口转发，80端口打到1081上，这样web服务都指向了nginx的1081（相当于原来的80端口）
