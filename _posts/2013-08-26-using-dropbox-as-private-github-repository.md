---
layout: post
category: tools
tags: dropbox git github mashup
title: Dropbox as "private" github repository
---
If you need a **private cloud repository** for free, **dropbox and git** are your friends! Basically you just need 

* to create a bare git repository within the dropbox folder!
* Than hook up your new bare repo as an upstream to your existing one.

### Setup

    $ mkdir dropbox/git
    $ cd dropbox/git
    $ git init --bare cloud-repo.git
    $ cd ~/local-repo
    $ git remote add dropbox ~/dropbox/git/cloud-repo.git

### Usage

Just do a `$ git push dropbox master` and dropbox will take care for you via cloud syncronization.

Check out the repo by `$ git clone ~/dropbox/git/cloud-repo.git` from any client with **dropbox syncronization**!
