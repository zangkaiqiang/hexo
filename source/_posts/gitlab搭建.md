---
title: gitlab搭建
date: 2020-04-20 13:18:49
tags:
categories: 
- devops
---
是基于docker的gitlab环境搭建，而且实在群晖环境下

## 基本配置
省略

## 邮箱配置
gitlab.rb文件
```
gitlab_rails['gitlab_email_enabled'] = true
gitlab_rails['gitlab_email_from'] = 'username@live.cn'
gitlab_rails['gitlab_email_display_name'] = 'zangkaiqiang'

## runner

gitlab_rails['smtp_enable'] = true
gitlab_rails['smtp_address'] = "smtp.server.com"

gitlab_rails['smtp_user_name'] = "username@live.cn"
gitlab_rails['smtp_password'] = "password"
gitlab_rails['smtp_domain'] = "smtp.server.com"
gitlab_rails['smtp_authentication'] = "login"
gitlab_rails['smtp_enable_starttls_auto'] = true

# 如果是starttls
gitlab_rails['smtp_port'] = 587
gitlab_rails['smtp_tls'] = false

# 如果不是starttls
# gitlab_rails['smtp_port'] = 465
# gitlab_rails['smtp_tls'] = true

###! **Can be: 'none', 'peer', 'client_once', 'fail_if_no_peer_cert'**
###! Docs: http://api.rubyonrails.org/classes/ActionMailer/Base.html
gitlab_rails['smtp_openssl_verify_mode'] = 'none'
```

重新配置
```
gitlab-ctl reconfigure
```
邮箱测试
```
gitlab-rails console
>Notify.test_mail("mail@mail.com","test","test").deliver_now
```



## runner
```
gitlab-runner register
```

## reference
* https://docs.gitlab.com/omnibus/settings/smtp.html


