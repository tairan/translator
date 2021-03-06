---
layout: post
title: "Rake Tutorial"
date: 2012-12-21 20:54
comments: true
categories: [Ruby, Rake]
---

[Rake Tutorial](http://docs.rubyrake.org/tutorial/index.html)

## Rake 指南 ##

Rake is a build tool, written in Ruby, using Ruby as a build language. Rake is similar to make in scope and purpose. Rake a simple ruby build program with capabilities similar to make.

Rake 是用Ruby写的Build工具，使用Ruby作为Build语言。Rake的目标和适用范围和make类似。Rake是一个简单的Ruby程序和make具有相似的功能。

Rake has the following features:

Rake 具有以下特征

- Rakefiles (rake’s version of Makefiles) are completely defined in standard Ruby syntax. No XML files to edit. No quirky Makefile syntax to worry about (is that a tab or a space?) 
	
	Rakefiles (Rake版本的Makefiles) 完全使用的标准的Ruby语法定义。不用编辑xml文件。没有Makefile的诡异语法问题比如(Tab和空格)

- Users can specify tasks with prerequisites. 

	用户可以指定执行某个预设的任务

- Rake supports rule patterns to sythesize implicit tasks.

	Rake 支持规则模式查找任务

- Rake is lightweight. It can be distributed with other projects as a single file. Projects that depend upon rake do not require that rake be installed on target systems.

	Rake 是轻量级的。它可以作为单个文件分发给其他项目。那些依赖于rake的项目并不强制要求rake安装在目标系统上。

**Copyright**

Copyright © 2005, by Jim Weirich, Some rights reserved.


### Chapter 1—Introducing Rake ###

章节 1 介绍Rake

#### Getting Started ####

起始

**Received via EMail:**

收到一些EMail

>> I have just started using the excellent Rake tool (thanks, Jim!) and I am at a bit of a loss on how to proceed. I am attempting to 	create unit test for some C++ code I am creating, [...]

>> 我已经开始使用这个Rake工具(感谢Jim), 我花费很多时间去用它。我试图为我的一些C++代码创建unite test...

Several people recently have made similar comments, they really like rake, but have had trouble getting started. Although the Rake documentation is fairly complete, it really does assume you are familiar with other build tools such as ant and make. It is not really material for the newbie.

最近有很多人都提到类似的意见，他们真的很喜欢rake，但是他们不知道怎么上手。尽管Rake的文档已经很全面了，但它真的是基于你已经接触过Build家族的其他工具，比如ant和make。所以它也确实不太适合新手。

This tutorial is an attempt to address this lack. We will start with a very simple problem: Building a Simple C Program.

此指南正是弥补这个不足。我们会从一个简单的问题开始：Building 一个简单的C程序

**The Problem**

We will start with a very simple build problem, the type of problem that make (and now rake) was desiged to deal with.

我们从一个非常简单的build问题入手，这类问题正是make(现在是rake)设计的目的。

Suppose I have a very simple C program consisting of the following files.

比如我有一个非常简单的C程序，有以下一些文件

{% codeblock main.c lang:c %}
#include "greet.h"
int main() {
    greet ("World");
    return 0;
}
{% endcodeblock %}

{% codeblock greet.h lang:c %}
extern void greet(const char * who);
{% endcodeblock %}

{% codeblock greet.c lang:c %}
#include <stdio.h>
void greet (const char * who) {
    printf ("Hello, %s\n", who);
}
{% endcodeblock %}

(Yes, it really is the old standard “Hello, World” program. I did say we were starting with the basics!)

(是的，这是一个旧式的的“Hello, World”程序。正如我说过的我们从基础开始。)

To compile and run this collection of files, a simple shell script like the following is adequate.

编译并运行这些文件， 使用下面的一个简单的shell脚本

{% codeblock build.sh lang:bash %}
#include <stdio.h>
void greet (const char * who) {
    printf ("Hello, %s\n", who);
}
{% endcodeblock %}

For those not familiar with compiling C code, the cc command is the C compiler. It generates an output file (specified by the -o flag) from the source files listed on the command line.

如果大家不熟悉编译C代码， 命令cc是一个C编译器。在命令行里读取指定的源代码之后生成一个输出文件(使用参数-o)

Running it gives us the following results …

运行它之后我们会得到下面这些结果

{% codeblock output %}
$ build.sh
$ ./hello
Hello, World
{% endcodeblock %}

**Building C Programs**

Compiling C programs is really a two step process. First you compile all the source code file into object files. Then you take all the object files and link them together to make the executable.

编译C程序实际上有两步过程。首先你编译所有的源代码文件到对象文章中。然后你再link这些对象文件去创建一个可执行文件。

The following figure illustrates the progression from source files to object files to executable program.

接下来的流程是从源码到对象文件再到可执行程序。

Our program is so small that there is little benefit in doing more than the three line build script above. However, as projects grow, there are more and more source files and object files to manage. Recompiling everything for a simple one line change in a single source file gets old quickly. It is much more efficient to just recompile the few files that change and then relink.

我们的程序小到以至于只用了三行build脚本就可以搞定。然而，随着项目的增长，这里会有更多的源代码文件和对象文件需要管理。在源码中任意一行代码的更改都需要重新编译所有的东西。它必须只去编译那些更改的文件并重新链接才会更有效。

But how do we know what to recompile? Keeping track of that would be quite error prone if we tried to do that by hand. Here is where Rake become useful.

但是我们怎么指导哪些需要重新编译？ 如果我们尝试手动的去跟踪常常会遇到一些错误。这时候Rake就派上用场了。

**File Dependencies**

文件依赖

First, lets take a look at when files need to be recompiled. Consider the main.o. Obviously if the main.c file changes, then we need to rebuild main.o. But are the other files that can trigger a recompile of main.o?

首先，让我们看看哪些文件需要重新编译的。看一下 main.o，显然的，如果 main.c 文件更改了，那么我们需要重新build main.o。 但是其他的文件也会触发重新编译main.o？

Actually, yes. Looking at the source of main.c, we see that it includes the header file greet.h. That means any changes in greet.h could possibly effect the main.o file as well.

事实上是的。看main.c的源代码，我们看到它包含了一个头文件greet.h。那意味着greet.h的改变也会影响main.o。

We say that main.o has a dependency on the files main.c and greet.h. We can capture this dependency in Rake with the following line:

由此我们可以知道main.o依赖main.c和greet.h。使用Rake我们可以使用一行代码去处理这个依赖问题。

**Rakefile Fragment**

Rakefile 片段

{% codeblock lang:ruby %}
file "main.o" => ["main.c", "greet.h"]
{% endcodeblock %}

The rake dependency declaration is just regular Ruby code. We take advantage of the fact that we can construct hash arguments on the fly, and that Ruby doesn’t require parenthesis around the method arguement to create a file task declaration that reads very naturally to the humans reading the rake file. But its still just Ruby code.

rake依赖定义是一个正常的Ruby代码。实际上我们利用了Ruby的hash结构的参数，并且不需要括号来包裹参数，创建文件任务定义读起来是那么的自然，人性化。 但它的确只是Ruby代码。

Likewise, we can declare the dependencies for creating the “greet.o” file as well.

同样的，我们定义一个创建“greet.o”的依赖关系。

**Rakefile Fragment**

{% codeblock lang:ruby %}
file "greet.o" => ["greet.c"]
{% endcodeblock %}

greet.c does include stdio.h, but since that is a system header file and not subject to change (often), we can leave itout of the dependency list.

greet.c包含stdio.h，但是系统头文件常常不会变动，我们可以不用再写它的依赖列表。

Finally we can declare the dependencies for the executable program hello. It just depends on the two object files.

最终，我们可以定义可执行文件hello的依赖关系。它只依赖两个对象文件。

**Rakefile Fragment**

{% codeblock lang:ruby %}
file "hello" => ["main.o", "greet.o"]
{% endcodeblock %}

Notice that we only have to declare the direct dependencies of hello. Yes, hello depends on main.o which in turn depends on main.c. But the .c files are not directly used in building hello, so they can safely be omitted from the list.

提醒一下，我们只是定义了hello的直接依赖。hello依赖main.o它又依赖main.c。但是.c源码文件没有直接用来build hello，所以它可以安全的被忽略。

**Building the Files**

We have carefully specified how the files are related. Now we need to say what Rake would have to do to build the files when needed.

我们仔细的指出这些文件是如何关联的。现在我们需要告诉你当build这些文件的时候rake是怎么工作的。

This part is pretty simple. The three line build script that we started with contains all the commands needed to build the program. We just need to put those actions with the right set of dependencies. Use a Ruby do / end block to capture actions …

这部分相当的简单。这三行build脚本包含了build程序所有的命令。我们只要把它们放在正确的依赖关系上。使用Ruby的 do/end 块去包裹这些指令。

The result looks like this:

结果看上去如下

**Rakefile**

{% codeblock lang:ruby %}

 file 'main.o' => ["main.c", "greet.h"] do
    sh "cc -c -o main.o main.c"
 end
    
 file 'greet.o' => ['greet.c'] do
   sh "cc -c -o greet.o greet.c"
 end
   
file "hello" => ["main.o", "greet.o"] do
  sh "cc -o hello main.o greet.o"
end

{% endcodeblock %}

**Trying it out**

So, let’s see if it works!

然我们看看它是怎么工作的吧。

**Output**

{% codeblock lang:bash %}
$ rake hello
  (in /home/jim/pgm/rake/intro)
  cc -c -o main.o main.c
  cc -c -o greet.o greet.c
  cc -o hello main.o greet.o
{% endcodeblock %}

The command line rake hello instructs rake to look through its list of tasks and find one called “hello”. It then checks hello’s dependencies and builds them if required. Finally, when everything is ready it builds hello by executing the C compiler command.

命令rake hello告诉rake去到任务列表中找到一个叫“hello”的任务。然后它检查build hello的依赖关系。最终，准备妥当后执行C编译命令。

Rake dutifully reports what it is doing as it goes along. We can see that each compiler invocation is done in the correct order, building the main program at the end. So, does the program work? Let’s find out.

Rake诚实的报告了它独自做了什么事情。我们看到build main程序从都到尾调用的顺序。

**Output**

{% codeblock lang:bash %}
$ ./hello
Hello, World
{% endcodeblock %}

Success!

搞定

But what happens when we change a file. Lets change the greet function in greet.c to print “Hi” instead of hello.

但是我们更改了一个文件后会发生什么事情呢？ 我们来修改greet.c文件中的greet方法，用打印“Hi”去替换hello。

**Output**

{% codeblock lang:bash %}
$ emacs greet.c
$ rake hello
(in /home/jim/pgm/rake/intro)
cc -c -o greet.o greet.c
cc -o hello main.o greet.o
$
$ ./hello
Hi, World
{% endcodeblock %}

Notice that it recompiles greet.c making a new greet.o. And then it needs to relink hello with the new greet.o. Then it is done. There is no need to recompile main.c since it never changed.

提示，它重新编译了greet.c去创建新的greet.o。之后，它需要用新的greet.o去重新link hello。这样就完成了。由于main.c没有任何的改变，在这里不需要重新编译。

What do you think will happend if we run Rake again?

你认为重新执行Rake会发生什么事情？

**Output**

{% codeblock lang:bash %}
$ rake hello
(in /home/jim/pgm/rake/intro)
$
{% endcodeblock %}

That’s right … nothing. Everything is up to date with its dependencies, so there is no work for Rake to do.

哈哈，什么也没发生。所有的都是最新的，所以Rake不需要做任何事情。

Ok, sure. Rake is a bit of overkill for only two source files and a header. But imagine a large project with hundreds of files and dependencies. All of a sudden, a tool like Rake becomes very attractive.

的确，Rake对只有两个源码和一个头文件的项目十分在行。但是，想象一个大项目，有上百个文件和依赖关系，像Rake这样的工具也会非常地受欢迎。

#### Summary ####

What have we learned? Building a Rakefile involves identifying dependencies and the actions required to create the target files. Then declaring the dependencies and actions are as simple as writing them down in standard Ruby code. Rake then handles the details of building

我们学到了什么呢？创建一个Rakefile，这涉及到识别目标文件的依赖关系和动作, 然后将这些用标准的Ruby代码写到Rakefile中。最后交给Rake去处理building的细节。

#### What’s Up Next ####

接下来

We notice that even our small example has a bit of duplication in it. We have specify how to compile both C file separately, even though the only difference is the files that are used. The next installment will look at fixing that problem as well as introduce non-file based tasks, rules and file lists.

我们依然会用一个小的示例。我们会指定如何独立地编译C文件，即使是使用上的区别。下面问会解决一些文件无关的任务规则。
