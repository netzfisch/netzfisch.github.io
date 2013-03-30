---
layout: post
category: howto
tags: howto application jekyll
title: Jekyll Jumpstart - switching back to static site generation
---
[Blogging like a hacker](http://tom.preston-werner.com/2008/11/17/blogging-like-a-hacker.html) suited me allways best, not really suprising because of my engineer background:

* So I started at the end of the **last century** with a [hand made sites](http://web.archive.org/web/*/http://netzfisch.de),
* later using some ruby/perl pre-processors [webgen](http://webgen.rubyforge.org/), [WML](http://thewml.org/),[WPP](http://wpp.sourceforge.net/),
* than switching to python based [Leonardo](http://jtauber.com/leonardo/) and
* later to [enki](https://github.com/xaviershay/enkit) a leight weight rails blog engine.

I never touched the main stream systems like MoveableType or the successor WordPress, but used in various occasions **Software as a Service (SaaS)** like Blogger (long before it was aquired by google) and than Posterous, which just recently disappeard ...

That way I left my content in various systems and locations and never got it really aggregated back. Nowadays with Google+, FourSquare and Twitter things are getting worse!

Sometimes I am realy tired searching for something I already commented, saved, etc. So my new content strategy aims chossing

* a neutral data storage (markdown) combined with a resoponsive design - better a mobile first concept 
* an independent and robust solution with flexible posibilities to realize my backlog and
* a variety of deployment options to choose from.

Finally it should not take me ages to get started. Sounds like **back to the roots** and probably partly it is. By time I will re-import integrate worthfully old content. But now lets get in into it!

### Jumpstart ###

Shortly I thought about [OCTOPRESS](http://octopress.org/docs/) but keept going the rough way. So read the Jekyll [getting started section](https://github.com/mojombo/jekyll/) and have a look at some sites and source code, which could serve as template or good starting point to find your way:

* [Tom Preston Werner](http://tom.preston-werner.com) [(source)](https://github.com/mojombo/mojombo.github.com/) - minimalistic version from the Jekyll creator and github founder
* [UberRobert](http://www.uberobert.com/) [(source)](https://github.com/rbirnie/uberobert.com/) - two column Twitter Bootsrap (TB) mit *Marketing Speak Header* and **very** clean structured source code
* [Nate Wieger](http://www.nateware.com/) [(source)](https://github.com/nateware/nateware-blog/) - two column TB basis with custom google search integration
* [Adam Ralph](http://adamralph.com/) [(source)](https://github.com/adamralph/adamralph.github.com/) - two column TB basis with a static tag cloud

and many more on the [Jekyll Wiki](https://github.com/mojombo/jekyll/wiki/Sites/) Site.

Than bash `$ gem install jekyll` and customise [Twitter Boostrap](http://get.bootstrap.com/) or head to [Bootswatch](http://bootswatch.com/) for ready to use layouts.

Plugins and extensions I used so far:

* Jekyll [tag cloud plugin](https://gist.github.com/710577/) by Ilkka Laukkan
* Embedded [Twitter timeline](https://dev.twitter.com/docs/embedded-timelines/)

### Deployment

Ther are variety of [options](https://github.com/mojombo/jekyll/wiki/Deployment): 

1. push to [github](https://github.com) and get version control - community participation for free
2. push to [heroku](http://heroku.com) and scale away
3. push to [amazon S3](http://www.nateware.com/jekyll-plus-twitter-bootstrap-on-s3.html) wiht CloudFront, as [Jeff Bezos](http://www.allthingsdistributed.com/2011/08/Jekyll-amazon-s3.html) also does
4. or just **rsync** the \_site directory to your static webserver

Your choice, my favorites are GitHub:

    $ git init
    $ git add .
    $ git commit -m 'initial blog commit'
    $ git remote add origin git@github.com:YourRepository/YourRepository.git
    $ git push origin master

and heroku:

    $ git init
    $ git add .
    $ git commit -m 'initial blog commit'
    $ heroku create
    $ git config branch.master.remote heroku
    $ git push heroku master

That it is, have fun.
