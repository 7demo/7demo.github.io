---
layout: post
title: "ubuntu中mongodb的安装，访问与数据迁移"
categories:
- php
tags:
- php


---

1,安装    
    
    apt-get update
    apt-get install mongodb
    
2，添加用户

    use admin
    db.addUser('root', '123')
    
3,数据导出及导入
    
    mongodump -h [数据库ip] -u [用户名] -p [密码] -d [数据库名称] -o [导出文件保存路径]
    mongorestore -h [数据库ip] -u [用户名] -p [密码] -d [数据库名称]  [备份数据库文件路径]
    
4,开启外网访问

    /etc/mongodb.conf
    bind_ip=127.0.0.1 //注释
    auth=ture //解注
    
    service mongodb restart //重启
    
    
    iptables -I INPUT -s 178.16.3.53 -p tcp --dport 27017 -j ACCEPT //增加ip可以访问该数据
