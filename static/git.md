---
layout:    default
tags:      developement git 2cent
title:     git cheat sheet, things to remember
---
### git cheat sheet

I'm not coding that often, but when the exception happens I often end up with a 'task' and the knowledge that there is a better way to handle the source code - but I can't remember!

Therefore I note here some **plain and ordinarily commands, don't expect fancy tricks** as it is more a **mnemonic** for me. Head for details better to the official **[git reference][1]**. Some time ago I posted some **[git basics][2]** to get started, which should be still valid!

#### snapshotting

    $ git add .
    $ git commit --amend

Replaces the tip of the current branch by creating a new commit. Be careful changing already pushed commits, because it rewrites the history.

#### branching / merging

    $ git stash
    $ git reset --hard # 87de91f
    $ git stash pop

Saves local modifications to a new stash, reverts to previous state/head (or to commit 87de91f) and finaly applys the stashed changes again.

#### sharing / updating

    $ git push origin :featureA

Removes branch 'featureA'.

    $ git push -f heroku staging:master
    $ heroku run rake db:migrate

Forces a pushing 'staging' branch to 'master' branch at heroku and runs afterwards a migration.

#### patching

    $ git cherry-pick 3e2734c52

Selects a commit '3e2734c52' from an other branch.

    $ git -n cherrypick master~1 master

Applys changes introduced by the second last and last commit pointed to by master, but do not create any commit yet.

  [1]: http://git-scm.com/docs
  [2]: /ruby/2010/01/29/git-basics.html
