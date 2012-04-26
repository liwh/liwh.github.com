---
layout: post
title: Rake Tutorial
category: other
---

我们在使用rails的过程中，经常都会用到rake,比如说执行rails数据库脚本的时候，
我们用rake db:migrate等等。这里介绍下如何在rails中使用rake任务。

## 关于rake

rake是一个构建任务的工具。rake任务可以完全用ruby代码写。具体可参见: [jimweirich](https://github.com/jimweirich/rake)

## rake的用法

这里我们定义一个shell任务，生成一个文件，并向文件中写入helloworld.以rakefile的名字把下面代码保存在任意目录下。

```ruby
directory "tmp"

file "hello.tmp" => "tmp" do
sh "echo 'Hello' >> 'tmp/hello.tmp'"
end
```

这样我们执行它就会产生下面的效果

```ruby
liweihui@desktop:~$ rake hello.tmp
(in /home/liweihui)
mkdir -p tmp
echo 'Hello' >> 'tmp/hello.tmp'
```

## 利用rake写rails任务

在rails中，一般把rake 任务放在lib/tasks目录下，但必须以.rake的后缀保存,用于编写项目中一些自动化的功能.比如：我们要输出当前环境下数据库结构的当前版本。

```ruby
namespace :do do
  desc "print the migration version"
  task :schema_version => :environment do
  puts ActiveRecord::Base.connection.select_value('select version from schema_info')
  end
end
```

我们执行在terminal中执行 rake db:schema_version 就能输出版本结果了。其中namespace是用来定义命名空间的，我们可以将多个类似的任务放在一个namespace中，这也就是将任务进行分组.
另外，我们还可以获取termial中的参数，来定义rake任务中变量的值。比如，我们执行rake db:rollback step=...就是这样的。

```ruby
desc "Make coffee"
task :make_coffee do
  cups = ENV["COFFEE_CUPS"] || 2
  puts "Made #{cups} cups of coffee. Shakes are gone."
end
```

这里我们就可以执行这样的命令rake ... COFFEE_CUPS = 3 。

## 相关资料

[Rake document](http://docs.rubyrake.org/)

[Rake tutorila](http://jasonseifer.com/2010/04/06/rake-tutorial)

[Custom Rake Tasks](http://railscasts.com/episodes/66-custom-rake-tasks)




