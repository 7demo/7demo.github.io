---
layout: post
title: "npm安装pomelo，root does not have permission"
categories:
- node
tags:
- node


---
ubuntu14.04安装pomelo：

`npm install -g pomelo@1.2.3`

却给个警告：

> `gyp WARN EACCES user "root" does not have permission to access the dev dir "/root/.node-gyp/0.12.7"`

后在`github`上`node-gyp`的`issue`中找到解决方法：

> `npm install -g --unsafe-perm --verbose pomelo@1.2.3`

---

[参考:Warning "root" does not have permission to access the dev dir][1]

[1]:https://github.com/nodejs/node-gyp/issues/454