---
layout:    post
category:  linux
tags:      lubuntu install backup tar dotfiles ruby
title:     Updated - lubuntu dev box quick setup
date:      2014-01-24
---
After years staying with my old laptop, downgrading the window manager from GNOME 3 back to GNOME 2 and finally to **LXDE**, to keep decent performance - while upgrading to new distro-releases, finally the point came: I had to change my beloved companion **[IBM ThinkPad T41][1]**! So, I sad good bye ;-( and hello :-) to a more powerful one!

But can you remember, what you installed in the past years, what to keep - what to move? It took me a while to get the same working state I had before, therefore I summarized the accomplished steps and put some love in my **dotfile organization** at [github][2].

In the last years, I got used to a fast and lightweight operating system, so I keep on using [lubuntu][3], as the core is based on Ubuntu, focuses on speed and energy-efficiency, and uses the minimal desktop LXDE. Perfect conditions to stay the next decade with my new gadget - hopefully.

So get your iso-image and copy it to a the USB-stick. Before installing the base system, be aware to change your BIOS boot order at startup via `<F1>` (persistent) or hit `<F12>` to choose your temporary boot device . 
While moving through the installer **make sure** to choose the **system encryption** option or encrypt at least your home directory!

Than backup your old home directory, see using [tar][4] details here:

    $ tar -cvpzf backup-home.tar --exclude=~/backup-home.tar --one-file-system /home/YourUserName

extract your backup on the new box

    $ tar -xvpzf backup-home.tar -C /home/YourUserName/backup

and copy your .ssh-folder from the backup directory to the root level of your home directory. Make sure only **YOU** can read/write to it `chmod -R 600 ~/.ssh` !

To install **[dropbox][5], [heroku CLI][6] and [foreman][7]** add additional repositorys

    $ sudo add-apt-repository "deb http://linux.dropbox.com/ubuntu saucy main"
    $ sudo add-apt-repository "deb http://toolbelt.heroku.com/ubuntu ./"

and the corresponding keys to your apt sources:

    $ sudo apt-key add https://toolbelt.heroku.com/apt/release.key
    $ sudo apt-key adv --keyserver pgp.mit.edu --recv-keys 5044912E

Afterwards update and spice up your system with the **essential terminal, and dev tools**:

    $  sudo apt-get update
    $  sudo apt-get install aptitude curl git git-man gitg gitk heroku-toolbelt mc nodejs vim vim-doc xfce4-terminal zsh zsh-doc

and some useful desktop apps:

    $  bogofilter chromium-browser dropbox geary gimp gkrellm gwakeonlan lubuntu-restricted-extras keepass2 shutter

Switch your shell `$ chsh -s /bin/zsh`, clone, and install your **dotfiles**, mine are [here][8].

Integrate your local NAS by changing `/etc/fstab` according to the hints at the [ubuntu wiki][9] and support your work flow by a music streaming service. I add the [google Linux Software Repositories][10], download and install the **[google-music-manager][11]**.

Finally install ruby via [rvm][12] or [rbenv][13] and commonly used gems, e.g.

    $ gem install jekyll rubocop termit

Happy hacking!

### UPDATE, 24th of January 2014

Meanwhile I run in some minor problems, which can be solved as follwed

**Autostart of dropbox** does not work out of the box in lubuntut. Therefore just add it manually to the autostart group: `echo "dropbox start -i" >> ~/.config/lxsession/Lubuntu/autostart`

I forgot a tool for **automated, regular backups**: `sudo apt-get install deja-dup`, similar to _Apples Time Machine_.

**Auto mounting the NAS** broke of with error message like

  * _mount error (101): Network is unreachable_ and
  * _mountall: mount /home/username/mountpoint [process ID] brach mit dem Status 32 ab_

This can happen if you use a **ssd hard disk**, which is much faster and tries to mount the NAS though networking is not ready yet. Therefore I added to the fstab the options `noauto,user`, so the __owner__ can mount after login manually

{% highlight linux-config %}
# mount Public NAS folder, like shared Music
//NAS/public/music /home/harm/Music cifs noauto,user,guest,uid=1000,iocharset=utf8 0 0

# mount Personal NAS folder, with the username
//NAS/username /home/harm/Archive cifs noauto,user,credentials=/home/username/.smbcredentials,iocharset=utf8,sec=ntlm 0 0
{% endhighlight %}

Alternatively use [autofs][14] or use a script like this

{% highlight bash %}
#! /bin/sh
mount <Mountpunkt1>
mount <Mountpunkt2>
{% endhighlight %}

Then `sudo chmod +x <Skript>` and start it also via the autostart group.

[1]: http://thinkwiki.de/T41
  [2]: https://github.com/netzfisch/dotfiles
  [3]: http://lubuntu.net
  [4]: https://help.ubuntu.com/community/BackupYourSystem/TAR
  [5]: https://www.dropbox.com/install?os=lnx
  [6]: https://github.com/heroku/heroku
  [7]: http://blog.daviddollar.org/2011/05/06/introducing-foreman.html
  [8]: https://github.com/netzfisch/dotfiles
  [9]: https://wiki.ubuntu.com/MountWindowsSharesPermanently
  [10]: http://www.google.com/linuxrepositories/
  [11]: https://play.google.com/music/listen#/manager
  [12]: https://rvm.io
  [13]: https://github.com/sstephenson/rbenv
  [14]: https://help.ubuntu.com/community/Autofs
