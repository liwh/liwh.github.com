---
layout: post
title: 使用pow作为rails server
category: other
---

[Pow](https://github.com/37signals/pow) 是信号公司一个开源的rack-server,官方版本只支持Mac OS平台,它的主要有点是零配置，我们可以很方便的
访问我们的app.

```bash
  #安装直接使用下面这句就可以了
  curl get.pow.cx|sh
  #只要下面简单的一句话，我们就可以将我们的app利用pow去运行了
  #这样我们就能通过http://shop.dev去访问我们的项目了
  cd ~/.pow && ln -s ~/workplace/shop
```

当我们使用rvm管理ruby版本的时候，我们需要在我们的项目中添加.rvmrc文件。
[具体参见](http://beginrescueend.com/workflow/rvmrc/) ，然后，创建对应的gemset之后，
我们就可以访问了。

Pow默认的日志文件是保存在~/Library/Logs/Pow/apps/#{app}.log 中的，app对应相应的程序。

为了更方便的使用pow,我们可以使用 [powder](https://github.com/Rodreegez/powder) 来操作pow.

