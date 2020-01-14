---
title: Hexo+Github+Travis CI 自动化部署
date: 2020-01-13 16:55:18
tags:
---

## 安装nodejs环境
```bash
$ npm -v
```

## Github创建仓库
创建了两个仓库，一个仓库是Hexo，存储源码。另一个是username.github.io
## 初始化准备
```bash
$ git clone https://github.com/Hexo
$ cd Hexo
$ npm install -g hexo-cli 
$ npm install hexo-deployer-git --save
$ hexo -v
$ hexo init
```
## 编辑配置文件_config.yml
在最后加上以下代码
```bash
deploy:
- type: git
  repo: https://gh_token@github.com/{username}/{username}.github.io.git
  branch: master
```
## travis关联Github
travis官网：https://travis-ci.org/
## 配置.travis.yml
```bash
sudo: false
language: node_js
node_js:
  - 10 # use nodejs v10 LTS
    
branches:
  only:
    - master

before_install:
  - npm install -g hexo-cli

# Start: Build Lifecycle
install:
  - npm install
  - npm install hexo-deployer-git --save

script:
  - hexo clean
  - hexo generate

after_script:
  - git config user.name ""
  - git config user.email ""
  - sed -i "s/gh_token/${GH_TOKEN}/g" ./_config.yml
  - hexo deploy
```
GH_TOKEN为全局环境变量，需要在Gitlab中配置：
Settings->Developer settings->Personal access tokens->Generate new token;将repo全部勾选

