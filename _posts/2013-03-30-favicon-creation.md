---
layout:    post
tags:      bash command
category:  2cent
title:     Easy Favicon Creation
---
In the old days, you could resize to 16x16 pixel, but todays brwosers support up to 64x64 pixel!

Easy come:

    $ sudo apt-get install imagemagick
    $ convert input.png -colors 256 -resize 64x64 favicon.ico

easy go! 
