---
layout: post
title: "How to use git over an HTTP proxy, with socat"
date: 2013-01-27 18:22
comments: true
categories: 
---

[原文链接](http://sitaramc.github.com/tips/git-over-proxy.html)

如何使用```socat```让```git```通过HTTP代理

Corporate firewalls and proxy typically block both of these (and often a lot more); here's how to get around them.

企业防火墙和代理通常会阻止它们(通常还有很多)，这里则是教你如何绕过它们。

If you're tracking a public repo, you will need to use the "git" protocol, because the "http" protocol is not very efficient, and/or requires some special handling on the server side. If you're pushing code to a public repo, you definitely need to use the "ssh" protocol.

如果你在跟踪一个公共的仓库，你需要使用git协议，因为http协议不是很有效，在服务器端一些特殊的处理。如果你推送代码到公共的仓库，你一定要使用ssh协议。

##a word about socat##

I will be using "socat", an absolute corker of a program that does so many things it's incredible! Other people use corkscrew, ssh-https-tunnel, etc., which are all specialised for just one purpose. I prefer socat, and once you spend the 2-3 years :-) needed to read the man page, you will see why!

我会使用"socat"， 一个绝对的程序corker能做这么多事情，是多么难以置信的！ 其他人使用 ```corkscrew```, ```ssh-https-tunnel```等。这些工具都只有一个目的。我喜欢socat，一旦你花上个2-3年去读它的手册，你就会明白的。

The basic idea is that you will somehow invoke socat, which will negotiate with the HTTP(S) proxy server using the CONNECT method to get you a clean pipe to the server on the far side.

一个基本的思想是你会如何调用socat，它会和https代理服务进行交涉，使用一个连接方法给你一个干净的管道在远端服务器。

However, do note that socat does have one disadvantage: the passwords to your proxy server are visible in to local users running ```ps -ef``` or something. I don't care since I don't have anyone else logging into my desktop, and the ability to use a program I already have anyway (socat) is more important.

然而，注意socat确实有个缺点: 在本地用户运行```ps -ef``` or 其他的什么的时候，你代理服务器的密码是可见的。我不在乎，我知道没其他人登录到我的桌面电脑，并且我已经在任何地方都有一个程序（socat）是如此的重要。

##proxying the git protocol##

When I want to download a public repo, I just type

当我想要下载一个公共仓库的时候，我只要输入

{% codeblock lang:bash %}
proxied_git clone ...repo...
proxied_git pull
{% endcodeblock %}

and so on, instead of

替代下面的

{% codeblock lang:bash %}
git clone ...repo...
git pull
{% endcodeblock %}

Here's the how and why of it.

这里你会知道为什么这样做

To proxy the git protocol, you need to export an environment variable called ```GIT_PROXY_COMMAND```, which contains the command that is to be invoked. I have a shell function in my ```.bashrc``` that looks like this:

要代理```git```协议，你需要一个环境变量```GIT_PROXY_COMMAND```, 它包含了需要被调用的命令。我把它写在了 ```.bashrc``` 中，如下

{% codeblock lang:bash %}
proxied_git () 
( 
    export GIT_PROXY_COMMAND=/tmp/gitproxy;

    cat  > $GIT_PROXY_COMMAND <<EOF
#!/bin/bash
/usr/bin/socat - PROXY:172.25.149.2:\$1:\$2,proxyport=3128
EOF
    chmod +x $GIT_PROXY_COMMAND;

    git "$@"
)
{% endcodeblock %}

Possible variations are:

可能的变化

- you could give ```/tmp/gitproxy``` a more permanent name and remove the middle pararaph completely. I don't do this because that's too small a file to bother with; it just seems cleaner this way)
- 你可以给 ```/tmp/gitproxy``` 一个永久的名称并移除中间部分。我没有做这个因为这只是一个很小的文件，这个方法似乎也很清洁。
- you could permanently set the environment variable if all your git repos are remote (very unlikely)
- 如果你的git仓库都在远端，你可以永久的设置一个环境变量(不太可能)

One thing you cannot do is to roll the entire socat command into the environment variable. Git passes the host and port as two arguments to the proxy command, but socat expects them in the syntax you see above, so you will need to wrap it in a script as I have done. I guess you could argue that this is a point in favour of corkscrew etc. ;-)

一个事情是你不能做的就是roll整个socat命令到环境变量里。git通过host和port作为代理命令的两个参数，但是socat期望它们在你在上面看到的语法，所以你会需要使用一个脚本去包装它。我想你可能会说这点是corkscrew常用的等。

##proxying the ssh protocol##

The git protocol is handled directly by git (duh!), but if you use the ssh protocol, it invokes ssh explicitly (again, duh!).

git协议是由git直接处理的，但是如果你使用ssh协议，它会调用ssh。

Ssh already has this sort of stuff built-in, so you simply add a few lines to your ```~/.ssh/config```

ssh 通常都会内置，所以你仅需要在你的 ```~/.ssh/config``` 中加入几行

{% codeblock lang:apache %}
host gh
    user git
    hostname github.com
    port 22
    proxycommand socat - PROXY:your.proxy.ip:%h:%p,proxyport=3128,proxyauth=user:pwd
{% endcodeblock %}

Now you can just say (for example):

现在你可以这样使用

{% codeblock lang:bash %}
git clone gh:sitaramc/git-notes.git
{% endcodeblock %}

###ssh proxy using corkscrew instead of socat###

ssh代理使用corkscrew替代socat

download and install [corkscrew](http://www.agroman.net/corkscrew/)

下载并安装[corkscrew](http://www.agroman.net/corkscrew/)

create a file (eg., ``` ~/.ssh/myauth ```) and put your http proxy username and password as ```"username:password"``` in it and save it.
safeguard the file

创建一个文件(比如``` ~/.ssh/myauth```)并输入你的http代理的用户名和密码，格式为```"username:password"```，然后保存。修改它的权限

{% codeblock lang:bash %}
chmod 600 ~/.ssh/myauth
{% endcodeblock %}

open ```~/.ssh/config``` and add the following entry, adding an explicit path to corkscrew if needed.

打开 ```~/.ssh/config``` 并且增加下面的一些代码，如果需要的话，给scorkscrew一个完整的路径。

{% codeblock lang:apache %}
host gh
    user git
    hostname github.com
    port 22
    proxycommand corkscrew your.proxy.ip 3128 %h %p ~/.ssh/myauth
{% endcodeblock %}

###extra coolness for github###

Noting that many corporate firewalls block access to the CONNECT method on ports other than 443, the good folks at github have an ssh server listening on 443 if you use the host ```"ssh.github.com"```, so you can replace the hostname and the port in the above ssh config stanza as appropriate, and you're all set

很多防火墙会阻挡443端口的连接，作为一个github的好公民，如果你使用```"ssh.github.com"```，需要又一个ssh服务器去监听443端口， 那么你可以替换这个主机名和端口号在ssh config中，好了，搞定。
