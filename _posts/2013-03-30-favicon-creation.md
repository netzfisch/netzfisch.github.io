---
layout:    post
category:  2cent
tags:      bash
title:     easy favicon creation
---
In the old days, you could just resize to 16x16 pixel, but todays browsers support up to 64x64 pixel and also royalty free .png-format!

Easy come:

    $ sudo apt-get install imagemagick
    $ convert input.png -colors 256 -resize 64x64 favicon.ico

easy go! 
