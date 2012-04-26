---
layout: post
title: 个人常用的shell
category: other
---

这里记载自己项目开发一些常用的命令

```bash
   #运行resque后台任务
   QUEUE=* INTERVAL=1 rake resque:work
   #查看resque任务的执行情况
   resque-web -p 8282
   #重启unicorn
   sudo sv restart unicorn-vagrant
   #查看unicorn日志
   tail -f /etc/service/unicorn-vagrant/log/main/current
   #restart the nginx server
   sudo /etc/init.d/nginx restart
   #restart the postgresql
   #export PGDATA = "/usr/local/var/postgres"
   pg_ctl restart
   for i in `pidof postgres`; do kill -9 $i ; done
```
