---
layout:    post
category:  linux
tags:      ubuntu rust coreutils uutils ruby asdf gem segfault 2cents
title:     Ruby segfaults after an Ubuntu update - blame the new install
---
Yesterday everything worked. Today, after a **glibc update** and a fresh
`asdf install ruby`, a tiny stdlib-only script died with **`[BUG] Segmentation
fault at 0x0000000000000008`** - arrgh! Neither glibc nor Ruby were guilty.
The culprit was **`/usr/bin/install`**, which on **Ubuntu 26.04** is no longer
the GNU tool but the **Rust rewrite from uutils** - and it silently **truncates
files** when the target is an **ecryptfs** home directory.

### The symptom

Every native gem extension compiled today crashed on `require`:

    $ ruby -e 'require "json"'
    .../gems/json-2.19.3/lib/json/ext/parser.so: [BUG] Segmentation fault at 0x0000000000000008

Ruby itself was fine - the **json 2.7.2 bundled with Ruby** loaded, psych and
openssl too. Only the gems built *after* the Ruby rebuild were broken, and all
of them: bigdecimal, prism, rbs, io-console, pg, puma, ... **45 extensions**
across two Ruby versions.

### Read the ELF, not the backtrace

`readelf` gave the game away:

    $ readelf -d parser.so
    readelf: Error: Reading 2560 bytes extends past end of file for section headers
    $ stat -c %s parser.so
    136088                      # header says the file should be ~250 KB

Half the file was **missing from the middle**, not the end - the section header
table was still there, just pulled forward. Something copied the linker output
and **dropped chunks** on the way. A quick way to find every damaged library:

    $ find ~/.asdf/installs/ruby -name '*.so' \
        -exec sh -c 'nm -D "$1" >/dev/null 2>&1 || echo "BROKEN $1"' _ {} \;

`nm` refuses a truncated ELF, `readelf` only warns - so use `nm` for the check.

### Finding the copier

Rubygems builds an extension with `make install`, and the generated Makefile
hardcodes **`INSTALL = /usr/bin/install -c`**. On my box that is

    $ readlink -f /usr/bin/install
    /usr/lib/cargo/bin/coreutils/install
    $ install --version
    install (uutils coreutils) 0.8.0

Rebuilding the same gem into `/tmp` (tmpfs) produced an intact library,
rebuilding it under `~/.cache` (ecryptfs) reproduced the crash byte for byte.
And copying a known-good file onto the home directory with each tool:

    GNU cp          OK
    GNU install     OK
    Ruby FileUtils  OK
    uutils install  DIFFER  136144 bytes

`strace` shows *why* - the Rust copy walks a fallback chain and **loses data
between two steps**:

    copy_file_range(...)          = -1 EXDEV
    sendfile(...)                 = -1 EINVAL
    splice(src -> pipe, 131072)   = 131072      # 128 KiB read into a pipe
    splice(pipe -> dst, 131072)   = -1 EINVAL   # ecryptfs has no splice_write
    write(dst, ..., 8192)         = 8192        # continues from the *advanced* offset

The 128 KiB sitting in the pipe are never written. On plain ext4 or tmpfs the
first call succeeds and nobody notices - which is why this **only bites on
ecryptfs** (and probably other filesystems without splice support).

### Why "yesterday it worked"

`rust-coreutils` had been installed for a month. Nothing had **compiled a
native gem since then** - until the Ruby rebuild reinstalled every gem in one
go. The glibc update the same week was a red herring: GNU `install`, Ruby and
the Rust binary all link the very same libc, and only one of them breaks.

### The fix: divert one binary

Don't swap the whole coreutils. On 26.04 `build-essential` **hard-depends on
`coreutils-from-uutils`**, so `apt install coreutils-from-gnu` refuses, and
forcing it with `--allow-remove-essential` removes `build-essential` and turns
your compiler into `apt autoremove` fodder. A **dpkg diversion** for the one
tool that is proven broken survives package upgrades and touches nothing else:

    $ sudo dpkg-divert --divert /usr/bin/install.uutils --rename /usr/bin/install
    $ sudo ln -s gnuinstall /usr/bin/install
    $ install --version
    install (GNU coreutils) 9.7

`cp` and `mv` already point to `gnucp` / `gnumv` by Ubuntu's own default, so
they need nothing.

**Revert** once uutils ships a fix:

    $ sudo rm /usr/bin/install && sudo dpkg-divert --rename --remove /usr/bin/install

**Check** whether a newer `rust-coreutils` is fixed before reverting:

    $ head -c 300000 /dev/urandom > /tmp/src
    $ /usr/bin/install.uutils -c /tmp/src ~/install-test && cmp /tmp/src ~/install-test && echo FIXED

### Rebuilding the gems

`gem pristine --extensions` **re-extracts but does not recompile** most gems -
learned that the hard way. Worse, if a broken extension is one that `gem`
itself loads (psych, via `~/.gemrc`) the command segfaults before doing
anything. Rubygems has a built-in escape hatch: a gem whose
`gem.build_complete` marker is missing is **ignored with a warning** and
rebuilt by

    $ E=~/.asdf/installs/ruby/3.3.9/lib/ruby/gems/3.3.0/extensions/x86_64-linux/3.3.0
    $ rm $E/<gem-version>/gem.build_complete      # for every broken gem
    $ gem pristine --only-missing-extensions

Run the `nm` loop from above again - no output means you are done.

### What did *not* work

* `PATH="/usr/bin/gnu:$PATH" asdf install ruby ...` - there is no such
  directory, and the Makefiles use the absolute path anyway. It "worked" for me
  only because the diversion was already in place.
* `alias install=/usr/bin/gnuinstall` - aliases never reach `make`.
* An `update-alternatives` loop over `/usr/bin/gnu*` - dpkg overwrites the
  link on the next upgrade, and the glob happily catches `gnumeric`.

Ubuntu's own [discussion thread][1] on the switch lists more corruption
reports for `cp`, `mv` and `rm` - keep an eye on that before trusting the
Rust tools on anything encrypted. The upstream project lives at [uutils][2].

[1]: https://discourse.ubuntu.com/t/an-update-on-rust-coreutils/80773
[2]: https://github.com/uutils/coreutils
