---
layout: post
title: "Introducing DNSCrypt"
date: 2013-01-13 18:24
comments: true
categories: DNS
---

国内的网络通畅形势越来越严峻，网络被重置、无法访问的情况时有发生。而网络基础协议之一的DNS使用加密连接也成为必须的选择。OpenDNS就提供了一种对DNS进行加密的解决方案:DNSCrypt。这篇文章就是翻译其对DNSCrypt的介绍。

[原文链接](http://www.opendns.com/technology/dnscrypt/)

## Introducing DNSCrypt (Preview Release) ##

介绍DNSCrypt (发布预览)

### Background: The need for a better DNS security ###

背景: 需要更好的DNS加密

DNS is one of the fundamental building blocks of the Internet.  It's used any time you visit a website, send an email, have an IM conversation or do anything else online.  While OpenDNS has provided world-class security using DNS for years, and OpenDNS is the most secure DNS service available, the underlying DNS protocol has not been secure enough for our comfort.  Many will remember the [Kaminsky Vulnerability](http://www.unixwiz.net/techtips/iguide-kaminsky-dns-vuln.html), which impacted nearly every DNS implementation in the world (though not OpenDNS). 

DNS 是无联网的一个基础的组成部分。它应用在你访问网站，发送Email，实时聊天等任何的在线活动。多年来，OpenDNS一直提供世界级安全的DNS，并且OpenDNS提供的安全DNS服务是有效的，DNS协议并不是足够安全。很多人还会记得Kamisky Vulnerability，它影响了这个世界上的每一个DNS实现(不是通过OpenDNS)。

That said, the class of problems that the Kaminsky Vulnerability related to were a result of some of the underlying foundations of the DNS protocol that are inherently weak  -- particularly in the "last mile."  The "last mile" is the portion of your Internet connection between your computer and your ISP.  DNSCrypt is our way of securing the "last mile" of DNS traffic and resolving (no pun intended) an entire class of serious security concerns with the DNS protocol. As the world’s Internet connectivity becomes increasingly mobile and more and more people are connecting to several different WiFi networks in a single day, the need for a solution is mounting.

表明，和Kamisky Vulnerability相关的一类问题是由于DNS协议自身问题的结果。-- particularly in the "last mile." "最后一公里"是指你的电脑和ISP之间的互联网连接。DNSCrypt是我们的安全解决方案，保证DNS传输的“一公里”的安全和解析每一类DNS协议安全连接场景。当今世界已经成为移动互联网，很多人在一天中使用多种不同的WiFi接入到网络，我们需要一揽子的解决方案。

There have been numerous examples of tampering, or man-in-the-middle attacks, and snooping of DNS traffic at the last mile and it represents a serious security risk that we've always wanted to fix. Today we can.

这里有 numberous examples of tampering, 或者中间人攻击，以及在一公里进行DNS流量监控，它代表我们一直想去解决的安全风险场景。今天我们可以做到。

### Why DNSCrypt is so significant ###

Why DNSCrypt is so significant

In the same way the SSL turns HTTP web traffic into HTTPS encrypted Web traffic, DNSCrypt turns regular DNS traffic into encrypted DNS traffic that is secure from eavesdropping and man-in-the-middle attacks.  It doesn't require any changes to domain names or how they work, it simply provides a method for securely encrypting communication between our customers and our DNS servers in our data centers.  We know that claims alone don't work in the security world, however, so we've opened up the source to our DNSCrypt code base and it's available on [GitHub](http://www.github.com/opendns).

和使用SSL对HTTP加密网络传输方式一样，DNSCrypt在正常的DNS传输上加密，that is secure from eavesdropping and 中间人攻击。它不需要改变域名系统的工作，它在我们的客户和我们的DNS服务器之间提供一种简单的安全加密的通讯方法。我们知道我们不能在这个安全的世界内闭门造车，所以我们开放了DNSCrypt的源代码，它托管在Github中。

DNSCrypt has the potential to be the most impactful advancement in Internet security since SSL, significantly improving every single Internet user's online security and privacy.

TODO:

Download Now:

- [Download DNSCrypt for Mac](http://opendns.github.com/dnscrypt-osx-client/)
- [Download DNSCrypt for Windows](http://shared.opendns.com/dnscrypt/packages/windows-client/DNSCryptWin-v0.0.6.exe)


### Frequently Asked Questions (FAQ): ###

1. In plain English, what is DNSCrypt?

DNSCrypt是什么？

DNSCrypt is a piece of lightweight software that everyone should use to boost online privacy and security.  It works by encrypting all DNS traffic between the user and OpenDNS, preventing any spying, spoofing or man-in-the-middle attacks.

DNSCrypt是一个轻量级软件，它为任何一个使用在线隐私和安全的人。它可以加密所有用户和OpenDNS之间的DNS传输，preventing any spying, spoofing or man-in-the-middle attacks.

2. How can I use DNSCrypt today?

我先在可以使用DNSCrypt吗？

DNSCrypt is immediately available as a technology preview.  It should work, shouldn't cause problems, but we're still making iterative changes regularly.  You can download a version for Mac or Windows from the links above.

DNSCrypt作为一个技术预览版本已经可以使用。它工作起来没有什么问题，但是我们仍在改进它使它更易用。你可以从上面的链接去下载Mac或者Windows版本。

Tips:

提示:

If you have a firewall or other middleware mangling your packets, you should try enabling DNSCrypt with TCP over port 443.  This will make most firewalls think it's HTTPS traffic and leave it alone.

如果你使用防火墙或者其它的中间件管理你的数据包，你应该尝试使用443端口。这会保证你的防火墙把它作为https传输处理。

If you prefer reliability over security, enable fallback to insecure DNS.  If you can't reach us, we'll try using your DHCP-assigned or previously configured DNS servers.  This is a security risk though.

TODO

3. What about DNSSEC? Does this eliminate the need for DNSSEC?

No. DNSCrypt and DNSSEC are complementary.  DNSSEC does a number of things.  First, it provides authentication. (Is the DNS record I'm getting a response for coming from the owner of the domain name I'm asking about or has it been tampered with?)  Second, DNSSEC provides a chain of trust to help establish confidence that the answers you're getting are verifiable.  But unfortunately, DNSSEC doesn't actually provide encryption for DNS records, even those signed by DNSSEC.  Even if everyone in the world used DNSSEC, the need to encrypt all DNS traffic would not go away. Moreover, DNSSEC today represents a near-zero percentage of overall domain names and an increasingly smaller percentage of DNS records each day as the Internet grows.

That said, DNSSEC and DNSCrypt can work perfectly together.  They aren't conflicting in any way.  Think of DNSCrypt as a wrapper around all DNS traffic and DNSSEC as a way of signing and providing validation for a subset of those records.  There are benefits to DNSSEC that DNSCrypt isn't trying to address. In fact, we hope DNSSEC adoption grows so that people can have more confidence in the entire DNS infrastructure, not just the link between our customers and OpenDNS.

4. Is this using SSL? What's the crypto and what's the design?

它使用SSL？用什么加密，是怎么设计的？

We are not using SSL.  While we make the analogy that DNSCrypt is like SSL in that it wraps all DNS traffic with encryption the same way SSL wraps all HTTP traffic, it's not the crypto library being used.  We're using elliptical-curve cryptography, in particular the [Curve25519](http://dnscurve.org/crypto.html) eliptical curve.  The design goals are similar to those described in the [DNSCurve forwarder](http://dnscurve.org/out-implement.html) design.

我们没有使用SSL。 一直以来我们说DSNCrypt类似SSL，它是对DNS传输的包装，是和SSL对HTTP传输的包装的路子一样， 它没有使用任何的加密库。 我们使用elliptical-curve加密算法。设计目标和DNSCurve formarder设计的类似。
