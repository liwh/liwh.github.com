---
layout: post
title: rails3中邮件服务设置
category: other
---

对于一个web服务，邮件提醒是必须的，比如，发送给用户注册信息，密码重置信息等等，
都需要通过邮件来提醒用户。这里简单介绍一下rails3的邮件服务设置。

首先，我们得创建mailer

{% highlight bash %}
  rails g mailer UserMailer
{% endhighlight%}

接着，我们编辑app/mailer/user_mailer

```ruby
UserMailer < ActionMailer::Base
 default :from => "xxx@gmail.com"

   def welcome
     mail(to: "xxx@gmail.com",body:"aaa", subject: "welcome")
   end
 end
```

下一步，就是配置action mailer了,由于国内的gmail连接老出现问题，所以这里就介绍163的了，
基本都差不多

```ruby
 ActionMailer::Base.smtp_settings = {
   :address              => "smtp.163.com",
   :port                 =>  25,
   :domain               => "domail.com",
   :user_name            => "xxx@163.com",
   :password             => "xxx"
   :authentication       => "plain",
   #if got the same error with me,please change the options false
   :enable_starttls_auto => true
  }
ActionMailer::Base.default_url_options[:host] = "localhost:3000"

```

注意:如果你和我出现了同样的[堆栈错误](https://gist.github.com/1021285)

这样，我们在rails console中执行 UserMailer.welcome.deliver就能发送邮件到指定的
邮箱了。

## 参考资料

[mail guides](http://guides.rubyonrails.org/action_mailer_basics.html)
[action mailer in rails 3](http://asciicasts.com/episodes/206-action-mailer-in-rails-3)
