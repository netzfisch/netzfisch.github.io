---
layout: post
category: tools
tags: 2cent application vim
title: Comment - Uncomment multiline code blocks with VIM
---
I didn't use VIM for a looong time and forgot everything ... feels like learning again everything from ground up!

For commenting a block of code in [VIM](http://vim.org), go to the first character of the line, press <kbd>Ctrl-v</kbd>, then select until the last line and press <tt>c/I</tt> and <tt>#</tt>, wich will insert a "#-character" on all selected lines.

To uncomment put your cursor on the first **#** character, press **Ctrl-v**, and go down until the last commented line and press **x**, that will delete all the "#-characters" vertically.

Alternatively, put the following section in your .vimrc:

    vmap ,ic :s/^/#/g<CR>:let @/ = ""<CR>
    map  ,ic :s/^/#/g<CR>:let @/ = ""<CR>
    vmap ,rc :s/^#//g<CR>:let @/ = ""<CR>
    map  ,rc :s/^#//g<CR>:let @/ = ""<CR>

So I can press **,ic** to insert and **,rc** to remove comments in normal and visual mode.
