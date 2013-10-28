---
layout:    post
category:  tools
tags:      termit google translate bash mashup
title:     Translating besides VIM in your terminal - get ready for Termit!
---

Regulary looking for a certain englisch word, verb or noun, which really fits your model methods - database symantics? Until last week I just openend annother browser tab an asked [dict.cc](http://dict.cc) or similar services ...

Since than I do **[Termit](htpps://github.com/pawurb/termit)**, which basically is an easy way/ shortcut to use [Google Translate](https://translate.google.com) directly in your terminal.

### Usage

While adding the `-s` parameter gives you additionaly synonyms:

    $ termit de en "anpassen" -s
    => adjust
    => Synonyms: adjust, adapt, customize, fit, suit, accommodate, tune, readjust, blend in, fit on, bring into line
    
the talk `-t` switch even dares you a voice impression of chinese:

    $ termit de zh "netzfisch" -t
    => 鱼网

This is a damn **simple, helpful and a defintely cool tool mashup**!

### Install

    $ gem install termit
    $ sudo apt-get install mpg123
