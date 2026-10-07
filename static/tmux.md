---
layout:    default
tags:      development 2cent application tmux config
title:     tmux cheat sheet, things to remember
---
## tmux cheat sheet

Working on a remote box over a shaky SSH link I want the shell to **keep running** when the connection drops - that is what **tmux** is for: detach, log out, come back later and reattach.

As the other sheets, this is a **mnemonic** with the plain commands, details are in the **[tmux manual][1]**.

### keys

The default **prefix** is `Ctrl+b`: press and **release** it, then the command key.

| what it does | keys |
| :--- | :--- |
| detach the whole session, all windows and panes | `Ctrl+b d` |
| split, new pane below | `Ctrl+b "` |
| split, new pane beside | `Ctrl+b %` |
| move between panes | `Ctrl+b` arrow |
| next pane / zoom pane on and off | `Ctrl+b o` / `Ctrl+b z` |
| resize pane | `Ctrl+b Ctrl+`arrow |
| new window ("tab") / next / previous | `Ctrl+b c` / `n` / `p` |
| pick a session / rename it | `Ctrl+b s` / `Ctrl+b $` |
| close pane, asks first | `Ctrl+b x` |
| clock mode, leave with `q` | `Ctrl+b t` |

A **big clock** in the pane means `t` was hit instead of `d`. If the prefix keeps misfiring, `tmux detach` from any pane does the same.

### sessions

    $ tmux new -s main          # start a named session
    $ tmux ls                   # list running sessions
    $ tmux attach -t main       # reattach by name, or just: tmux a
    $ tmux new -A -s main       # attach if it exists, otherwise create

**Detaching closes nothing**, every window and pane keeps running with its scrollback. Closing does: the last pane ends the window, the last window ends the session.

Attach on every SSH login, in `~/.zshrc`:

    if [[ -z "$TMUX" && -n "$SSH_CONNECTION" ]]; then
      tmux new-session -A -s main
    fi

### reboot

A **reboot kills all sessions**. The plugins [tmux-resurrect][2] and [tmux-continuum][3] bring back windows, panes, layout and working directories, not the state of running processes. Install them via [TPM][4]:

    $ git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm

and add to `~/.tmux.conf`:

    set -g @plugin 'tmux-plugins/tpm'
    set -g @plugin 'tmux-plugins/tmux-resurrect'
    set -g @plugin 'tmux-plugins/tmux-continuum'
    set -g @continuum-restore 'on'
    run '~/.tmux/plugins/tpm/tpm'    # keep at the very bottom

Reload with `tmux source-file ~/.tmux.conf`, then `Ctrl+b I` installs the plugins. `Ctrl+b Ctrl+s` saves, `Ctrl+b Ctrl+r` restores, continuum saves every 15 minutes by itself.

### prefix

Check the current one with `tmux show-options -g prefix`. Many switch to `Ctrl+a`, which costs the shell's jump to start of line - that then needs `Ctrl+a` twice:

    unbind C-b
    set -g prefix C-a
    bind C-a send-prefix

### alternative

[Omarchy][5] goes further and ships a whole tmux setup: prefix `Ctrl+Space` and shell functions which build a dev layout in one command - `tdl` puts editor, AI agent and terminal side by side, `tsl 4 c` opens a grid of four agents.

  [1]: https://man7.org/linux/man-pages/man1/tmux.1.html
  [2]: https://github.com/tmux-plugins/tmux-resurrect
  [3]: https://github.com/tmux-plugins/tmux-continuum
  [4]: https://github.com/tmux-plugins/tpm
  [5]: https://omarchy.org/manual/terminal/#tmux
