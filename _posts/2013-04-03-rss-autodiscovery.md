---
layout: post
category: web
tags: 2cent rss feed coding html
title: RSS Autodiscovery
---
Ever thought about the nifty RSS-symbol in the browsers adress bar and how to get it there? Quiet easy - just put the the following meta-tag into the `<head>` section of your html-site:

{% highlight html %}

<link rel="alternate" type="application/atom+xml" title="Example RSS Feed" href="http://www.example.com/xml/atom.xml">

{% endhighlight %}

### Links
* [Chromium Reference](http://dev.chromium.org/user-experience/feed-subscriptions)
* [RSS Advisory Board](http://www.rssboard.org/rss-autodiscovery)
