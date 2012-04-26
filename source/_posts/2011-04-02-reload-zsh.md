---
layout: post
title: reload zsh
category: other
---

由于zsh相对于bash来说，有些更好的功能。比如说补全功能等。但是，有个问题，
在我们安装完 某个库，或者新增什么bin的时候，我们等新开terminal，才能重新
加载它。这里，我们在zshrc里，定义 一个reload函数，这样，我们在新增什么函数，
或者bin的时候，我们就不需要重启terminal，就能加载它了

```bash
function reload() {
 if [[ "$#*" -eq 0 ]]; then
   test -r /etc/zsh/zsh-oli && . /etc/zsh/zsh-oli
   test -r ~/.zshrc && . ~/.zshrc
   return 0
 else
   local fn
   for fn in $*; do
     unfunction $fn
     autoload -U $fn
     done
   fi
}
```
