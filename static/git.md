---
layout:    default
tags:      development git 2cent
title:     git cheat sheet, things to remember
---
## git cheat sheet

I'm not coding that often, but when the exception happens I often end up with a 'task' and the knowledge that there is a better way to handle the source code - but I can't remember!

Therefore I note here some **plain and ordinarily commands, don't expect fancy tricks** as it is more a **mnemonic** for me. Head for details better to the official **[git reference][1]**. Some time ago I posted some **[git basics][2]** to get started, which should be still valid!

Instead of re-writing **git man-pages** I organized it by thinking in **workflows**

### commit

Add all files to the **staging area** and comment the change

    $ git add .
    $ git commit -m "change awesome things"

or in one line:

    $ git commit -a -m "change awesome things"

To add annother 'change' to the last commit, do:

    $ git add other_file
    $ git commit --amend

This replaces the tip of the current branch by creating a new commit, so **be careful** changing already pushed commits. In that case you have to **force the push**, because it **rewrote the history**.

To **undo** the last commit and redo - better do:

    $ git reset --soft HEAD^
    $ vim my_file
    $ git commit -a -c ORIG_HEAD

or **revert** to previous state/head, e.g. to commit 87de91f

    $ git reset --hard 87de91f

Finally save local modifications to a **stash** ... do something not related ... and finally re-apply the stashed changes.

    $ git stash
    $ git stash pop

#### patch from other branch

Select a commit '3e2734c52' from an other branch.

    $ git cherry-pick 3e2734c52

Apply changes introduced by the second last and last commit pointed to by master, but do not create any commit yet.

    $ git -n cherrypick master~1 master

To get the remote with commits not present in your local branch AND vice versa
in sync do `git rebase origin master`.

#### patch from other repo

Extract two topmost commits from the current branch and format them as **email-able patches**:

    $ git format-patch -2

Apply a series of patches from a mailbox or a given one, e.g. '0001-use-unicorn-via-procfile'.

    $ git am 0001-use-unicorn-via-procfile.patch

**Fall back** on **3-way merge** if the patch does not apply cleanly:

    $ git am --3way 0001-use-unicorn-via-procfile.patch

### branch

#### merge

Create a **new branch**, merge and delete it:

    $ git checkout -branch featureA
    $ git merge featureA
    $ git branch --delete featureA  # remove localy
    $ git push origin :featureA     # remove remote

If this does not work, because meanwhile master got further commits

    $ git mergetool featureA

might help or try to solve conflicts by:

    [featureA]$ git rebase -interactive master
    [featureA]$ git checkout master
    [master]$ git merge --no-ff featureA

#### share / update

If **push** is **rejected**, because of a **non-fast-forward** merge, force it

    $ git push -f origin featureA

or use the ``-f`` to **overwrite remote-master with featureA-branch**

    $ git push -f heroku featureA:master

#### list / rename

    $ git branch --all              # List all branches
    $ git branch --remote           # or only remote ones.
    $ git branch --move <old> <new> # Rename old to new.

  [1]: http://git-scm.com/docs
  [2]: /ruby/2010/01/29/git-basics.html
