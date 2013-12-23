---
layout:    post
category:  linux
tags:      lubuntu install backup tar dotfiles
title:     lubuntu dev box quick setup
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

Than spice up your system with the **essential terminal, and dev tools**:

    $  sudo apt-get install aptitude curl git git-man gitg gitk mc vim vim-doc xfce4-terminal zsh zsh-doc
    
and some nice desktop apps:

    $  bogofilter chromium-browser dropbox geary gimp gkrellm gwakeonlan lubuntu-restricted-extras keepass2 shutter

Switch your shell `$ chsh -s /bin/zsh`, clone, and install your **dotfiles**, mine are [here][5].

Integrate your local NAS by changing `/etc/fstab` according to the hints at the [ubuntu wiki][6] and support your work flow by a music streaming service. I add the [google Linux Software Repositories][7], download and install the **[google-music-manager][8]**.

Finally install [rvm][9] and some often used gems

    $ gem install jekyll termit

Happy hacking!

  [1]: http://thinkwiki.de/T41
  [2]: https://github.com/netzfisch
  [3]: http://lubuntu.net
  [4]: https://help.ubuntu.com/community/BackupYourSystem/TAR
  [5]: https://github.com/netzfisch/dotfiles
  [6]: https://wiki.ubuntu.com/MountWindowsSharesPermanently
  [7]: http://www.google.com/linuxrepositories/
  [8]: https://play.google.com/music/listen#/manager
  [9]: https://rvm.io/
