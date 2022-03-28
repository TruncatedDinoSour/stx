# Stx

> A startx alternative

# Why

Idk I was bored and the header of `/usr/bin/startx` says:

```
This is just a sample implementation of a slightly less primitive
interface than xinit. It looks for user .xinitrc and .xserverrc
files, then system xinitrc and xserverrc files, else lets xinit choose
its default. The system xinitrc should probably do things like check
for .Xresources files and merge them in, start up a window manager,
and pop a clock and several xterms.

Site administrators are STRONGLY urged to write nicer versions.

```

I'm not a site admin but you know, I wanted something cleaner and nicer

# How do I install it

## Manual

```sh
su -c 'install -Dm755 stx /usr/local/bin'
```

# Usage

```sh
stx
```

# Environment variables

- `XC` -- stx configuration (the same as `.xinitrc`)
- `X` -- the X server (default is `/etc/X11/xinit/xserverrc`)
- `STX_LIB` -- if you want to use stx as a lib (set it to anything you want)

# Configuration

Default config file is in `~/.config/stx_init.sh` it's configured
the same way as xinitrc,

For example:

```sh
#!/usr/bin/env sh

userresources=$HOME/.Xresources
usermodmap=$HOME/.Xmodmap
sysresources=/etc/X11/xinit/.Xresources
sysmodmap=/etc/X11/xinit/.Xmodmap

# Make sure this is before the 'exec' command or it won't be sourced.
[ -f /etc/xprofile ] && . /etc/xprofile
[ -f ~/.xprofile ] && . ~/.xprofile

# merge in defaults and keymaps

if [ -f $sysresources ]; then
    xrdb -merge $sysresources
fi

if [ -f $sysmodmap ]; then
    xmodmap $sysmodmap
fi

if [ -f "$userresources" ]; then
    xrdb -merge "$userresources"
fi

if [ -f "$usermodmap" ]; then
    xmodmap "$usermodmap"
fi

# start some nice programs

if [ -d /etc/X11/xinit/xinitrc.d ]; then
    for f in /etc/X11/xinit/xinitrc.d/?*.sh; do
        [ -x "$f" ] && . "$f"
    done
    unset f
fi

# execute our WMs and nice some utilities

exec dwm
```
