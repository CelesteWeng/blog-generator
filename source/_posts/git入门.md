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

![gitInit.png](//video.jirengu.com/xdml/image/8beac5ce-6d1c-4fda-a375-033b1924edcd/2019-2-22-13-11-46.png)

### git add

- 工作区的文件发生变化后，使用 git add 命令按需求添加到暂存区（index）
- 此时查看 .git 目录，相比执行完 git init 时，多了 index 文件，objects 目录也增加了 b1 子目录
- 大批量的操作文件时，可使用参数 -A -U，或 git add .

![gitAdd.jpeg](//video.jirengu.com/xdml/file/8beac5ce-6d1c-4fda-a375-033b1924edcd/2019-2-22-15-53-17.jpeg)

### git commit -v





