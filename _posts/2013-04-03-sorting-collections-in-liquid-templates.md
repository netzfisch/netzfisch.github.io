---
layout: post
category: ruby
tags: application jekyll liquid template coding 2cent
title: Sorting collections in Liquid templates
---
**Sorting in Jekyll** looks quiet simple with standard [Liquid Filters](https://github.com/Shopify/liquid/wiki/Liquid-for-Designers) `sort`, but it didn`t worked out. 

Because **Liquid** converts ruby hashes to arrays, e.g the Jekyll variables beneath *site*, like *categories* and *tags* (e.g. site.tags):

{% highlight ruby %}
for tag in site.tags 
  <li>{{ tag[0] }}</li>
endfor 
{% endhighlight %}

e.g. *site.tags* will be converted to an array of tuples in which \[0\] refers to the key and \[1\] to the list of values. **Without monkey patching** you can`t sort the tuples alphabetically by the key \[0\] of each of them - very sad!

There`s a pull request that adds it at [Shopify/liquid#101](https://github.com/Shopify/liquid/pull/101), but it looks like that`s not been merged (see also [mojombo/jekyll#766](https://github.com/mojombo/jekyll/issues/766)) ;-(
