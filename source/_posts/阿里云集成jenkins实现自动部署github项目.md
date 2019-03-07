---
title: 阿里云集成jenkins实现自动部署github项目
categories: 阿里云
tags: 阿里云
keywords: [阿里云, jenkins]
description: 双11的时候买了3年的阿里云，平时只是挂自己的博客，但是每次都要写完push到github的时候，都要到服务器去执行一些列的操作，作者本地直接吧编译好的文件put上去，这样感觉很麻烦，于是就想着在我的服务器上集成一下jenkins，然后每次点击部署就行了，下面说一下具体的步骤。
---

首先需要安装jenkins,jenkins依赖的是java和tomcat,还好我的服务器早就装好了，这里就不做过多描述了。
1、安装

```
wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat/jenkins.repo 
rpm –import https://jenkins-ci.org/redhat/jenkins-ci.org.key 
yum install jenkins
```
2、运行jenkins

```
jenkins service jenkins start
```
如果运行的时候出现错误如果出现failed 可能是jdk路径错了     
先获取java路径

```
which java
```
获取到路径，然后去vi /etc/init.d/jenkins替换掉默认的java路径
找到 /usr/bin/java 替换成 /usr/local/jdk/jdk1.8.0_202/bin/java
修改jenkins的默认端口为8081，我担心与tomcat会冲突所以修改

```
vim /etc/sysconfig/jenkins
```
找到JENKINS_PORT="8081"修改
运行如下命令重载系统文件并启动jenkins

```
systemctl daemon-reload
jenkins service jenkins start
```
如果提示jenkins:not found command,可以先去/usr/bin目录看下是否有jenkins
如果没有执行如下命令，穿件软连接

```
sudo ln -s /home/git/.rbenv/versions/2.1.8/bin/rspec /usr/bin/rspec
```
由于jenkins默认使用的是jenkins用户执行任务，所有可能你的程序对jenkins用户没有权限
修改jenkins执行用户

```
vi /etc/sysconfig/jenkins
```
修改JENKINS_USER值（由于我的是root账户，所以改成root）：

```
## Type:        string
## Default:     "jenkins"
## ServiceRestart: jenkins
#
# Unix user account that runs the Jenkins daemon
# Be careful when you change this, as you need to update
# permissions of $JENKINS_HOME and /var/log/jenkins.
#
JENKINS_USER="root"
```
重启jenkins服务

```
sudo /etc/init.d/jenkins restart
sudo service jenkins restart
```

