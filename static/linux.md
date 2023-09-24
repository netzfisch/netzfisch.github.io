---
layout:    default
tags:      devops system 2cent
title:     linux cheat sheet, things to remember
---
## linux cheat sheet

Regular linux systems have to be re-configured ... sometimes one runs **later** in the same problems and can not remember what did the "trick"!

Therefore finde here some **plain and ordinarily commands, don't expect fancy tricks** as it is more of a **mnemonic** for me. For details check manual pages `man postgres`, `tldr wine`, etc.!

### low disk space

free memory by deleting unused files

    $ sudo apt clean && sudo apt autoremove -y
    $ snap list --all | awk '/deaktiviert|disabled/{print $1, $3}' | while read snapname revision; do sudo snap remove "$snapname" --revision="$revision"; done
