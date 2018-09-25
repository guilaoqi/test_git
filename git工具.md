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

`git status `会比较工作区的代码和仓库代码