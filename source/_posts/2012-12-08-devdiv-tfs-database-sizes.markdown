---
layout: post
title: "DevDiv TFS Database Sizes"
date: 2012-12-08 13:53
comments: true
categories: TFS
---

这是一篇来自Brian Harry的博客，介绍文章发表时(2009)TFS团队自己的TFS数据库的状况。

[原文链接](http://blogs.msdn.com/b/bharry/archive/2009/05/31/devdiv-tfs-database-sizes.aspx)

> Someone asked me the other day how big to expect the relative sizes of TFS databases to be.  At the time all I had time to say was “Over time TfsVersionControl will dwarf everything else”.  This weekend, I finally had a few minutes to sit down and do some analysis.  As with all such things, your mileage will vary.  DevDiv is a VERY heavy version control user and this may be a bit disproportionate from what you’ll see but as a system grows, I expect it will start to look more and more like this.

有人问我TFS数据库会多大？ 一般我都是回答“随着时间的推移，TfsVersionControl总会显得很壮观"。 这周末，我终于有点时间坐下来就此做一些分析。每个人的情况会有一些区别。DevDiv是TFS一个很大的用户，并且可能与你看到过的系统增长有些不同。我期望能看到更多彼此的相似之处。

> Here’s a pie chart that shows you relative sizes:

这是一幅数据库大小相关的饼图

<img src="http://blogs.msdn.com/blogfiles/bharry/WindowsLiveWriter/DevDivTFSDatabaseSizes_BB10/image_thumb_5.png" alt="a pie chart shows you relatvie sizes" />

> And here’s the actual numbers:

以及一些真实的数据

<img src="http://blogs.msdn.com/blogfiles/bharry/WindowsLiveWriter/DevDivTFSDatabaseSizes_BB10/image_thumb_6.png" alt="the actual numbers" />

> It’s worth looking at how TfsVersionControl breaks down.  Note this won’t match your schema exactly because it is a hybrid TFS 2008/TFS 2010 schema but what I show you will be close.

TfsVersionControl很值得去分析一下。 提醒这个和你实际的数据库schema不一致，因为他是一个TFS 2008/TFS 2010的schema混合体，但是他很接近我想要说的意思。

<img src="http://blogs.msdn.com/blogfiles/bharry/WindowsLiveWriter/DevDivTFSDatabaseSizes_BB10/image_thumb_3.png" alt="TfsVersionControl Database">

<img src="http://blogs.msdn.com/blogfiles/bharry/WindowsLiveWriter/DevDivTFSDatabaseSizes_BB10/image_thumb_4.png" alt="the table's numbers" />
