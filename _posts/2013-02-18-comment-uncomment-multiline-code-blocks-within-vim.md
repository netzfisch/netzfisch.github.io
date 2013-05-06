---
layout: post
category: tools
tags: 2cent application vim
title: Comment/Uncomment multiline code blocks with VIM
---
I did not **use VIM** for a looong time and forgot nearly everything ... feels like learning again from ground up!

For **commenting a block of code** in [VIM](http://vim.org), go to the first character of the line, press `Ctrl + v`, then select until the last line (e.g. `Shift + g`) and press `I + #` to insert new or `c + #` to replace first character with the **hash-symbol** on all selected lines.

To uncomment put your cursor on the first hash symbol, press `Ctrl + v`, and go down until the last commented line and press `x`, which will delete all hash-characters vertically.

Alternatively configure your .vimrc:

    vmap ,ic :s/^/#/g<CR>:let @/ = ""<CR>
    map  ,ic :s/^/#/g<CR>:let @/ = ""<CR>
    vmap ,rc :s/^#//g<CR>:let @/ = ""<CR>
    map  ,rc :s/^#//g<CR>:let @/ = ""<CR>

So you can press `,ic` to insert and `,rc` to remove comments in normal and visual mode.
