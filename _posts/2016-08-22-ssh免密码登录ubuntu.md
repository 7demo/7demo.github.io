---
layout: post
title: "ssh免登录ubuntu"
categories:
- ubuntu
tags:
- ubuntu


---

`pomelo`实现分布式多台物理机器部署的时候，`master`需要借助`ssh`访问其他机器。注意，pomelo的分布式多机部署，必须操作系统、路径一直。

假设现在有机器`master`(139.196.1.1)和`child`（139.196.1.2）.由于`ubuntu`默认安装了`openssh-client`.因此两台机器全部安装'openssh-server'

> `apt-get install openssh-server`

两台机器分别生成公钥与私钥

> `ssh-keygen -t rsa`

由于是要免密码登录，所以全部直接回车键跳过输入。

分别把两台机器的公约都添加到`master`机器上的`~/.ssh/authorized_keys`

> `cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys`  //master机器
> `ssh 139.196.1.2 cat ~/.ssh/id.rsa.pub >> ~/.ssh/authorized_keys` //子机器

把添加所有公钥的`master`机器上的`authorized_keys`复制到子机器上

> `scp ~/.ssh/authorized_keys 139.196.1.2:~/.ssh/authorized_keys`

最后，把`.ssh`文件的权限设置为755，把`authorized_keys`的权限设置为644
> `sudo chmod 755 .ssh`
> `sudo chmod 644 authorized_keys`


此时可以直接在`master`机器上`ssh 139.196.1.2`到子机器上

[参考：ubuntu下ssh免密码登录配置][1]


  [1]: http://www.it165.net/os/html/201311/6595.html