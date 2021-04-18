---
title: Vim
toc: true
thumbnail: /gallery/thumbnails/star.jpg
widgets:
  - type: profile
    position: left
    author: 博雅
    author_title: zju小硕一枚
    location: 'Hangzhou,China'
    avatar: /images/Golden_Medal_-1_Icon.svg
    gravatar: null
    avatar_rounded: false
    follow_link: 'https://github.com/MasterXuBoya'
    social_links:
      Github:
        icon: fab fa-github
        url: 'https://github.com/MasterXuBoya'
      E-Mail:
        icon: fa fa-envelope
        url: xuboya@zju.edu.cn
      Facebook:
        icon: fab fa-facebook
        url: 'https://facebook.com'
      Twitter:
        icon: fab fa-twitter
        url: 'https://twitter.com'
      RSS:
        icon: fas fa-rss
        url: /
  - type: category
    position: left
  - type: toc
    position: left
date: 2020-03-18 23:04:54
tags:
  - Vim
  - Linux
categories:
  - 工具
  - Vim
---
## 为什么使用Vim
> 程序员编程不同于英语写作，大部分时间可能都在看代码和修改代码，一直在编辑的时间比较少，因此引入Vim工具。Vim本身通过character命令的方式进行文本编辑，当熟练之后可以大大提高程序编写速度。

<!--more -->

![Vim-cheat-sheet](Vim/vi-vim-cheat-sheet-sch.gif)
## Mode

> 存在几种基本模式：Normal Mode,Insert Mode,Command-line Mode,Viusal Mode 

+ Normal Mode-->Insert Mode：i    Back to Normal Mode:ESC
+ Normal Mode-->Command-line Mode: :   Back to Normal Mode:ESC
+ Noraml Mode-->Viusal Mode: v    Back to Normal Mode:ESC
+ Replace Mode

---
## Movement-光标移动

> Movements in Vim are also called “nouns”, because they refer to chunks of text.

+ Basic movement: hjkl (left, down, up, right)
+ Words: w (next word), b (beginning of word), e (end of word)
+ Lines: 0 (beginning of line), ^ (first non-blank character), $ (end of line)
+ Screen: H (top of screen), M (middle of screen), L (bottom of screen)
+ Scroll: Ctrl-u (up), Ctrl-d (down)
+ File: gg (beginning of file), G (end of file)
+ Line numbers: :{number}<CR> or {number}G (line {number})
+ Misc: % (corresponding item)
+ Find: f{character}, t{character}, F{character}, T{character}
    find/to forward/backward {character} on the current line
    , / ; for navigating matches
+ Search: /{regex}, n / N for navigating matches


h/j/k/l:上下左右移动

nj:光标向下移动多行

0：行首  ="Home" Key

$：行尾  ="End"  Key

Ctrl+f ="Page Down" Key

Ctrl+b ="Page Up" Key

e:下一个单次结尾字符

b：与e相反

w:跳转到下一个单次

o:open在当前行下面添加一行

---

## Viusal Mode

v:Viusal Mode        普通选中

V:                   选中多行

Ctrl+v:Visual Block  选中一个矩形区域

---
## Edits
>  Vim’s editing commands are also called “verbs”, because verbs act on nouns.

+ i enter insert mode
    but for manipulating/deleting text, want to use something more than backspace
+ o / O insert line below / above
+ d{motion} delete {motion}
    e.g. dw is delete word, d$ is delete to end of line, d0 is delete to beginning of line
+ c{motion} change {motion}
    e.g. cw is change word
    like d{motion} followed by i
+ x delete character (equal do dl)
+ s substitute character (equal to xi)
+ visual mode + manipulation
    select text, d to delete it or c to change it
+ u to undo, <C-r> to redo
+ y to copy / “yank” (some other commands like d also copy)
+ p to paste
Lots more to learn: e.g. ~ flips the case of a character

### 删除
x:删除当前字符
dw:删除当前单词
dnw:删除n个单词

dd:删除光标所在行
ndd:删除光标以下n行
d1G:删除第一行到本行所有内容
dG:删除本行到结尾所有内容

d0:删除光标到本行开始内容
d$:删除光标到本行结束的位置

选中块删除:visual模式

### 复制

yy:
nyy:
y1G:
yG:

y0:
yG:

选中块复制：进入Visual模式后  y
### 粘贴
p

### 剪切
cj:剪切一行
cw:剪切一个单词

选中块剪切:Visual Mode

### 查找
/word:向下查找
?word:向上查找
n:下一个
N:上一个

### 替换(Command Mode)

:%s/word1/word2/g  全文替换
:n1,n2s/wordl/word2/g  n1到n2行间查找替换

### 撤销

u:Undo
ctrl+r: Redo

### Counts

> You can combine nouns and verbs with a count, which will perform a given action a number of times.

+ 3w move 3 words forward
+ 5j move 5 lines down
+ 7dw delete 7 words


### Modifiers(括号、引号等)

> Some modifiers are i, which means “inner” or “inside”, and a, which means “around”.

d%:删除括号内所有内容
ci[ ：剪切[]内容
ci( : 剪切()内容
da':删除''内所用内容,包括引号本身

[hello world]
(open a window hello)

### Reference
[MIT Vim Lessons](https://missing.csail.mit.edu/2020/editors/)

---


Add MasterXuBoya's file

