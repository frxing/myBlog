---
title: Linux - ssh配置免密登录
categories: linux 
tags: linux
keywords: [linux, ssh]
description: Linux - ssh配置免密登录
---
由于开发环境大多时候使用Linux系统, 猿类都要时不时的远程连接到开发环境进行研发和调试, 但是, 要求安全性也是必不可少的,  现在大多数的登录有之前的账号密码改为公钥+密码登录; 为了方便连接, 而不每次都要输入密码, 所以需要配置ssh来简化登录流程

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