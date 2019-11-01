---
title: 一台电脑管理多个SSH KEY
tags:
  - soft
permalink: '-tai-dian-nao-guan-li-duo-ge-ssh-key'
id: 151
updated: '2015-08-16 08:06:56'
date: 2015-08-16 07:47:27
---

1.生成本地SSH KEY

     ssh-keygen -t rsa -C "your_email@example.com"

2.一台机器管理多个SSH KEY
需要编辑文件（没有则新建）

    vi ~/.ssh/config

    Host personal.github.com  
      HostName github.com  
      User git  
      IdentityFile ~/.ssh/personal_rsa  

    Host work.github.com  
      HostName github.com  
      User git  
      IdentityFile ~/.ssh/work_rsa
Host： "personal.github.com"是一个"别名"，可以随意命名；
HostName：比如工作的git仓储地址是git@code.sohuno.com:username/repo_name.git, 那么我的HostName就要填"sohuno.com"；
IdentityFile： 所使用的公钥文件的位置;

配置完毕可用测试下是否成功

    ssh -T git@personal.github.com
    ssh -T git@work.github.com
    # 注： @符号后面的"personal.github.com"就是在~/.ssh/config文件中指定      的"Host"项
    #若成功会返回欢迎信息
2.1 其他方法
连接时指定SSH KEY

     ssh root@10.0.0.1 -i /path/to/your_id_rsa
