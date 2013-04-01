---
layout:    post
category:  howto
tags:      application git
title:     git basics
---
### Customise to your needs

    $ git config --global user.name "Your Name"
    $ git config --global user.email yourname@yourdomain.com

### Create a .gitignore file

    $ less .gitignore
    .DS_Store
    log/*.log
    tmp/**/*
    config/database.yml
    db/*.sqlite3

### Initialize git project

    git init
    git status
    git add .
    git commit -a -m 'initial project'

### Checkout, Merge and Push

* [git cheat sheet](http://cheat.errtheblog.com/s/git/)
* [Switching from SVN to git](http://git.or.cz/course/svn.html)
