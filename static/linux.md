---
layout:    default
tags:      devops system 2cent
title:     linux cheat sheet, things to remember
---
### linux cheat sheet

Regular linux systems have to be configured ... sometimes on runs **later** in same problem and can not remember how to handle the command line!

Therefore I note a this **wiki wiki page** some **plain and ordinarily commands, don't expect fancy tricks** as it is more of a **mnemonic** for me. For details check `rails --help` or `rails generate --help`!

#### snap

free memory by deleting unused snap packages

    $ snap list --all | awk '/deaktiviert|disabled/{print $1, $3}' | while read snapname revision; do sudo snap remove "$snapname" --revision="$revision"; done

#### system

### wine

    $ rm -R .wine

