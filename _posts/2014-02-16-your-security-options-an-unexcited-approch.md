---
layout:    post
category:  linux
tags:      linux security encryption passwords
title:     Your security options - an unexcited approach
---
[Jan Krutisch][1] announced on the january [Ruby User Group][2] a security focus for the february event. So I started collecting some of my experiences ... Sadly I didn't made it in time for last week's RUG at [Jimdo][3]. But this way I can sum up some learnings and links.

Security might drive you mad those days, so don't think the way "**I have to secure everything**" but look for a **certain aim you** want to fulfill, e.g. 

* I want to secure my files 
* I want to send safe mails
* I want to browse safely
* I want to secure my blog
* ..

because "**security itself is just an abstract construct** and concrete aims will better stick to your brain" (see [MeierOnline][4]'s slides). That way psychology won't stay in your way! Basically you have the following options, which will help to focus you.

# Computer

Let's start with different grades of system security, either **encrypt**

* the **whole system** while installing check the appropriate options or do it [afterwards][5],
* the [home directory][6] or
* just add an [encrypted folder][7].

The last is really fast done, e.g. install **EncFS** `$ sudo apt-get install cryptkeeper encfs` and than create with the cryptkeeper GUI an encrypted directory.

For a good overview see also the [Electronic Frontier Foundation's notes][8], which are a bit older but not outdated!

# Network

When logged in public wireless lan use common **VPN Services** or pimp your [FRITZ!Box][9] and observe with [IP Schwein][10] the change!

### Mail

Use [GnuPGP][11] for signing and **encrypting emails**.

### Browser

Install **security extensions** which will help you

* to avoid pixel tracking, use [Ghostery][12] and
* to encrypt your communications with many major websites, making your browsing more secure, use [HTTPS Everywhere][13].

# Passwords

When I first explained my kids the "idea of passwords" they replied immediately: "Than we use the the string "**key**" as password - as it locks something away!" Oops ...

### Safe Passwords

Just **two simple rules**

* Use erverywhere different passwords, with at least 8 better 10 digits and some unordinary signs.
* Generate **random passwords** or build sentences to remember self made ones: "**T**his **i**s **m**y **1**st **a**wesome **&** **r**eally **s**afe **P**assword **!**" => "**Tim1a&rsP!**"

Than find yourself a location to

### ... access them from everywhere

Regardless where you are, for sure you will need some **passes**. I like the **cross plattform password manager [KeePassX][14]**, which works on Linux, Mac, Android ([KeePassDroid][15], [Keepass2Android][16]) and is Open Source. But there are many others!

To install the latest greatest with support for the **KeePass2 database format (.kdbx)**, build yourself via the [github repo][17] or get a [debian package][18] from the KeePassX developers team:

    $ sudo add-apt-repository ppa:keepassx/daily
    $ sudo apt-get update
    $ sudo apt-get install keepassx

Then "mashup" and put the database file in your trusted home [ownCloud][19] or cloud drive ([Google Drive][20]/[Dropbox][21]) and you're **always on**.

Alternatively - if you don't need any GUI - use [VIM as your Password Manager][22].

# Website Encryption (SSL)

See Ben's introduction to [client side certificates][24] and Jan's blog post "[Going Full Encryption][23]" with the associated [slides][25] - no more to say!

---

Further infos and links on the [February RUG][26] page, over and out!

[1]: http://jan.krutisch.de
[2]: http://hamburg.onruby.de
[3]: http://jimdo.com
[4]: http://meier-online.com/2014/02/risiko-sicherheit-und-menschliche-entscheidungsfindungen/
[5]: http://wiki.ubuntuusers.de/System_verschl%C3%BCsseln
[6]: https://help.ubuntu.com/community/EncryptedHome
[7]: https://help.ubuntu.com/community/FolderEncryption
[8]: https://www.eff.org/deeplinks/2012/11/privacy-ubuntu-1210-full-disk-encryption
[9]: http://www.avm.de/de/Service/Service-Portale/Service-Portal/index.php?portal=VPN
[10]: http://ipschwein.de
[11]: http://wiki.ubuntuusers.de/GnuPG
[12]: http://www.ghostery.com/
[13]: https://www.eff.org/Https-everywhere
[14]: http://www.keepassx.org/
[15]: https://play.google.com/store/apps/details?id=com.android.keepass
[16]: https://play.google.com/store/apps/details?id=keepass2android.keepass2android
[17]: https://github.com/keepassx/keepassx
[18]: https://launchpad.net/~keepassx/+archive/daily
[19]: http://owncloud.org/
[20]: https://drive.google.com
[21]: https://www.dropbox.com
[22]: http://stelfox.net/blog/2013/11/using-vim-as-your-password-manager/
[23]: https://jan.krutisch.de/en/2014/01/06/going-full-encryption.html
[24]: http://ben.rexin.at/clientcert-slide/#/
[25]: https://speakerdeck.com/halfbyte/ssl-deployment-best-practices
[26]: http://hamburg.onruby.de/events/ruby-usergroup-hamburg-februar-2014
