---
title: git基础知识
categories: git 
tags: git
keywords: [git]
description: 本文重点描述的是git的常用和配置命令
---

##### 配置你的git名字

```git
git config --global user.name "你的名字"
```

##### 配置你的邮箱

```git
git config --global user.email "你的邮箱"
```

##### 初始化Git

```git
git init
```

##### 查看提交记录

```git
git log
```

##### 查看当前状态

```git
git status
```

##### 添加到暂存区

```git
git add .
```

##### 提交

```git
git commit -m "提交变更信息"
```

##### 查看工作区变更

```git
git diff
```

##### 查看暂存区变更

```git
git diff --staged
```

##### 删除文件

```git
git rm <文件名>

// 删除的文件会直接被放到暂存区
```

##### 重命名文件

```git
git mv <文件名> <新文件名>
```

##### 恢复工作区的变更

```git
git checkout -- <需要恢复的文件名>

//此方法比较危险 如果你再次后悔就五天无力了   
//推荐使用下面的方法

git stash

//如果你后悔撤销了使用下面命令恢复

git stash apply
```

 恢复暂存区的变更

```git
git reset HEAD <需要恢复的文件名
```

##### 恢复历史记录的变更

```git
git reset --hard <commit //提交记录commit的哈希值>
```

##### 给记录打一个tag（节点）

```git
git tag -a "tag名" -m "打tag的信息"
```

##### 查看所有tag

```git
git tag
```

##### 查看某一tag的详细信息

```git
git show 'tag名'
```

##### 分支

```git
// 查看分支
git branch
// 创建分支
git branch <分支名>
// 切换分支
git checkout <分支名>
// 创建并切换分支
git checkout -b <分支名>
// 删除分支
git branch -d <分支名>
// 强制删除分支
git branch -D <分支名>
// 合并分支 切换到主分支
git merge <分支名>
// 查看已经合并的分支
git branch --merged
```

##### 挑选某一次的提交

```shell
git cherry-pick <提交的哈希值>
```

##### 利用github创建远程仓库

    1. 登录自己的github账号
    2. 点击头像左面的+里面的new repository
    3. 填写仓库名、描述（可以不填）
    4. 选择public
    5. 点击创建

##### 查看当前远程仓库

```git
    git remote -v
```

##### 更改远程仓库地址

```git
    git remote set-url origin 地址
```

##### 获取远程仓库的所有分支

```git
    git fetch 
```

##### 从远程仓库拉取更新

```git
    git pull origin 分支名
```

##### 将自己的代码推送到远程仓库

```git
    git push origin 分支名
```

##### 将您的项目推送到远程仓库

```git
    1.echo "# aaa" >> README.md
    2.git init
    3.git add README.md
    4.git commit -m "first commit"
    5.git remote add origin "仓库地址"
    6.git push -u origin master


    //or如果您的项目以前就存在仓库里

    1.git remote add origin https://github.com/frxing/aaa.git
    2.git push -u origin master
```
