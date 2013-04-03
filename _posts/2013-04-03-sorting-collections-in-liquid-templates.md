---
layout: post
category: ruby
tags: 2cent application jekyll liquid coding
title: Sorting collections in Liquid templates
---
Sorting in Jekyll looks quiet simple with standard [Liquid Filter](https://github.com/Shopify/liquid/wiki/Liquid-for-Designers) "sort"! but did not worked at the beginning. 

Than I found the [Liquid Issue #143](https://github.com/Shopify/liquid/issues/143), which basically - says get rid of whitespaces near the "pipe" and "sort-filter":

{% highlight ruby %}

for tag in site.tags|sort:"alpha-asc"

{% endhighlight %}
