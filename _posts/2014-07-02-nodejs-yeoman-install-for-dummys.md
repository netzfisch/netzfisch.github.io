---
layout:    post
category:  linux
tags:      2cents ubuntu debian nodejs yeoman install
title:     Node.js & Yeoman install for dummys
---
If you are heading for a **Node.js install** into **Debian /Ubuntu**, you will
potentially face two issues concerning a **namespace collision** and generators
which want to **install globally**!

Looking at /usr/share/doc/nodejs/README.Debian, which tells you:

> The upstream name for the Node.js interpreter command is "node".  In Debian
> the interpreter command has been changed to "nodejs".
>
> This was done to prevent a namespace collision: other commands use the same
> name in their upstreams, such as ax25-node from the "node" package.
>
> Scripts calling Node.js as a shell command must be changed to instead use the
> "nodejs" command.

To fix this create a symnlink `$ sudo ln -s /usr/bin/nodejs /usr/bin/node` or
install the additionally nodejs-legacy package, which does that for you:

    $ sudo apt-get install nodejs nodejs-legacy npm

The other conflict might that the **node package manager** (npm) wants to
install further packages globally with `sudo`, which you might not like? When
you look at that excellent stackoverflow [post][1]

> you can enable npm to install packages globally without breaking out of $HOME
> by setting a local node prefix:
>
>     $ echo prefix = ~/.node >> ~/.npmrc

Than adjust your $PATH, to point to the new installation destination for global
node executables:

    $ export PATH=$HOME/.node/bin:$PATH

to your ~/.zshrc. After that, you can install Yeoman by running

    $ npm install -g yo

without sudo or permission conflicts and the opportunity to start from scratch,
when something is completely broken. Therefore just remove your ~/.node
directory.

Find other interesting ways to **install node and npm without having to sudo**
in this [gist][2] or look at [that][3] detailed discription.

[1]: http://stackoverflow.com/a/18277225
[2]: https://gist.github.com/isaacs/579814
[3]: https://www.digitalocean.com/community/tutorials/how-to-install-node-js-on-an-ubuntu-14-04-server
