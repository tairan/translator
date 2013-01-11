---
layout: post
title: "Rake Tutorial 2"
date: 2013-01-04 14:38
comments: true
categories: [Ruby, Rake]
---

[Rake Tutorial](http://docs.rubyrake.org/tutorial/chapter02.html)

##Chapter 2—Managing Files with FileLists##

Handling Common Actions

处理通用动作

> Rake is a tool for controlling builds. In this part of the Rake tutorial, we see how to organize the Rake actions to apply to many similar tasks.

> Rake 是一个用来控制build的工具。在此指南的这部分，我们去看如何组织Rake动作并应用到相似的任务中去。

> In the previous chapter, we talked about the basics of specifying dependencies and associating actions to build the files. We ended up with a nice Rakefile that built our simple C program, but with some duplication in the build rules.

> 在前一个章节中，我们谈论了关于去构建Rakefile用到特定依赖和关联行为的基本知识。最后我们有了一个不错的用来build我们简单的C程序的Rakefile，但是在build规则中有一些重复。

But First, Some Extra Rake Targets

首先，一些额外的Rake目标

But before we get into all that, lets add some convience targets to our Rakefile. First of all, it would be nice to have a default target that is invoked when we don’t give any explicit task names to rake. The default target looks like this:

在我们深入开始之前，让我们为Rakefile添加一些切实可行的目标。首先，当我们没有为rake明确声明任务名称时，它会被作为默认的目标被调用。这个默认的目标如下

Rakefile Fragment

{% codeblock lang:ruby %}
task :default => ["hello"]
{% endcodeblock %}

Until now, the only kind of task we have seen in Rake are file tasks. File tasks are knowledgable about time stamps on files. A file task will not execute its action unless the file it represents doesn’t exist, or is older than any of its prerequisites.

直到现在，在Rake中我们看到的唯一的任务类型是file tasks。File tasks很擅长于文件上的时间戳处理。 除非file task代表它不存在，或者比它要求的时间更旧，否则file task不会执行这个动作。

A non-file task (or just plain “task”) does not represent the creation of a file. Since there is no timestamp for comparison, non-file tasks always execute their actions (if they have any). Since the default task does not represent a file named “default”, we use a regular non-file task for this purpose. Non-file tasks just use the task keyword (instead of the file keyword).

一个非文件任务(或只是简单的“task”) 并不代表创建一个文件。因为这里没有时间戳用来比对，所以非文件任务总是执行这些动作(如果它有的话)。默认的任务不代表一个叫“default”的文件，为了这个目的，我们使用一个非文件任务。非文件任务只是用任务关键字。（替代file关键字）

Here are a couple of other really useful tasks that I almost always include in a Rakefile.

这里有一组真实有用的任务，我通常总会将他们包含在Rakefile中。

clean:
	Remove temporary files created during the build process.

清理:
	移除在build过程期间创建的临时文件。

clobber:
	Remove **all** files generated during the build process.

clobber:
	移除所有在build过程期间生成的文件。

clean tidies up the directories and removes any files that generated as part of the build process, but are not the final goal of the build process. For example, the .o files used to link up the final executable hello program would fall in this category. After the executable program is built, the .o files are no longer needed and will be removed by saying “rake clean”.

整理目录，并且清理在build处理期间生成的文件，但不是build流程最终的目标结果。例如： 用来link最终可执行程序的.o 文件都归为这类。 在可执行程序build完成之后，.o 文件就不再需要了，把它们移除掉可以称为"rake clean"。

clobber is like clean, but even more aggressive. “rake clobber” will remove all files that are not part of the original package. It should return a project to the “just checked out of CVS” state. So it removes the final executable program as well as the files removed by clean.

colbber类似clean，但是更具有侵略性. "rake colbber"会移除所有不是原包中的所有文件。它会回到“刚刚从cvs中checkout”的状态。因此它会做clean相关的移除外还会删除最终的可执行程序。

In fact, these tasks are so common, Rake comes with a predefined library that implements clean and clobber.

实际上，这些任务是如此的通用，Rake有预定义的库来实现清理和clobber.

But every project is different, how do we specify which files are to be cleaned and clobbered on a per project basis?

但是每个项目的都不尽相同，我们怎么为每个项目指定哪些文件需要被清理和clobbered呢？

The answer is File lists.

答案是，文件列表。

File Lists to the Rescue

文件列表去拯救

A file list is simply a list of file names. Since a lot of what Rake does involves files and lists of those files, a file list has some special features to make manipulating file names rather easy.

文件列表是一个简单的文件名字的列表。因为Rake做的很多事情是调用文件和那些文件的列表，所以这个文件列表具有一些特征使处理文件名更容易。

Suppose you want a list of all the C files in your project. You could add this to your rake file:

假如你想要一个在你项目中所有C文件的列表。你可以增加下面的代码在你的Rakefile中。

Rakefile Fragment

{% codeblock lang:ruby %}
SRC = FileList['*.c']
{% endcodeblock %}

This will collect all the files ending in ”.c” in the top level directory of your project. File lists understand glob patterns (i.e. things like ".c") and will find all the matching files.

这会手机你项目中顶级目录中文件名以".c"结尾的文件。文件列表支持glob模式。(i.e. 比如 ".c")并且会找到所有符合的文件。

By the way, no matter where you invoke it, rake always executes in the directory where the Rakefile is found. This keeps your path names consistent without depending on the current directory the user interactive shell.

顺便提一下, 无论你在什么地方调用它, rake总是在Rakefile所在的目录中执行。这能保证你在shell中不会依赖当前目录。

The clean and clobber tasks use file lists to manage the files to remove. So if we want to clean up all the .o files in a project we could try …

清理和clobber任务使用文件列表来管理哪些文件被移除。因此如果我们需要清理项目中的所有.o文件，可以试一下..

Rakefile Fragment*

{% codeblock lang:ruby %}
CLEAN = FileList['*.o']
{% endcodeblock %}

(CLEAN is the file list associated with the clean task. I’ll let you guess the name of the file list associated with clobber).

(CLEAN 是关联到清理任务的文件列表。 我会让你猜一下关联到clobber任务的文件列表名字)

##Chapter 3—Reducing Duplication with Rules##

Dynamically Building Tasks

动态的Building任务

The command to compile the main.c and greet.c files is identical, except for the name of the files involved. The simpliest and most direct way to address the problem is to define the compile task in a loop. Perhaps something like this …

编译 main.c 和 greet.c 文件的命令是完全一致的，除了被调用的文件名。最简单直接的方式去处理这个问题是在一个循环中定义编译任务。示例

Rakefile Fragment

{% codeblock lang:ruby %}
SRC = FileList['*.c']
SRC.each do |fn|
  obj = fn.sub(/\.[^.]*$/, '.o')
  file obj  do
    sh "cc -c -o #{obj} #{fn}"
  end
end
{% endcodeblock %}

Just a couple things to note about the above code.

关于上面的代码仅有两个需要注意的地方。

- The dependencies are not specified. This is a common where we specify the dependents at one place and the actions in another. Rake is smart enough to combine the dependencies with the actions.

- 依赖没有指定。这种在一个地方定义指定依赖另一个地方执行是一种普遍现象，Rake具有足够的智能去绑定这些依赖关系和行为。

- Although the task was named after the .o (which is, after all, what we want to generate), the file list is defined in terms of the .c files. Why?

- 尽管任务使用 .o 来命名 (毕竟这些是我们想去生成的东西), 但是文件列表依照 .c 去定义，为什么呢？

The simple reason is that file lists search for file names that exist in the file system. We have no guarantee that the .o files even exist at this point (indeed, the will not after invoking the clean task). The .c are source and will always be there.

一个简单的理由是文件列表在文件系统中搜索已经存在的文件的文件名。我们不能保证.o文件是一直存在的.(确实，在调用clean任务后就会没有了). .c作为源代码总是存在。

Rake Can Automatically Generate Tasks

Rake 能自动生成任务

Defining tasks in a loop is pretty cool, but is really not needed in a number of simple cases. Rake can automatically generate file based tasks according to some simple pattern matching rules.

在一个循环中定义任务很酷，但是在一些简单的任务中并不需要。Rake能基于基本的任务规则去生成相应匹配的任务。

For example, we can capture the above logic in a single rule … no need to find all the source files and iterate through them.

示例，我们可以按照一个单个的规则来捕获上面的逻辑。 不需要查找所有的源代码并迭代处理。

Rakefile Fragment

{% codeblock lang:ruby %}
rule '.o' => '.c' do |t|
  sh "cc -c -o #{t.name} #{t.source}"
end
{% endcodeblock %}

The above rule says that if you want to generate a file ending in .o, then you if you have a file with the same base name, but ending in .c, then you can generate the .o from the .c.

上面的规则说如果你要生成一个以 .o 结尾的文件，然后如果你有一个文件和基础文件有相同的名称，但是你可以从 .c 文件生成 .o

t.name is the name of the task, and in file based tasks will be the name of the file we are trying to generate. t.source is the name of the source file, i.e. the one that matches the second have of the rule pattern. t.source is only valid in the body of a rule.

t.name 是任务的名称，在文件任务中，我们会尝试以文件的名称来命名生成的文件。 t.source 是其源文件的名字，i.e. the one that matches the second have of the rule pattern. t.source 在规则中唯一有效的。

Rules are actually much more flexible than you are led to believe here. But that’s an advanced topic that we will save for another day.

规则实际上可以做的比你想象中的更加灵活。但是那是高级话题，我们会在以后再说。

Final Rakefile

Here is our final resule. Notice how we use the SRC and OBJ file lists to manage our lists of scource files and object files.

这是我们最终的结果。提示我们是怎么使用SRC和OBJ文件列表去管理我们的源文件和目标文件的。

Rakefile

{% codeblock lang:ruby %}
require 'rake/clean'

CLEAN.include('*.o')
CLOBBER.include('hello')

task :default => ["hello"]

SRC = FileList['*.c']
OBJ = SRC.ext('o')

rule '.o' => '.c' do |t|
  sh "cc -c -o #{t.name} #{t.source}"
end

file "hello" => OBJ do
  sh "cc -o hello #{OBJ}"
end
  
# File dependencies go here ...
file 'main.o' => ['main.c', 'greet.h']
file 'greet.o' => ['greet.c']

{% endcodeblock %}

