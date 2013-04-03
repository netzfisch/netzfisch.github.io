---
layout:    post
category:  linux
tags:      application imagemagick bash 2cent
title:     Easy favicon creation
---
In the old days, you could just resize to 16x16 pixel, but todays browsers support up to 64x64 pixel and also royalty free .png-format!

Easy come:

    $ sudo apt-get install imagemagick
    $ convert input.png -colors 256 -resize 64x64 favicon.ico

easy go! 
