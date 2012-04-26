---
layout: post
title: 更新用户信息不提供密码(devise)
category: other
---

当我们使用devise的时候，我们要更新用户信息的时候，比如说，我们在form中提供了password 和password_confirmation的输入框,
但是，我们希望更新用户信息的时候，如果不输入密码的时候，则不校验password.
这时我们只需要将下面两句加在user.save的前面就行了。

```ruby
  params[:user].delete(:password) if params[:user][:password].blank?
  params[:user].delete(:password_confirmation) if params[:user][:password_confirmation].blank? && params[:user][:password].blank?
```


