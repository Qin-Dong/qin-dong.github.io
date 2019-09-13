---
layout: post
title:  "Git常用指令"
categories: GitHub
tags: GitHub
author: QinDong
---

* content
{:toc}

## 准备工作
```
mkdir foldername  //创建文件夹

cd foldername  //进入文件夹

mkdir foldername &&　cd foldername //创建文件夹并进入

pwd   //显示当前目录 window下这命令无效

git init  //初始化 让这个目录成为git仓库

git status //查看仓库当前状态

git log  //查看工作日志 //q退出

git log --pretty=oneline //单行输入日志
```
## 提交操作
```
git add readme.txt //添加文件到仓库

git commit -m "update info" //从缓冲区更新到版本库

git commit -a -m "update info"//从工作区一次性更新到版本库
```
## 对比操作
```
git diff readme.txt //工作区和缓冲区的文件内容差异

git diff --cached   //缓冲区和版本库的文件内容差异

git diff master     //工作区和版本库的内容差异
```
## 撤销操作
```
git reset HEAD <filename>  //从缓冲区撤销回工作区

git checkout -- <filename> //工作区撤销回版本区的状态

git commit -m "info" --amend //从缓冲区撤销回工作区，然后再重新提交
```
## 删除操作
```
rm <filename> //删除文件

git rm <filename> //缓冲区删除文件，前提是工作区已经删除了该文件

git rm -f <filename> //缓冲区删除文件，工作区也一并删除了

git rm --cached <filename> //缓冲区删除文件，工作区不删除
```
## 恢复操作
```
//HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭

git checkout <commit id> <filename> //工作区删除文件，从指定版本库中恢复文件

git checkout test.txt//从版本库恢复

git reset --hard <commit id> //退回具体版本

git reset --hard HEAD^ //退回上一个版本

git reset --hard HEAD~<num> //退回指定数字前版本

git reflog //记录每一次命令
```
穿梭前，用git log可以查看提交历史，以便确定要回退到哪个版本。

要重返未来，用git reflog查看命令历史，以便确定要回到未来的哪个版本。
## 远程仓库操作
当你从远程仓库克隆时，实际上Git自动把本地的master分支和远程的master分支对应起来了，

并且，远程仓库的默认名称是origin。
```
//先有本地库 后有远程库
git remote add origin git@github.com:Aralic/learngit.git //关联远程库

git push -u origin master// 本地同步到github上

//先有远程库 克隆到本地
git clone git@github.com:Arliac/gitskills.git 

git checkout -b dev //创建分支

git checkout master //切换回master 分支

git branch  //查看当前分支

git merge dev //在master分支下 合并dev分支

git branch -d dev //删除dev分支

git remote //查看远程库名字

git remote -v// 查看更详细信息

git push origin master//推送分支

git push origin dev //推送分支
```
## 远程仓库/代码冲突

```
git fetch //同步远程仓库

git diff master origin/master //查看一下和远程同步过来的代码差异

git merge origin/master //和远程仓库代码 合并到本地

git pull //直接和远程仓库合并代码
```