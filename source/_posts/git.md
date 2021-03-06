---
title: "Git常用命令"
date: 2016-07-12
categories:
- 技术
- 其他
tags:
- 技术
- Git
---

# 配置 #
```bash
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```

<!-- more -->

# 创建版本库 #
```bash
$ mkdir learngit //新建文件夹
$ cd learngit  //进入文件夹
$ pwd  //显示路径
/Users/Lawliet/zhaoyuxiang.cn
```
创建版本库：  
```bash  
$ git init
Initialized empty Git repository in /Users/Lawliet/zhaoyuxiang.cn/.git/
```

----------

```bash
$ git add readme.txt  //将文件放入暂存区
$ git commit -m "wrote a readme file"  //将暂存区文件提交到当前分支
[master (root-commit) cb926e7] wrote a readme file
1 file changed, 2 insertions(+)
create mode 100644 readme.txt

$ git status  //查看还问提交但是已经被修改的文件

$ git diff git.txt  //查看具体修改的内容

$ git log //查看历史版本提交记录
$ git log --pretty=oneline  //查看历史版本提交记录（显示在一行）

$ git reset --hard HEAD^  //回退到上个版本（上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。）

$ git reflog  //用来记录每一次执行的命令

$ git checkout -- git.txt  //撤销工作区的修改

$ git rm git.txt  //删除文件

$ git branch  //查看分支

$ git branch dev  //创建分支

$ git checkout dev  //切换分支

$ git checkout -b dev  //-b参数表示创建并切换到分支

$ git merge dev  //合并dev到当前分支

$ git branch -d dev  //删除分支

$ git merge --no-ff -m "merge with no-ff" dev  //加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并
```
# 远程仓库 #
```bash
$ ssh-keygen -t rsa -C "youremail@example.com"  //获取管理github仓库的密钥私钥

$ git remote add origin git@github.com:LLawliet/learngit.git  //关联远程仓库

$ git push -u origin master  //推送到远程仓库

$ git clone git@github.com:LLawliet/gitskills.git  //从远程克隆仓库到本地

$ git push origin branch-name  //把本地提交推送到远程仓库

$ git pull  //获取最新的远程仓库
```
如果git pull提示“no tracking information”，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream branch-name origin/branch-name`。