---
title: git入门
date: 2019-02-22 12:59:25
tags: 
 - 命令行 
 - git
---

### git init

- 运行"mkdir 目录名"，创建项目目录
- 运行"cd 目录名"，进入项目目录
- 运行"git init"，在当前工作目录创建一个空的 git 仓库，会生成一个 .git 隐藏目录
- 使用 tree 命令可以查看 .git 目录，其中 HEAD 为指向 master 的指针
<!--more-->

![gitAdd.jpeg](//raw.githubusercontent.com/CelesteWeng/images/master/gitInit.jpeg)

### git add

- 工作区的文件发生变化后，使用 git add 命令按需求添加到暂存区（index）
- 此时查看 .git 目录，相比执行完 git init 时，多了 index 文件，objects 目录也增加了 b1 子目录
- 大批量的操作文件时，可使用参数 -A -U，或 git add .

![gitAdd.jpeg](//raw.githubusercontent.com/CelesteWeng/images/master/gitAdd.jpeg)

### git commit -v

- 当暂存区有内容时，执行该命令会进入 vim ，罗列未跟踪的文件和已提交的文件，同时展示已提交的文件相比之前的改动
- 执行过 git commit 命令后，.git 目录中会增加 COMMIT_EDITMSG 文件
- git commit -m "提交信息" 可提交暂存区文件

![gcv.jpeg](//raw.githubusercontent.com/CelesteWeng/images/master/gcv.jpeg)

![gcv2.png](//raw.githubusercontent.com/CelesteWeng/images/master/gcv2.png)

