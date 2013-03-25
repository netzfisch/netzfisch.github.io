---
layout: post
title: Steuererklaerung mit Ubuntu 10.4 (Lucid Lynx)
date: '2010-05-26'
category: howto
tags: ubuntu wine elster application
---
... mit "wine":http://www.winehq.org/ läuft "ElsterFormular":https://www.elster.de/elfo_home.php unter "Ubuntu 10.4. (Lucid Lynx)":http://ubuntu.com tadellos. Nur der Aufruf einzelner Formulare ist quälend langsam, deshalb

{% highlight console linenos %}
  $ rm /usr/shrare/wine/fonts/vga.sys
{% endhighlight %}

Für das "ElsterOnline":https://www.elsteronline.de/eportal/ Zertifikat wird dann noch die Java-Version von SUN im Firefox-Browser benötigt, die wegen des Kaufs durch Oracle im Partner-Repository zu finden ist:
<code>deb http://archive.canonical.com/ lucid partner</code>

dann

```bash
$ sudo apt-get install sun-java-jre sun-java-plugin
$ sudo dpkg -P openjdk-*
```
