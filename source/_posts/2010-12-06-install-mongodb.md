---
layout: post
title: 安装MongoDB
category: MongoDB
---

最近，安装了下NoSql的MongoDB数据库,这是一个基于
分布式文件存储的数据库开源项目，由C++ 语言编写。
旨在为WEB应用提供可护展的高性能数据存储解决方案。
常用于高流量网站，在线游戏网站和搜索引擎的大规模数据管理和分类。
另外，在rails中，它也存在对应的ORM框架 [Mongoid](https://github.com/mongoid/mongoid) ，可用来替代Rails自带的ActiveRecord。
现在最新版本是1.6,[下载地址](http://www.mongodb.org/downloads)
下面介绍下在OS X下的安装步骤：

## 安装homebrew

现在,在os x下比较流行的包管理是用homebrew,替代了MacPorts.
具体步骤见:[安装homebrew](http://mxcl.github.com/homebrew/)

## 安装MongoDB

* sudo brew install mongodb
* sudo cp /usr/local/Cellar/mongodb/1.6.XXXXX/org.mongodb.mongod.plist  /Library/LaunchDaemons/
* sudo launchctl load /Library/LaunchDaemons/org.mongodb.mongod.plist

## 参考资料

[How to install MongoDB on OS X](http://shiftcommathree.com/articles/how-to-install-mongodb-on-os-x)

## MongoDB学习资料

[MongoDB入门学习资料](http://www.javaeye.com/news/16677-mongodb)
[Details for mongodb](http://www.markus-gattol.name/ws/mongodb.html)
