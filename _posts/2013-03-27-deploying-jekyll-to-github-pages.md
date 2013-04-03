---
layout:    post
category:  ruby
tags:      application jekyll config 2cent
title:     Deploying Jekyll with plugins to GitHub Pages
---
... was not that easy, as I thought in my [last post][1], because [GitHub Pages][2] do **not allow any custom plugins!**. So I came up with the following strategy also used by [Octopress][3] and well described from [Ixti][4]:

1. First create a scecond branch called e.g. *source*
2. and make it default at [GitHub][5] Repository.

That way you can compile locally and publish your site to the master branch, which leaves you version control under the source branch.

[Ixti][4] has also a nice implementation for publishing, so you can do `$ rake publish`.

[1]: {% post_url 2013-03-24-jekyll-jumpstart %}
[2]: https://pages.github.com/
[3]: http://octopress.org/
[4]: http://ixti.net/software/2013/01/28/using-jekyll-plugins-on-github-pages.html
[5]: http://github.com/
