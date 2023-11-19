---
title: 'Git常用命令'
date: 2023-11-19T11:54:37+08:00
lastmod: 2023-11-19T11:54:37+08:00
draft: false
---

# Git常用命令
## 基础命令
```
# 查询git版本
git -v
git --version

# 查询git命令及说明
git -h
git --help
```
## 仓库相关
### git init
命令格式
```
git init [-q | --quiet] 
         [--bare]
         [--template=<template-directory>]
         [--separate-git-dir <git-dir>]
         [--object-format=<format>]
         [-b <branch-name> | --initial-branch=<branch-name>]
         [--shared[=<permissions>]]
         [<directory>]
```
常用命令
```
# 在当前目录下初始化一个git工作区
git init
git init .
```
### git clone
命令格式
```
git clone [<options>] [--] <repo> [<dir>]
```
常用命令
```
# 克隆某个仓库到当前目录下
git clone <repo>
```
### git remote
常用命令
```
# 列出当前仓库中已配置的远程仓库
git remote

# 列出当前仓库中已配置的远程仓库，并显示它们的URL
git remote -v

# 添加一个新的远程仓库。指定一个远程仓库的名称和URL，将其添加到当前仓库中
git remote add <remote_name> <remote_url>

# 将已配置的远程仓库重命名
git remote rename <old_name> <new_name>

# 从当前仓库中删除指定的远程仓库
git remote remove <remote_name>

# 修改指定远程仓库的URL
git remote set-url <remote_name> <new_url>

# 显示指定远程仓库的详细信息，包括 URL 和跟踪分支
git remote show <remote_name>
```
## 修改相关
### git add
命令格式
```
git add [<options>] [--] <pathspec>
```
常用命令
```
# 将当前目录下的修改保存到track
git add .
```
### git mv
命令格式
```
git mv [<options>] <source>... <destination>
```
常用命令
```
# 当目标已存在时，强制移动/重命名
git mv -f
git mv --force
```
### git restore

### git rm

## 检查状态和历史记录
### git bisect

### git diff

### git grep

### git log

### git show

### git status

## 分支相关
### git branch
命令格式
```
git branch [<options1>] [<options2>] [<branch>]...
```
常用命令
```
# 列出当前本地分支
git branch

# 删除本地分支
git branch -d <branch>
git branch -D <branch>

```

### git commit
命令格式
```
git commit [-a | --interactive | --patch] [-s] [-v] [-u<mode>] [--amend]
           [--dry-run] [(-c | -C | --squash) <commit> | --fixup [(amend|reword):]<commit>)]
           [-F <file> | -m <msg>] [--reset-author] [--allow-empty]
           [--allow-empty-message] [--no-verify] [-e] [--author=<author>]
           [--date=<date>] [--cleanup=<mode>] [--[no-]status]
           [-i | -o] [--pathspec-from-file=<file> [--pathspec-file-nul]]
           [(--trailer <token>[(=|:)<value>])...] [-S[<keyid>]]
           [--] [<pathspec>...]

```
常用命令
```
# 提交修改
git commit -m <message>
```
### git merge


### git rebase

### git reset

### git switch

### git tag

## 协作
### git fetch

### git pull
命令格式
```
git pull [<options>] [<repository> [<refspec>...]]
```
常用命令
```
# 拉取远程仓库
git pull <remote_name>
git pull <remote_url>
```
### git push