---
layout: post
title: "How Emacs changed my life"
date: 2012-12-15 19:37
comments: true
categories: [Emacs, Ruby]
---

这篇文章是Ruby之父松本行弘(Matz)先生的演讲稿，介绍了Emacs是如何走进并改变他的生活以及为他开发Ruby提供的各种帮助。

[原文链接](http://www.slideshare.net/yukihiro_matz/how-emacs-changed-my-life) *Fuck GFW first*

>> How Emacs changed my life
>>
>> Yukihiro "Matz" Matsumoto
>> @yukihiro_matz

Emacs 如何改变了我的生活

>> 1980

>> I started programming

我开始了编程

![sharp](https://raw.github.com/tairan/translator/master/source/assets/images/2012-12-15/sharp.png)

>> BASIC

>> 400 steps

在那个上面用BASIC写了400多条操作

>> 1988

>> I met Emacs

>> On Sun-3

1988年，在Sun-3上面，我接触了 Emacs

>> Shared by 200 undergraduates

那个机器由200个本科生共享

>> I tried Emacs

>> But I never used

我尝试了一下Emacs但是并没有使用它

>> Emacs was prohibited

Emacs 是被限制的

>> It consumed too much precious memory

它需要太多的宝贵的内存

>> We are free to download free software

>> We are free to read the source code

我们可以自由的下载免费自由的软件也可以自由的阅读源代码

>> I downloaded Emacs source code

>> and investigated

我下载了Emacs源码并研究它

>> Emacs was my first Lisp interpreter

Emacs 是我第一个Lisp直译器

>> I learned a lot about language implementation from Emacs

我从Emacs那里学到了很多关于语言实现的东西

>> Embadding integers in pointers

在指针里嵌入整数

>> Mark and sweep garbage collection

标记和清扫垃圾收集

>> Calling convention between Lisp and C

在Lisp和C之间相互转换调用

>> I really understood how Lisp work

我确实地了解了Lisp是如何工作的

>> I was fascinated by Lisp objects

我沉迷于Lisp的对象

>> Lisp objects implemented by C

Lisp 对象是由C实现的

>> Then I got a Sparc Station

之后我得到一台Sparc工作站

>> I started to use Emacs

我开始使用Emacs了

>> Emacs become part of me

Emacs 成为了我的一部分

>> If I didn't like anything in Emacs, I could change it

如果我不喜欢Emacs的哪个部分，我可以修改它

>> Emacs is totally configurable

Emacs是完全可配置的

>> Emacs made me realize anything can be changed by a programmer

Emacs让我意识到作为一名程序员可以改变一切

>> It is total freedom

它完全是自由的

>> I could edit without thinking key bindding

我可以在编辑的时候不用考虑各种组合键绑定

>> I didn't want to write anything without Emacs

离开了Emacs我什么都不想写

>> Programs, Documents and Mails

程序，文档以及邮件等

>> so I wrote my own mail client

于是我写了自己的邮件客户端

>> named "cmail"

我给他命名叫 "cmail"

>> in Emacs lisp

>> It was my first non-trivial (Emacs) Lisp program

在 Emacs lisp 中，它是我第一个Lisp程序，意味深长。

>> I used it everyday

我每天都在使用它

>> 1993

>> I started Ruby development

1993， 我开始开发Ruby

>> with influence from Emacs implementation

>> Integers are coded in tagged pointers

>> It uses simple marked sweep garbage collection

>> It uses similar object model to Lisp

受Emacs实现的影响，Intergers are coded in tagged pointers (**Could someone help me to translate it?**)

它使用了简单的标记和清扫垃圾收集

它使用了和lisp相似的对象模型

>> Then I put Smalltalk-like OO system on top

之后我在它上层封装了类似 smalltalk 的面向对象系统

>> For syntax, I wanted Algol/Ada/Eiffel like one

语法方面，从Algol/Ada/Eiffel 吸收了一点

>> But as an Emacs addict, I needed a language model
作为一个对Emacs重度依赖的人来说，我需要一个language-model

>> auto-indent was a must

自动缩进是必须的

>> Back to 1993, there was no auto-indenting language mode for a language with such syntax

回到1993， 还没有一个可以自动缩进的 language-mode 为这样的语法

>> So I tried to write experimental ruby-model.el

所以，我试着写一个 ruby-model.el

>> fighting with emacs lisp and regular expression,

>> for almost whole week

差不多和emacs lisp以及正则表达式奋战了整整一周

>> I somehow succeeded to implement auto-indentation

>> for a languge with "end" delimiters

我成功的实现了一个可以基于 'end' 结尾的自动缩进功能

>> If I couldn't make ruby-model to work

如果我不能让 ruby-model 工作起来

>> The syntax of Ruby would have changed

>> to more C-like one

>> too similar to other scripting languages

Ruby 的语法也许就要改变的像C一样或者类似其他的脚本语言

>> as a result, Ruby would not have gained current popularity

那样的话，Ruby 也许就不会像现在这样流行了

>> Summary

>> 1. Emacs taught me freedom for software
>> 2. Emacs taught me how to read source code
>> 3. Emacs taught me power of Lisp
>> 4. Emacs taught me how to implement a language core
>> 5. Emacs taught me how to implement a garbage collector
>> 6. Emacs helped me to code and debug
>> 7. Emacs helped me to write and edit text/mails/doucments
>> 8. Emacs helped me to be a effective programmer
>> 9. Emacs made me a hacker
>> 10. Emacs has changed my life
>> forever

总结

1. Emacs 告诉我软件的自由
2. Emasc 告诉我如何阅读源代码
3. Emacs 告诉我Lisp的强大之处
4. Emacs 告诉我如何实现一个语言的核心
5. Emacs 告诉我如何实现一个垃圾收集器
6. Emacs 帮助我去写代码和调试代码
7. Emacs 帮助我撰写编辑各种文字，邮件和文档
8. Emacs 帮助我成为一名高效的程序员
9. Emasc 塑造我成为一名黑客
10. Emacs 永远的改变了我的生活

>> Thank you

谢谢
