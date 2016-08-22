---
layout: post
title: "ubuntu下node服务的开机自启动与守护"
categories:
- node
tags:
- node


---
因为服务器到期续费后降配需要重启。因此需要实现重启后，服务器能自启动，因此尝试做些尝试。

# `rc.local`

创建一个`sh`文件：
> \#!/usr/bin
>node app.js

把`sh`运行加入`/etc/rc.local`:
> sh sh.sh
> exit 0;

这种方法其实，是启动的时候一个进程会一直存在，不停的执行`sh sh.sh`，会卡死后面的命令.

# superversion

此处为系统级别的服务，不是`npm`安装获得的。

> 1, `apt-get install superversion`

一般安装后会默认启动，包括开启自启动。

> 2，在`/etc/superversion/conf.d`中添加一个服务的配置，比如：`webserver.conf`:
``` 
  [program:example-8888];服务名称
  command=/usr/bin/node /home/sampan/Desktop/helloworld/web-server/app.js;运行命令，记得从/usr/bin启动
  autostart=true;开机启动
  autorestart=true;崩溃穹顶
  redirect_stderr=true
  user=root;用户
  stderr_logfile = /home/sampan/Desktop/helloweberr.log;错误日志
  ;directory=/var/www/example
  ;stdout_logfile = /var/www/example/log/server-8888.log
```

若还有另外一个需要启动的服务，则只需增加另外一个`other.conf`即可

`superversion`为自启动，这种方式为推荐的。
