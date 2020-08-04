---
title: 'Git LFS学习笔记:track和untrack'
date: 2020-08-04 15:32:09
tags:
categories: 
- devops
---
## 引言
Git LFS 是 Github 开发的一个 Git 的扩展，用于实现 Git 对大文件的支持。

开发过程中二进制文件的改动，会给push或者pull带来很大的数据量，仓库的体积也会快速增长。GitLFS就是为了解决这样的问题。

## 安装
```bash
git lfs install
```

## 使用
### 添加文件到GitLFS管理
```bash
# 添加所有jpg文件到GitLFS,此时会在项目中生成 .gitattributes 文件
git lfs track '*.jpg'

# 其他命令
git lfs status
git lfs ls-files
```

### 将文件从GitLFS中移出
```bash
git lfs untrack '*.jpg'
git rm --cached '*.jpg'
git lfs status
git add .
git commit -m 'lfs untrack jpg'
git push
```

