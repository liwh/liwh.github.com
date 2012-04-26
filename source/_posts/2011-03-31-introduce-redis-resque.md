---
layout: post
title: introduce the resque based on redis
category: other
---

在项目中，很多时候，我们的代码都是从上到下依次执行的，就是说，
得等到上面的任务完成之后，下面的任务才能执行， 但是，很多时候，
当用户提交一个很耗时间的请求的时候，这时候，为了让用户不需要等待，
去做别的事情，我们就得把 这些耗时间的任务放在后台执行了。用于后台
任务执行的插件，在ruby中，是有不少数的，具体的，可以看下这篇文章
[introduceing resque](https://github.com/blog/542-introducing-resque) ,这里面介绍了几个后台任务执行的插件和
resque的对比，resque是用于github平台的任务插件。

Resque是一个基于redis的后台任务的Ruby库，将任务放在redis的多个队列中，并且待会执行它们。

# 安装

首先，由于它是基于redis的，所以，得先安装redis，resque则已gem的形式安装
{% highlight shell %}
  #install redis on osx
  sudo brew install redis
  gem install redis redis-namespace yajl-ruby vegas sinatra
{% endhighlight %}


# Resque的使用

在resque中，我们一般把所要执行的任务放在一个perform的方法中，同时，得为该class或module定义一个实例参数@queue,这个实例
参数，用来代表任务队列。并且，你可以任务命名它。下面，是一个简单的例子.

```ruby
class Archive
  @queue = :file_serve

  def self.perform(repo_id, branch = 'master')
    repo = Repository.find(repo_id)
    repo.create_archive(branch)
  end
end
```

这里，我们就将Archive的任务，放在了名叫file_serve的队列中了.如果，要将这个任务放在别的类中，我们需要调用Resque的enqueue方法，如下

```ruby
  class Repository
    def async_create_archive(branch)
      Resque.enqueue(Archive,self.id,branch)
    end
  end
```

这样，我们就可以调用repo.async_create_archive('master')这样的方法在我们的程序中，执行这个，它就会创造一个job，并且将它放在file_serve的队列中。
下面方法是用来执行file_serve队列上的任务的。

```ruby
  QUEUE=file_serve rake resuqe:work
  #我们也可以用*来代替file_serve,
  #用*表示运行所用的QUEUE
```

#  Resque Jobs

resque的jobs是以JSON对象的形式持久存在的。所以，perform方法里面的参数必须能用JSON 编码，例如，我们创建一个job

```ruby
  repo = Repository.find(4)
  repo.async_create_archive('master')
```

执行这个之后，就会有下面形式的JSON对象存在于file_server队列中:

```javascript
  {
    'class':'Archive',
    'args' : [4,'master']
  }
```

所以，jobs只接受能被JSON化的参数。

# Workers

Resque workers是一直执行的rake任务，它就像下面这样

```ruby
  start
  loop do
    if job = reserve
     job.process
    else
      sleep 5
    end
  end
  shutdown
```

如果，我们需要把resque以插件的形式安装在rails application中的时候，因为,它是不知道你的application环境的，所以，我们得先加载application 到内存
中,所以，我们得这样。

```ruby
  #lib/tasks/resque.rake or Rakefile
  require 'resque/tasks'

  task "resque:setup" => :environment
```

resque还支持对队列的优先级的处理，比如，这里有另外一个队列，warm_cache,我们利用下面的形式启动worker.

```bash
  QUEUES=file_serve,warm_cache rake resque:work
```

它就会首先，找file_serve队列中有没有相应的队列任务，如果没有了，它才会去处理warm_cache里面的jobs.

同时执行多个队列任务

```bash
  COUNT=5 QUEUE=* rake resque:workers
  #这个会启动五个相应的resque workers,每个都在一个独立的线程中。
```

# Front End

另外，我们也可以很方便的通过web的形式查看队列任务的执行情况

```bash
  resque-web -p 8282
  #或者加namespace
  resque-web -p 8282 -N myapp
```

# 整合rails

首先配置redis

```ruby
defaults: &defaults
  host: localhost
    port: 6379

  development:
      <<: *defaults

  test:
      <<: *defaults

  staging:
      <<: *defaults

  production:
      <<: *defaults
```

初始化config/initializers/resuqe.rb

```ruby
  rails_root = ENV['RAILS_ROOT'] || File.dirname(__FILE__) + '/../..'
  rails_env = ENV['RAILS_ENV'] || 'development'

  resque_config = YAML.load_file(rails_root + '/config/resque.yml')
  Resque.redis = resque_config[rails_env]
```

# 测试

在进行spec测试的时候，我们需要将resque任务立即执行，这时候，我们需要使用 [resquespec](https://github.com/leshill/resque_spec)  这个gem
包来完成，我们的工作，让resque_worker立即执行。主要用到的方法是with_resque();
