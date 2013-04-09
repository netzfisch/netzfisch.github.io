---
layout: post
category: tools
tags: howto application jekyll markdown
title: My Jekyll blogging workflow
published: false
---
Lately I switched back to a static blog generation and enjoying a higher blog post frequence. Following is my workflow

* Either I use VIM on my local box
* or in mobile enviroment a Chrome app, called ![markdown](https://www.google.de/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&ved=0CDUQFjAA&url=https%3A%2F%2Fchrome.google.com%2Fwebstore%2Fdetail%2Fmarkdown-here%2Felifhakcjgalahccnjkneoccemfahfoa&ei=Vk9cUYSVDsjZswbR4YHAAw&usg=AFQjCNE9AjPvFKhon79znSJayV5tG-K4ug&sig2=VXKsp4HmOmzccK0S_bigpw&bvm=bv.44697112,d.Yms)
* save the file to [DropBox](https://dropbox.com) or [GDrive](https://gdrive.com)
* then generate the site `$ jekyll` and
* push to GitHub

Alternatively, just edit directly at GitHub
* than do `git pull origin soure`
* compile `jekyll build`
* and push `git push origin master`
