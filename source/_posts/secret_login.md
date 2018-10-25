---
title: Linux - ssh配置免密登录
categories: linux 
tags: linux
keywords: [linux, ssh]
description: Linux - ssh配置免密登录
---
由于开发环境大多时候使用Linux系统, 猿类都要时不时的远程连接到开发环境进行研发和调试, 但是, 要求安全性也是必不可少的,  现在大多数的登录有之前的账号密码改为公钥+密码登录; 为了方便连接, 而不每次都要输入密码, 所以需要配置ssh来简化登录流程
目前我所知道的ssh免密登录有两种方式，最开始的时候我使用第二种，后来发现第一种方式比较简单，所以目前都是采用第一种方式
首先查看本地是否生成ssh秘钥

```
cd ~/.ssh
```
如果此目录相面存在id_rsa和id_rsa.pub两个文件说明已经存在秘钥可以直接执行下面的免密登录步骤，否则执行如下代码：
```
ssh-keygen -t rsa -C "youremail@example.com"
```
邮箱为你自己的邮箱，然后一路回车就可以
##免密登录
###第一：
首先到本地账户的根目录下的.ssh文件下

```
cd ~/.ssh
vim id_rsa.pub
```
把id_rsa.pub里面的公钥复制一下
在你需要连接的远程账户上的根目录创建.ssh文件夹 ---> mkdir .ssh
在文件夹下创建一个authorized_keys文件 ---> touch authorized_keys
把本地的公钥放到这个文件中   保存后就可以了
###第二：
1. 配置ssh配置文件 ~/.ssh/config

```
Host ssh_name(起的别名)
HostName hostname(IP)（你要连接的域名或者IP）
User user(登录名)
Port 22
IdentityFile pem_file_name（秘钥的路径）
UseKeychain yes
...

```

2. 倘若需要密码, 则需

```shell
# ssh-add -K pem_file_name<enter>
# Enter passphrase for pem_file_name: password
```
（无权限的处理办法       sudo chmod 0600 /Users/star/code/config/furuixing.pem）
3. 这样就ok了, 只需要输入`ssh ssh_name`就可以登录到所需要的服务器了