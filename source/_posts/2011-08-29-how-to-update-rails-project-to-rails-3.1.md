---
layout: post
title: 将rails项目升级到rails 3.1
category: other
---

有很久都没更新博客了，最近，发现很多东西不记下来，特别容易忘记。最近把项目从rails 3.0迁移
到了3.1版本，rails 3.1增加了许多新的特性，比如说：默认支持coffee-script,把jQuery作为默认的js库
等等，还有一些新特性,可以查看 [rails 3.1 release note](http://guides.rubyonrails.org/3_1_release_notes.html) .

## 更新gem

比如说把原来的meta_where替换成squeel等等，基本上，现在版本的gem都支持rails 3.1了， 另外，原来的用于编译
coffeescript的barista可以去掉了，因为默认rails模版已经支持编译coffeescript了。以前，默认的情况下，我们设置
base=true这个配置，用来不让

```javascript
  (function() {
    ...
    }).call(this);
```

包围js代码，这样很多js变量就能全局性的用了。但是，现在rails ,默认是将bare设置为false的，这个可以看 [rails的源码](https://github.com/rails/rails/issues/1125)  .在这种情况下，我们可以通过设置gloabla 变量，如window.xxx = xxx .
这里有 [一些讨论](http://stackoverflow.com/questions/5211638/pattern-for-coffeescript-modules/5212449#5212449) ,
另外还有就是active_hash这个gem也做了一些改变， 当我们要实现activerecord和activehash对象关联时，得将原来的代码改写成：

```ruby
class Country < ActiveHash::Base
end

class Person < ActiveRecord::Base
  extend ActiveHash::Associations::ActiveRecordExtensions
  belongs_to_active_hash :country
end
```

## sprockets , asset pipline

这个应该是rails 3.1最主要的更新内容了。用asset pipline 替换了原有的？形式的引入文件， 利用的是 [sprockets gem](https://github.com/sstephenson/sprockets)  .这个具体可以看 [文章](http://guides.rubyonrails.org/asset_pipeline.html) ，这里面详细介绍了，用最新hash方式缓存静态文件。

## subdomain, domain

现在rails url helper已经默认支持subdomain了，我们可以使用xxx_url(subdomian: xxx)

## Http Steaming

HTTP Streaming是Rails 3.1中一项新改进，可以让浏览器在页面作出响应的同时下载样式表和JavaScript文件。该特性需要Ruby 1.9.2，以及Web服务器的支持，幸运的是流行的nginx和unicorn组合已经支持。

## 一些方法

在rails 3.1 的更新日志中，你会发现,clone方法发生了改变，我们以前用clone方法复制model,会是以下这种情况

```ruby
  p = Product.first.clone
  p.new_record? # => true
```

而现在clone 之后的对象调用new_record?会返回false,即这是一种浅赋值，以下是changelog内容

<pre>
1. ActiveRecord::Base#dup and ActiveRecord::Base#clone semantics have changed to closer match normal Ruby dup and clone semantics.

1. Calling ActiveRecord::Base#clone will result in a shallow copy of the record, including copying the frozen state. No callbacks will be called.

1. Calling ActiveRecord::Base#dup will duplicate the record, including calling after initialize hooks. Frozen state will not be copied, and all associations will be cleared. A duped record will return true for new_record?, have a nil id field, and is saveable.
</pre>

另外，现在在query的时候，我们不能写以前的那种关联方式查询了，例如下面例子

```ruby
   Shop.where(user: User.first)
   #上面的话得换成
   Shop.where(user_id: User.first)
```

还有，现在通过association.new创建对象,等于以前的build方式创建对象了。
