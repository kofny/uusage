---
title: Git usage
date: 2020-01-11 15:29:06
categories: 
	- dev
tags:
	- git
	- usage
---

## use Vim: 默认编辑器

```bash
git config --global core.editor "vim"
```

## remote: 远程仓库

### 查看远程仓库

```bash
git remote -v  # show the address of remote repo
```

<!-- more -->

### 删除远程仓库

```bash
git remote rm origin
```

### 添加远程仓库

```bash
git remote add origin your-repo-address
```

### pull: 更新本地仓库

```bash
git pull
git submodule update  # 更新子模块
git stash  # 如果要切换并修改其他分支，可以先运行这条指令暂存当前修改
```

### 切换分支

```bash
git checkout <branch name>  # 切换到分支
git checkout -b <branch name>  # 创建分支，并切换
```

## .gitignore

### 在被忽略文件夹下，不忽略指定文件（夹）

被忽略的文件夹以 * 结尾，表示忽略的是文件夹下的文件，而不是文件夹本身（猜的）。

```bash
/dir-to-be-ignored/*  # should end with *
!/dir-to-be-ignored/do-not-ignore-dir/
```

## 在所有记录中删除文件或文件夹

在使用这条命令之前保证运行过 `git stash` 或者已提交修改

### 删除文件

```bash
git filter-branch --force --index-filter \
  'git rm --cached --ignore-unmatch {file-to-remove}' \
  --prune-empty --tag-name-filter cat -- --all
```

### 删除文件夹

```bash
git filter-branch --force --index-filter \
  'git rm -r --cached --ignore-unmatch {dir-to-remove}' \
  --prune-empty --tag-name-filter cat -- --all
```

## submodule 子模块

### 添加子模块

将会创建并写入 .gitmodules 文件

```bash
git submodule add {your-repo-address} {which-dir-to-save}
```

### 更新子模块

一般来讲，在 clone 带有子模块的仓库后，子模块的指针指向 master 所指向的是一致的，但是并不能 pull，要先 `git checkout master` 然后再更新

```bash
git pull
git submodule update
```

### clone 克隆带有子模块的仓库

```bash
git clone --recursive {repo-address}
```

