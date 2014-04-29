---
layout: post
title:  "jekyll文章摘要的实现"
tags: [jekyll, 摘要]
---

Summary: 一开始想用插件的，貌似github不支持，于是乎在加上特殊的标签来搞定..........


<!--summary-->
一开始想用插件的，貌似github不支持，于是乎在加上特殊的标签来搞定
{% highlight html %}
---
layout: post
title:  "jekyll文章摘要的实现"
tags: [jekyll, 摘要]
---
文章摘要
<!--more-->
文章内容
{% endhighlight %}

显示的时候就
{% highlight html %}
post.content | split:'<!--more-->' |first
{% endhighlight %}

---
总的来说这中模式更加的自由些，想加到哪里就加到哪里。
