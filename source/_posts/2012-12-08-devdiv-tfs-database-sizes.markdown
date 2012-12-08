---
layout: post
title: "DevDiv TFS Database Sizes"
date: 2012-12-08 13:53
comments: true
categories: TFS
---

����һƪ����Brian Harry�Ĳ��ͣ��������·���ʱ(2009)TFS�Ŷ��Լ���TFS���ݿ��״����

#DevDiv TFS Database Sizes#

[ԭ������](http://blogs.msdn.com/b/bharry/archive/2009/05/31/devdiv-tfs-database-sizes.aspx)

> Someone asked me the other day how big to expect the relative sizes of TFS databases to be.  At the time all I had time to say was ��Over time TfsVersionControl will dwarf everything else��.  This weekend, I finally had a few minutes to sit down and do some analysis.  As with all such things, your mileage will vary.  DevDiv is a VERY heavy version control user and this may be a bit disproportionate from what you��ll see but as a system grows, I expect it will start to look more and more like this.

��������TFS���ݿ���� һ���Ҷ��ǻش�����ʱ������ƣ�TfsVersionControl�ܻ��Եú�׳��"�� ����ĩ���������е�ʱ���������ʹ���һЩ������ÿ���˵��������һЩ����DevDiv���㿴����ϵͳ���ܻ�ܲ���ƣ�����ĺܴ��������ܿ�����������֮����

> Here��s a pie chart that shows you relative sizes:

����һ�����ݿ��С��صı�ͼ

<img src="http://blogs.msdn.com/blogfiles/bharry/WindowsLiveWriter/DevDivTFSDatabaseSizes_BB10/image_thumb_5.png" alt="a pie chart shows you relatvie sizes" />

> And here��s the actual numbers:

�Լ�һЩ��ʵ������

<img src="http://blogs.msdn.com/blogfiles/bharry/WindowsLiveWriter/DevDivTFSDatabaseSizes_BB10/image_thumb_6.png" alt="the actual numbers" />

> It��s worth looking at how TfsVersionControl breaks down.  Note this won��t match your schema exactly because it is a hybrid TFS 2008/TFS 2010 schema but what I show you will be close.

TfsVersionControl��ֵ��ȥ����һ�¡� �����������ʵ�ʵ����ݿ�schema��һ�£���Ϊ����һ��TFS 2008/TFS 2010��schema����壬�������ܽӽ�����Ҫ˵����˼��

<img src="http://blogs.msdn.com/blogfiles/bharry/WindowsLiveWriter/DevDivTFSDatabaseSizes_BB10/image_thumb_3.png" alt="TfsVersionControl Database">

<img src="http://blogs.msdn.com/blogfiles/bharry/WindowsLiveWriter/DevDivTFSDatabaseSizes_BB10/image_thumb_4.png" alt="the table's numbers" />
