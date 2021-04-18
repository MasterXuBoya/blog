---
title: MarkDown基本语法
date: 2020-02-21 21:39:51
toc: true
thumbnail: /gallery/thumbnails/white-and-brown-house.jpg
tags: 
    - Markdown
    - Hexo
categories: 
    - 编程语言
    - Markdown
mathjax: true #开启Latex公式解析
# related_posts : true #相关文章推荐
---

# 标题&字体
## 标题
    # 一级标题
    ## 二级标题
<!-- more -->
    ### 三级标题
    #### 四级标题

<center><font color=#3da742 size=5>一、标题</font></center></br>
<span>html标签</span>

## 字体


### 🌟 New Features
<font color=#FF0000 >红色</font>
*斜体字*
**粗体字**
***斜体加粗***
~~删除线~~

#  列表
+ 无序列表(+-*)
1. 有序列表(1.)

+ 列表嵌套
    + 列表嵌套
        1. 列表嵌套

#  代码
```
$ ls
$ mkdir blog
$ hexo server
```
```C++
#include <iostream>
#include <stdio.h>
using namespace std;
int main(){
    cout<<"hello Hexo"<<endl;
    return 0;
}
```
```python
import numpy as np
import pandas as pd
def func(argc,arg):
    a=input()
    return a
```
# 超链接
> 两种基本形式
+ <http://www.baidu.com>
+ [百度](http://www.baidu.com)

# 图片
![风景图片](hexo MarkDown基本语法/example.jpg "示例图片")

# 表格

|表头1|表头2|表头3|
|:-|:-:|-:|
|内容11|内容12|13|
|内容21|内容22|23|

| 表头1|表头2|表头3|表头4
|-| :- | :-: | -: |
|默认左对齐|左对齐|居中对其|右对齐|
|默认左对齐|左对齐|居中对其|右对齐|
|默认左对齐|左对齐|居中对其|右对齐|

# 公式

$y=x+1$

$ \Gamma(z) = \int_0^\infty t^{z-1}e^{-t}dt. $

$$\Gamma(z) = \int_0^\infty t^{z-1}e^{-t}dt.$$


# 文件下载

