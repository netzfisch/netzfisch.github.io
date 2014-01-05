---
layout:    default
tags:      developement git 2cent
title:     git cheat sheet, things to remember
---
### git cheat sheet

I'm not coding that often, but when the exception happens I often end up with a 'task' and the knowledge that there is a better way to handle the source code - but I can't remember!

Therefore I note here some **plain and ordinarily commands, don't expect fancy tricks**, for details you better head to the official **[git reference][1]**. Some time ago I posted **[git basics][2]** to get started, but it should be still valid.

#### working with staging area

    $ git stash # stash all actual changes 'away'!
    $ git pop   # apply the stashed changes again

#### add/change the last commit

    $ git add .
    $ git commit --amend

#### working with branches

    $ git cherry-pick 3e2734c52 #select a commit from a other branch

#### working with remote

    $ git push origin :featureA # remove feature branch A

#### pushing to production (@heroku)

    $ git push -f heroku staging:master
    $ heroku run rake db:migrate


  [1]: http://git-scm.com/docs
  [2]: /ruby/2010/01/29/git-basics.html
