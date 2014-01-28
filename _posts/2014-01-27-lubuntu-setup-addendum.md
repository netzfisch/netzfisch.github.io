---
layout:    post
category:  linux
tags:      lubuntu install backup nas
title:     Lubuntu setup - addendum
---
After my last [lubuntu post][1] I run in some minor problems, which can be solved easily. For example the **autostart** function of **dropbox** does not work out of the box in lubuntu 13.10. Therefore add it manually to the autostart group:

`echo "dropbox start -i" >> ~/.config/lxsession/Lubuntu/autostart`

Additionally I forgot a tool for **automated, regular backups**: `sudo apt-get install deja-dup`, similar to _Apples Time Machine_.

**Auto mounting the NAS** broke of with error message like

  * _mount error (101): Network is unreachable_ and
  * _mountall: mount /home/username/mountpoint (process ID) brach mit dem Status 32 ab_

This can happen if you use a **ssd hard disk**, which is much faster and tries to mount the NAS though networking is not ready yet. Therefore I added to the fstab the options `noauto,user`, so the __owner__ can mount after login manually

{% highlight linux-config %}
# mount Public NAS folder, like shared Music
//NAS/public/music /home/username/Music cifs noauto,user,guest,uid=1000,iocharset=utf8 0 0

# mount Personal NAS folder, with the username
//NAS/username /home/username/Archive cifs noauto,user,credentials=/home/username/.smbcredentials,iocharset=utf8,sec=ntlm 0 0
{% endhighlight %}

Alternatively use [autofs][2] or use a script like this

{% highlight bash %}
#! /bin/sh
mount <Mountpunkt1>
mount <Mountpunkt2>
{% endhighlight %}

Then `sudo chmod +x <Skript>` and start it also via the autostart group.

[1]: {% post_url 2013-12-23-lubuntu-devbox-quick-setup %}
[2]: https://help.ubuntu.com/community/Autofs
