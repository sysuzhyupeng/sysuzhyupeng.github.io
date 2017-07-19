---
layout: post
title: 编程语言实现模式(1)
description: 编程语言实现模式.
category: blog
---

语言读取
-
  语言读取是这样一个过程：文件读取部分对输入内容进行“识别”，并输出数据结构作为中间表示(intermediate representation，IR)，供其他部件使用。流水线的末端是生成器，会根据IR及之前收集信息进行计算，并输出最终结果。
  
  把输入内容用IR的形式放到内存以后，就可以从中抽取信息或者修改它，第一步要做的就是`遍历IR`。要想输出有用的输出结果，就必须`分析输入内容`，对于一个字符x，我们需要知道它是一个变量，还是一个方法，要知道它的类型以及定义位置。
  
识别式子结构
-
  例如，`return x+1`这个由词法单元组成的小式子就是表达式。我们可以把x+1看做一个`expr`，整个return语句归类为一个`returnstat`，同时语句也是一个`stat`。将其翻转，就能得到解析树。
  
构建递归下降语法解析器
-
  解析器用来解析识别的解析树，解析器不必构造具体的解析树，因为只要为解析中的指定子结构(树的内节点)编写专用的函数，就能得到解析树的信息。
```javascript
  //对于return x + 1
  void stat(){ returnstat() };
  void returnstat(){ match("return"); expr(); match(";"); }
  void expr(){ match("x");match("+");match("1"); }
```
