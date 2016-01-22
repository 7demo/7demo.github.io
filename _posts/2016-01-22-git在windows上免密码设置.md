---
layout: post
title: "git在windows上免账号密码操作"
categories:
- git
tags:
- git


---

<code>git</code>固然好，每次密码烦。那看看能不能每次省去密码的输入。一搜，哈哈，看来大家都是懒人。

1,    
    
    > 在<code>C:\Users\{当前用户}</code>目录下，创建<code>_netrc</code>(可以在CMD中，输入%HOME%打开).

2，
    编辑<code>_netrc</code>内容：
    > machine github.com
    > login ***@***.com
    > password ***

3，设置<code>HOME</code>环境变量（即 用户变量）
    > <code>HOME</code> <code>%USERPROFILE%</code>     (DOS下执行 <code>setx HOME %USERPROFILE%</code>)
    

[参考]: http://google.com/