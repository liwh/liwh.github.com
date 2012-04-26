---
layout: post
title: Unix Shell -- zsh
category: other
---

最近，看了下unix下的shell,发现品种还是挺多的，如系统默认的bash,还有csh等等，
但是 个人觉得现在最好用的还是zsh,由于zsh具有很强大的自动补全和纠错功能，还有高度可定制性及
扩张 等特性， 被称作“the last shell you'll ever need”.这里，说说它的安装和使用，还有一些简单的特性。

## 安装

 相比原生的zsh,个人觉得还是oh-my-zsh好用些，毕竟这里对zsh做了一些很好的定制，
 具体安装过程可以参见 [安装过程](https://github.com/robbyrussell/oh-my-zsh)
 其中，注意切换默认用户的shell命令为

```bash
  chsh  -s /bin/zsh
```

## binding keys

 我们可以加入一些快捷键，例如，在bash里面，我们通常用c-a和c-e来返回当前行的行首和行尾，还有查找
 使用过的命令c-r.这些，我们在zsh当然同样也可以定制。我们可以通过bindkey查看所有的zsh keybord shortcuts

```ruby
  bindkey '^A' beginning-of-line
  bindkey '^E' end-of-line
  bindkey "^R" history-incremental-search-backward
```

## 主题

 在oh-my-zsh中，有很多主题。具体的可以参见 [zsh-theme](https://github.com/robbyrussell/oh-my-zsh/wiki/themes)
 同样，你也可以定制自己喜欢的主题。

## 注意

 这里要注意，当我们使用zsh的时候，得注意讲.bash_profile中的一些命令，复制到.zshrc中，最主要的是，当你使用
 rvm做你的ruby version manager的时候，一定要将下面这句话放在.zshrc中。

```bash
  #auto start the rvm shell on terminal
  if [[ -s /Users/Apple/.rvm/scripts/rvm ]] ; then source /Users/Apple/.rvm/scripts/rvm ; fi
```

## 参考资料

 [zshwiki](https://wiki.archlinux.org/index.php/Zsh)
