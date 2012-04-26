---
layout: post
title: git使用
category: git
---

git是现在最流行的版本的版本控制系统了,并且它是免费而且开源的。并且它支持多分支，我们可以很轻易
的创建分支和合并分支。现在越来越多的项目正在用git,如eclipse rails android.. 除此之外，现在常用
的版本控制系统还有cvs,svn,vss等等。。

## git quick start

 例如，当我们向github平台提交我们的代码时，我们可以执行以下代码
{% highlight bash %}
  git add .
  git commit -m 'first commit'
  #将本地本件提交到github server的master brach
  git push this master
{% endhighlight %}
这样，我们的代码就提交到github server了，我们在github的project中就能清楚看到我们提交的代码.

## git command list

```bash

  # 显示当前代码库的状态，增加，修改，删除了什么文件，这样我们可以很清楚的看到，我们对代码的改动.
  git status

  # 显示代码的改动细节，方便我们对代码进行对比。知道改动了什么。 后面的options color是为了方便利用颜色的对比清晰的看到改动
  git diff --color

  #查看git的忽略文件（即上传的时候不提交的文件，这个可以通过正则去匹配）
  cat .gitignore

```

## step by step learning

### 用户配置

```bash
  # 设置用户
  git config --global user.name "liwh"
  git config --global user.email "liwh87@gmail.com"
  #设置默认的提交的仓库，这样，修改的代码会默认提交到此仓库
  git config --global push.default "example"
  #查看设置的git变量
  git config --list
```

### 高亮显示

```bash
  #在控制台高亮显示信息
  git config --global color.status auto
  git config --global color.branch auto
```

### 创建仓库，增加和提交

```bash
  #初始化本地仓库，讲当前目录下文件，加入git的管理
  git init
  #将所有的文件和目录加入git控制
  git add .
  #提交文件到本地的仓库
  git commit -m 'initial commit'
  #查看日志文件
  git log
```

### 查看不同和提交更改

```bash
  #查看不同
  git diff --color
  #提交更改
  git commit -m -a 'first commit'
```

### 增加remote

```bash
  #add ../remote-repository.git with the name origin
  git remote add origin ../remote-repository.git
  git commit -a -m 'this is the first commit the remote repository'
  git push origin
```

### 创建分支

```bash
  #add branch
  git branch b
  #go to the branch b
  git checkout b
  #delete branch b
  git branch -d b
```

### 合并分支

```bash
  #将b分支合并到当前分支
  git merge b
  #选择某一个分支中的一个或几个commit来进行合并
  git cherry-pick commit-id
  #还有就是通过git rebase 来合并，它会根据时间顺序来进行一个一个的合并
  git rebase  b
  #处理冲突继续
  git rebase --continu
  #取消合并
  git rebase -- abort
  #更新本地分支的远程repo的url
  git remote set-url remote_name new_url
  #合并多个commit,可见 http://stackoverflow.com/questions/4936778/git-merge-commits
  git rebase -i HEAD~2  #然后对文件进行编译，可达到预定的效果
```

### 储存

```bash
  #储存变更
  git stash
  #查看储存列表
  git stash list
  #还原最近一次仓库里的代码
  git stash apply
  #指定恢复的代码
  git stash apply stash@{2}
```

### Tag

```
  #增加tag
  git tag -a v0.5 -m “Version 0.5 Stable”
  #删除tag
  git tag -d v0.0.1
  #github add tag
  git push origin tagname
  #remove tag
  git push origin :tagname
  #提交本地的最近的tag
  git push origin --tags
```
