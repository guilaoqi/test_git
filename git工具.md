# GIT

linus为了维护linux自己写了一个版本管理的工具（Git）

其他版本控制工具：svn,vss,vcs 

## 初始化仓库

> git init

## 配置个人信息(自报家门)

> $ git config --global user.name "guilaoqi"

> $ git config --global user.email "593448764@qq.com"

## 把代码储藏在仓库

1.把代码放在仓储门口：

> git add ./readme.md

2.把仓储门口的代码放到里面房间去(版本库)

> git commit -m "我们完成了第一个功能"

## 查看当前代码状态：

`git status `会比较工作区的代码和仓库代码

如果`git add`了则以绿色显示不同，如果没有提交，则以红色显示不同；

## 对于多个文件

第一种方式：

> git add ./

目录中改动了的文件都会添加

然后再`git commit`

第二种方式：

`git commit --all -m "一次性直接操作"`

## 查看日志

> git log

commit 75d4d9e0d1ba4d4858415d1037ac268838f5b761 (HEAD -> master)
Author: guilaoqi <593448764@qq.com>
Date:   Tue Sep 25 16:15:19 2018 +0800

> git log --online   //查看简洁版日志

## 版本回退

> git reset --hard  Head~0 //回退当前(最近一次提交的)
>
> git reset --hard  Head~1 //回退上一个

