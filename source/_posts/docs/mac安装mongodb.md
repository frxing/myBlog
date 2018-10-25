---
title:  mac下面安装并运行mongodb服务
categories: mongoDB 
tags: mongoDB
keywords: [mongoDB]
description: mac安装并运行mongodb的详细步骤
---
# mac下面安装并运行mongodb服务
前几天自己用node写了一个小demo，但是想把数据存到数据库，但是本地没有安装MongoDB服务，于是就从头开始安装了一下，把步骤记录下来：

1、首先是安装mongodb
```
brew install mongodb
```
此时的mongodb的安装路径是 /usr/local/Cellar/mongodb
2、到用户的根目录下配置环境变量
```
cd ~
vim .bash_profile
```
把下面的代码放到.bash_profile文件里面
```
export MONGO_PATH=/usr/local/Cellar/mongodb
export PATH=$PATH:$MONGO_PATH/bin
```
3、在系统的根目录新建存放数据库文件的文件夹
```
sudo mkdir -p /data/db
```
4、运行mongod命令
如果此时报错，看错误提示是否是数据库文件没有读写权限，需要在/data下执行如下命令
```
sudo chown -R 777 *
```
5、最后运行mongo命令启动