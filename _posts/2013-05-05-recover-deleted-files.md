---
layout:    post
category:  linux
tags:      application foremost photorec bash 2cent
title:     Recover deleted files
---
Ever **accidently deleted files** with `Shift + Del` or directly at the command line. Then **stop** any **further writings** to the disk and try the good old **command line programs foremost or photorec**. Both are at its best already installed, otherwise that is quickly done by

    $ sudo apt-get install foremost testdisk

**photorec** is included in the testdisk utility and a more **guided solution**, although still running in the bash. While **foremost** works straight foward at the command line: 

    $ man foremost
    $ foremost -t pdf -i /dev/sda1 -T
    $ cat output/audit.txt
    $ ls -l output/pdf/
