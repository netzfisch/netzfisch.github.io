---
layout: post
category: tools
tags: howto application jekyll markdown workflow
title: Perfect Jekyll blogging workflow
---
[Lately]({% post_url 2013-03-24-jekyll-jumpstart %}) I switched back to a **static blog generation** - enjoying a **higher** blog **post frequence**. This are my **perfect blogging workflow** alternatives:

Either just use [VIM](http://www.vim.org) on a local box or in mobile enviroment take a Chrome-App - called [textdown](https://chrome.google.com/webstore/detail/textdown/efalomlklhakojjbdfehfkgoicablooc):
* save the file to [DropBox](https://dropbox.com) or [GDrive](https://gdrive.com)
* then generate the site with `$ jekyll build` and
* push to GitHub!

Alternatively you could edit directly at GitHub:

* than `$ git pull origin source`
* compile `$ jekyll build` and
* push `$ git push origin master`
