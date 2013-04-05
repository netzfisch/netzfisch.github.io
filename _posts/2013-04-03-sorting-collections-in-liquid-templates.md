---
layout: post
category: ruby
tags: 2cent application jekyll liquid coding
title: Sorting collections in Liquid templates
---
Sorting in Jekyll looks quiet simple with standard [Liquid Filter](https://github.com/Shopify/liquid/wiki/Liquid-for-Designers) "sort", but it did not worked out. 

Liquid converts ruby hashes to arrays, e.g the variables beneath **site** - like **categories** and **tags**:

{% highlight ruby %}
    for tag in site.tags 
      <li>{{ tag[0] }}</li>
    endfor 
{% endhighlight %}

... site.tags will be converted by Jekyll to an array of tuples in which \[0\] refers to the key,\[1\] the list of values. Which you can not map to be sorted alphabetically by the key \[0\] of each tuple.

There’s a pull request that adds it at [Shopify/liquid#101](https://github.com/Shopify/liquid/pull/101), but it looks like that’s not been merged (see also [mojombo/jekyll#766](https://github.com/mojombo/jekyll/issues/766)) ;-(
