---
layout: post
title: Textmate快捷键
category: textmate
---

这里介绍一些textmate的常用快捷键。

## 光标移动

textmate默认的光标移动是类似emacs的。如果你习惯了vim的形式的光标移动，可以安装 [vimode插件](http://sourceforge.net/projects/tm-vi-plugin/)

c-f : 向右移动光标
c-b : 向左移动光标
c-n : 向下移动光标
c-p : 向上移动光标
command－← :光标移动到行首(c-a)
command－→ :光标移动到行尾(c-e)
command－↑ :光标移动到文件头
command+数字 :tab间移动
command－↓ :光标移动到文件尾
c － ← :移动一个单词向左
c－→ : 向右移动一个单词

## 编辑

ctrl + t :调换选中文本顺序，如果没有选中任何文本则对换光标左右字符位置
ctrl + o :不移动光标同时插入新的一行
ctrl + y :复制出刚被删除的文本
ctrl + k :删除光标到行尾的所有字符
ctrl + d :删除光标右侧一个字符
ctrl + w:选中当前单词
shift + 左 :移动一次向左选定一个字符
shift + 右 :移动一次向右选定一个字符
ctrl + command + 左 :移动选中字符向左移动一个字符
ctrl + command + 右 :移动选中字符向右移动一个字符
shift + command + f :搜索当前项目
shift + command + l :选中当前行
shift + command + 左 :选中光标到行首
shift + command + 右 :选中光标到行尾

## 查找移动

ctrl-s :查找匹配的字符

shift+command+t :通过symbol查找,在ruby中，只查找class/method 定义。

## 行移动

command-L :移动到指定行


## 常用命令

command + t : 查找文件
ctrl + ⌘  + 上下键：移动整行
ctrl + shift + k： 删除整行
ctrl + shift +d：复制整行
ctrl + shift + T :查看todo
command + ] : 增加text indent
command + [ : 删除text indent
command + shift + 方向键 :选中
esc :自动补完，补完顺序视变量数量而定
shift + ctrl + option + x : x为你想切换的语言的首字母，比如Python则是P，HTML则是H
opt + cmd + . :自动增加html close tag

## 参考资料

[textmate and vim](http://www.hazaah.com/programming/textmate-and-vim/)
快捷键: <a href="/images/textmate_shortcuts_1920.png"><img src="/images/textmate_shortcuts_1920.png" width="300" height="200" alt="" /></href>






