---
layout: post
title: "构建cocos2d开发环境"
date: 2012-01-08 14:26
comments: true
categories: iOS
---

cocos2d是一个开源框架，用于构建2D游戏、演示程序和其他图形界面交互应用等。参见[百科](http://baike.baidu.com/view/3800461.html?fromTaglist)

* 首先下载相应的[文件](http://www.cocos2d-iphone.org/download),最新的代码可以在github上[查看](https://github.com/cocos2d/cocos2d-iphone)

* 然后，到对应的目录，在termianl下，执行下面命令

```bash
 ./install-templates.sh -u -f 
```

* 然后，在我们创建工程的模版中，我们就嫩看见对应的模版了。如下图所示：
![image](/images/cocos2d.png)

* 最后，我们点xcode的运行，就可以看到如下效果了
![image](/images/hello_world.png)