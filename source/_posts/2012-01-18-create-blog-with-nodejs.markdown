---
layout: post
title: "用NodeJS创建博客"
date: 2012-01-18 14:07
comments: true
categories:
---

最近，想折腾下nodejs,所以，就想折腾一下nodejs搭建一个blog,现在支持nodejs的主机也还有不少的。比如,[heroku](http://devcenter.heroku.com/articles/node-js)目前就支持nodejs的主机，当然，还有其它的一些了。

#### 如何开始

这次，我们选用 node + express(类似ruby的sinatra框架) + mongoDB(NoSQL数据库). 一般来说，express我们一般用npm(node的包管理)进行安装，只有下面简单的命令就可以了。

```bash
sudo node install express -g
```

#### 实现功能

在这里，我们主要为了实现功能:

* 创建新文章
* 展示所有文章列表

#### 具体步骤

下面，我们就介绍详细的具体步骤。

```
mkdir blog
cd blog
express -c stylus #创建使用jade模版引擎和stylus css引擎的应用
npm install -d  #下载所依赖的包
```


#### 构建对应的ORM,用于处理文章的CRUD操作

首先，我们得构建对应的操作来处理文章的crud操作，代码如下：

```javascript
mongoose.connect(process.env.MONGOLAB_URI || 'mongodb://localhost/blog');
var Article = mongoose.model('Blog', new mongoose.Schema({
    title: String,
    body: String,
  }));
//具体的crud可参照 https://github.com/LearnBoost/mongoose#readme
```

#### 新增文章

首先，我们定义请求/blogs/new页面为新增的页面。 在此页面存在表单元素title和body.我们通过填写对应的内容，然后发送post请求，增加对应的内容。具体代码如下：

```javascript

app.get('/blogs/new',function(req,res){
  res.render('blog_new.jade',{locals: {
        title: "新增文章",
        }
        });
  });

app.post('/blogs/new', function(req, res){
    var article = new Article({title: req.param('title'), body: req.param('body')});
    article.save(
      function( error) {
      console.log("there is a error occured!");
      }
      );
    res.redirect('/')
});
```

### 查看文章

```javascript
app.get('/blogs/:id',function(req,res){
    articles = Article.find({});
    Article.findById(req.params.id, function(err, article) {
    res.render('show.jade',{locals: {
       title: article.title,
      body: article.body,
    }
   });
  })
});
```

#### 参考资料

*  [node jade](http://blog.xebia.com/2011/06/24/getting-started-with-node-js-npm-coffeescript-express-jade-and-redis/)
*  [Blog rolling with mongoDB, express and Node.js](http://howtonode.org/express-mongodb)
*  [mongoose](https://github.com/LearnBoost/mongoose)
