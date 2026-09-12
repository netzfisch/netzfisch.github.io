---
layout:    post
category:  linux
tags:      application ubuntu vmware bash 2cent 
title:     VMWare Workstation 6.5.3 Installer hangs
---
The installation of VMware-Workstation-6.5.3-185404.i386.bundle at Ubuntu Karmic 9.10 hangs. Therefore do

pre(terminal). $ sudo -i
$ while true; do killall -9 vmware-modconfig-console; sleep 1; done

In a second console do

pre(terminal). $ sudo ./VMware-Workstation-6.5.3-185404.x86_64.bundle --ignore-errors

If you get some errors, that you do not have sufficient rights, do

pre(terminal). $ sudo rm /etc/vmware/not_configured
$ sudo vmware-modconfig --console --install-all
$ sudo /etc/init.d/vmware restart 
