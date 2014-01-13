---
layout:    default
tags:      developement git 2cent
title:     git cheat sheet, things to remember
---
### git cheat sheet

I'm not coding that often, but when the exception happens I often end up with a 'task' and the knowledge that there is a better way to handle the source code - but I can't remember!

Therefore I note here some **plain and ordinarily commands, but don't expect fancy tricks** as it is for me more a mnemonic. For details you better head to the official **[git reference][1]**. Some time ago I posted also some **[git basics][2]** to get started, which should be still valid!

#### branching and merging

    $ git stash        # save local modifications to a new stash
    $ git reset --hard # revert to previous state/head
    $ git stash pop    # apply the stashed changes

### patching

    $ git -n cherrypick master~1 master

Apply the changes introduced by the second last and last commits pointed to by
master, but do not create any commit with these changes.

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
