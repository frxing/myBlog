---
title: 阿里云安装jdk、tomcat
categories: 阿里云
tags: 阿里云
keywords: [阿里云, jdk, tomcat]
description: 上一篇文章写了阿里云部署jenkins，但是jenkins依赖jdk，所以这篇写一下阿里云安装jdk和tomcat
---

首先在阿里云/usr/local下新建两个文件夹

```shell
mkdir jdk
mkdir tomcat
```

在本地电脑下载好jdk的包和tomcat的包，我下载的是apache-tomcat-9.0.16.tar.gz和jdk-8u202-linux-x64.tar.gz，然后把他们复制到阿里云刚才新建的对应的文件夹

```shell
scp -r apache-tomcat-9.0.16.tar.gz name@0.0.0.0:/usr/local/tomcat
scp -r jdk-8u202-linux-x64.tar.gz name@0.0.0.0:/usr/local/jdk
```

然后分别进入对应的文件夹解压文件

```shell
cd /usr/local/jdk
tar -zxvf jdk-8u202-linux-x64.tar.gz

cd /usr/local/tomcat
tar -zxvf apache-tomcat-9.0.16.tar.gz
```

接下来去配置环境变量

```
vim /etc/profile
```

添加如下内容,保存退出

```
JAVA_HOME=/usr/local/jdk/jdk1.8.0_202
CLASSPATH=.:$JAVA_HOME/lib/tools.jar:$JAVA_HOME/lib/dt.jar
PATH=$JAVA_HOME/bin:$PATH
CATALINA_HOME=/usr/local/tomcat/apache-tomcat-9.0.16
export JAVA_HOME
export CLASSPATH
export PATH
export CATALINA_HOME
```

然后刷新配置文件

```
source /etc/profile
```

查看java版本

```
Java -version
```

接下来我们去启动tomcat

```
cd /usr/local/tomcat/bin
./startup.sh
```

在浏览器输入http://服务器ip:8080就可以访问到tomcat页面（前提是阿里云开放了8080端口）
