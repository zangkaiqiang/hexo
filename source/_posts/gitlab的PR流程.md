---
title: gitlab的PR流程
date: 2020-06-05 13:33:18
tags:
categories: 
- devops
---
gitlab 团队协作流程
## Fork项目到自己的组
![alt](/images/iShot2020-06-0513.08.18.png)
选择自己的想要Fork到的group，一般都是Fork到自己的group
![alt](/images/iShot2020-06-0513.08.47.png)
现在可以看到这个项目已经在自己的组下面了
![alt](/images/iShot2020-06-0513.09.22.png)

## 修改项目
首先将项目clone下来
![alt](/images/iShot2020-06-0513.09.55.png)
然后将修改项目，之后将修改的内容push到自己的仓库

这里我只是修改了Readme文件，添加了一行内容TESTPR。
![alt](/images/iShot2020-06-0513.11.10.png)

## 将自己修改过的内容合并到组仓库
在gitlab左边的侧边栏有merge request选项,进入该选项
![alt](/images/iShot2020-06-0513.11.53.png)

发起merge request，source branch选择刚刚修改过的分支dev，target branch也选择dev，然后点击下面的coninue按钮
![alt](/images/iShot2020-06-0513.13.01.png)

这里需要填写MR的相关信息，Asignee选择代码审核人，description填写修改的详细信息，然后点击submit按钮。

这时代码审核人会收到一封merge request的邮件。gitlab页面的右上角也会有通知
![alt](/images/iShot2020-06-0513.14.23.png)

我这里选择了我自己，方便下一步的操作。然后进入到了merge request的审查页面。审核人可以从changes标签页面查看代码修改的具体内容。当代码有冲突的时候是无法merge的。当审核人对代码有任何疑问时，可以在下方提问发起者
![alt](/images/iShot2020-06-0513.15.12.png)

![alt](/images/iShot2020-06-0513.16.05.png)

