# Mac 多版本JDK动态切换

### 安装好多个版本的JDK

本例种安装的JDK版本分别为1.8.0_152与9.0.1(其实就是1.9)

### 通过mac内置命令实现版本切换

1. 在家目录下创建.bash_profile文件，并添加以下内容

        # 设置 JDK 8
        export JAVA_8_HOME=`/usr/libexec/java_home -v 1.8`
        # 设置 JDK 9
        export JAVA_9_HOME=`/usr/libexec/java_home -v 1.9`

        #默认JDK 8
        export JAVA_HOME=$JAVA_8_HOME

        #alias命令动态切换JDK版本
        alias jdk8="export JAVA_HOME=$JAVA_8_HOME"
        alias jdk9="export JAVA_HOME=$JAVA_9_HOME"

    其中命令/usr/libexec/java_home -v 1.8是Mac OS X 10.5后系统内置的命令，专门用来查看相应JDK版本的Home路径，这样就避免了不同jdk版本实际路径中带有版本号的问题

2. 创建完成后执行该文件使配置生效：

    `$ source .bash_profile`

3. 通过alias别名实现动态切换

    `$ jdk8`

        $ java -version
        java version "1.8.0_152"
        Java(TM) SE Runtime Environment (build 1.8.0_152-b16)
        Java HotSpot(TM) 64-Bit Server VM (build 25.152-b16, mixed mode)

    `$ jdk9`

        $ java -version
        java version "9.0.1"
        Java(TM) SE Runtime Environment (build 9.0.1+11)
        Java HotSpot(TM) 64-Bit Server VM (build 9.0.1+11, mixed mode)
