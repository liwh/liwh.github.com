---
layout: post
title: introduce Mac OSX launchd
category: osx
---

这里介绍一下在osx 下如何创建后台服务。比如，创建一些开机自启动的后台服务等等，
在linux 中我们一般用我们一般是创建对应的init.d脚本添加自启动服务。在mac osx中，
我们一般是先创建对应的launchd plist文件.


# introduce

一般，plist 文件主要放在以下几个目录中。我们可以在下面几个目录中找到大量的plist文件。

```bash
  ~/Library/LaunchAgents         Per-user agents provided by the user.
  /Library/LaunchAgents          Per-user agents provided by the administrator.
  /Library/LaunchDaemons         System-wide daemons provided by the administrator.
  /System/Library/LaunchAgents   Per-user agents provided by Mac OS X.
  /System/Library/LaunchDaemons  System-wide daemons provided by Mac OS X.
```

下面是如何将mongodb自启动的plist文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>Label</key>
  <string>org.mongodb.mongod</string>
  <key>ProgramArguments</key>
  <array>
    <string>/usr/local/bin/mongod</string>
    <string>run</string>
    <string>--dbpath</string>
    <string>/usr/local/var/mongodb</string>
    <string>--logpath</string>
    <string>/usr/local/var/log/mongodb.log</string>
  </array>
  <key>RunAtLoad</key>
  <true/>
  <key>KeepAlive</key>
  <true/>
  <key>UserName</key>
  <string>root</string>
  <key>WorkingDirectory</key>
  <string>/usr/local</string>
  <key>StandardErrorPath</key>
  <string>/usr/local/var/log/mongodb/output.log</string>
  <key>StandardOutPath</key>
  <string>/usr/local/var/log/mongodb/output.log</string>
</dict>
</plist>
```

在我们创建完所需要的launchd.plist文件加载呢，我们需使用下面的命令

```bash
  sudo launchctl load /Library/LaunchDaemons/org.mongodb.mongod.plist
  #当我们不需要启动这服务的时候，我们只需要unload that plist文件
  sudo launchctl unload /Library/LaunchDaemons/org.mongodb.mongod.plist
```

# 参考资料

[launchd examples](http://www.devdaily.com/mac-os-x/launchd-examples-launchd-plist-file-examples-mac)
[launchd.plist](http://developer.apple.com/library/mac/#documentation/Darwin/Reference/ManPages/man5/launchd.plist.5.html)
[launchd](http://developer.apple.com/library/mac/#documentation/Darwin/Reference/ManPages/man8/launchd.8.html)
