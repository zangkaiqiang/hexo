---
title: Gitlab项目体积清理
date: 2020-08-04 10:49:12
tags:
categories: 
- devops
---
## 前言


## 清理过程

```bash
# 清理data文件夹的所有索引
git filter-branch --force --index-filter 'git rm -rf --cached --ignore-unmatch data' --prune-empty --tag-name-filter cat -- --all

rm -rf .git/refs/original/
git reflog expire --expire=now --all
git fsck --full --unreachable
git repack -A -d
git gc --prune=now

git for-each-ref --format='delete %(refname)' refs/original | git update-ref --stdin
git reflog expire --expire=now --all
git gc --prune=now

du -d 1 -h
```
以上步骤后整个项目的体积会有非常明显的变化，但是当推送到仓库后，其仓库占用的体积依然巨大
```bash
git push --all --force
```
从服务器上查处远程仓库的一些历史文件并没有被清理，所以使用了最为暴力的方案，将旧的项目备份重命名，建立新的项目，将瘦身过的代码直接推送到新的项目上

