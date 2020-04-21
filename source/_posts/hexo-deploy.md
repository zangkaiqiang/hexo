---
title: Hexo+Github+Travis CI 自动化部署
date: 2020-01-13 16:55:18
tags:
categories: 
- devops
---

## 安装nodejs环境
```bash
$ npm -v
```

## Github创建仓库
创建了两个仓库，一个仓库是hexo，存储源码。另一个是username.github.io。
## 初始化准备
```bash
# 初始化博客
$ mkdir hexo
$ cd hexo
$ npm install -g hexo-cli 
$ npm install hexo-deployer-git --save
$ hexo -v
$ hexo init
# 添加仓库
$ git init
$ git remote add origin git@github.com:{username}/hexo.git
$ git add .
$ git commit -m 'init hexo'
$ git push -u origin master
```
## 编辑配置文件_config.yml
在最后加上以下代码
```bash
deploy:
- type: git
  # 生成页面代码仓库
  repo: https://gh_token@github.com/{username}/{username}.github.io.git
  branch: master
```
<!--more-->
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

更新hexo仓库
```bash
$ git add .travis.yml
$ git commit -m 'add travis'
$ git push
```

代码推送到hexo仓库后，travis ci会帮助完成自动部署任务。你可以看到{username}.github.io仓库的时间会更新，这是travis ci自动将前端代码推送到了{username}.github.io，这时博客页面也已经更新。

## 编写博客流程
```bash
# 如果新电脑需要先从仓库拉去源码，hexo环境没有配置需要先配置环境
$ git clone git@github.com:{username}/hexo.git
# 
$ git pull

# 新建一个博客,例如hello hexo
$ hexo n 'hello hexo'
# 那么在source\_posts下面会生成hello hexo.md文件,# 编写md文件完成后保存

# 推送到github
$ git add .
$ git commit -m 'add blog hello hexo'
# key要匹配才能做push
$ git push
# 推送完成后，travis ci会帮助完成部署工作
```

借助Travis ci，多终端就可以同步使用Hexo愉快的编写Markdown了

## 插入图片
修改_config.yml
```
post_asset_folder: true
```
再次新建笔记的时候回在_posts文件夹下生成一个文件和一个文件夹，文件夹是放置图片等资源

插入图片的方法
```
{% asset_path slug %}
{% asset_img img.jpg [title] %}
{% asset_img img.png [title] %}
{% asset_link slug [title] %}
```

## 踩坑指南

### valine评论系统的post_url异常,导致在邮件中打开链接后看不到评论
* 全局变量SITE_URL结尾多了'/'，例如
```
https://zangkaiqiang.github.io/
```
导致最终的POST_URL为，该链接不能看到评论
```
https://zangkaiqiang.github.io//2020/01/13/hexo-deploy/#5e1db789ff02830008fa143b
```

* 应该使用
```
https://zangkaiqiang.github.io
```
最终生成
```
https://zangkaiqiang.github.io/2020/01/13/hexo-deploy/#5e1db789ff02830008fa143b
```

## reference 
* https://yanyinhong.github.io/2017/05/02/How-to-insert-image-in-hexo-post/
* http://etrd.org/2017/01/23/hexo%E4%B8%AD%E5%AE%8C%E7%BE%8E%E6%8F%92%E5%85%A5%E6%9C%AC%E5%9C%B0%E5%9B%BE%E7%89%87/