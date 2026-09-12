---
layout:    post
date:      '2010-05-26'
category:  linux
tags:      application ubuntu wine elster config 2cent
title:     Steuererklaerung mit Ubuntu 10.4 (Lucid Lynx)
---
... mit "wine":http://www.winehq.org/ läuft "ElsterFormular":https://www.elster.de/elfo_home.php unter "Ubuntu 10.4. (Lucid Lynx)":http://ubuntu.com tadellos. Nur der Aufruf einzelner Formulare ist quälend langsam, deshalb

pre(terminal). $ rm /usr/shrare/wine/fonts/vga.sys

Für das "ElsterOnline":https://www.elsteronline.de/eportal/ Zertifikat wird dann noch die Java-Version von SUN im Firefox-Browser benötigt, die wegen des Kaufs durch Oracle im Partner-Repository zu finden ist

pre(terminal). deb http://archive.canonical.com/ lucid partner

dann

pre(terminal). $ sudo apt-get install sun-java-jre sun-java-plugin
$ sudo dpkg -P openjdk-*
