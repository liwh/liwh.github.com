---
layout: post
title: 后台管理框架active_admin
category: other
---

## 简介

基于rails的管理框架还是很多的，例如,rails_admin, active_admin等。
这里，介绍下最近使用的 [active_admin](https://github.com/gregbell/active_admin)

## 安装

在gemfile中添加

```bash
  gem 'activeadmin'
  gem 'sass-rails' #only in rails 3.1
```

装完对应的gem之后，执行下面task

```bash
   rails generate active_admin:install
```

这里会创建对应的文件，主要是在routes中添加对应的路由，和active_admin的设置文件，还有，就是
创建了对应的active_admin model,和对应的database migrate,在migrate中，创建了初始化的active_admin
的记录admin@example.com,我们可以通过这个登录后台管理的页面。当然，在执行数据迁徙前，我们可以将
这个改成我们所希望创建的默认用户。

## 使用

active_admin 创建的文件都放在app/admin这个目录下面。我们要想自定义首页显示的内容，可以查看dashboard文件，
更改里面的内容就能直观的显示在首页上了。 如果，我们原来使用的控制器，定义了module Admin里，我们可以通过更改
config/initializers/active_admin.rb这里面的配置，将config.default_namespace = :admin,换成我们所想要的，防止冲突。

当我们要对某个表进行管理时，我们此时就可以通过创建对应的active_admin resource,执行下面命令，就可以创建对应的ActiveAdmin model了.
执行完这个之后，在我们的后台中，会自动创建对应的增删改查功能。同样，我们可以自定义显示它，具体可以查看 [资料](http://activeadmin.info/docs/3-index-pages/index-as-table.html)

## 参考资料

[github gem](https://github.com/gregbell/active_admin.git)
[demo](http://demo.activeadmin.info/admin)
[doc](http://rubydoc.info/github/gregbell/active_admin/master/frames)
[info](http://activeadmin.info/)

