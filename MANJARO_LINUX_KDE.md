## Manjaro on MacbookPro 2013 11,3

These docs are mostly for me, so that my struggles in getting my environment running on mutliple linuxes is not something that I will forget.  I can't stand running into the exact same problems I solved 6 months ago and needed to spend just as much time solving them.  

### Overview

So far, I really like this Linux.  It's based on Arch, that's nice.  It's pretty simple, that's nice because, even though I really like low-level distro's like Arch and Gentoo, I really just don't have time to !@#$ with it.   

However, there's two things that are really irking the !@#$ out of me.  (1) The lock-screen bug, that prevents me from logging back into KDE.  (2) It just doesn't play nice with my custom keyboard.  I can't remap modkeys and function keys because of how the window manager intercepts keypresses.  I'll describe these issues more below.

> Note: i downloaded a more recent version of the KDE Manjaro release.  
> There are more stable release ISO's available which were about a month older.  
> It would have been wiser to have gone with one of these.  There were others on 
> IRC complaining about the lockout bug. This seemed to be new for Manjaro and it 
> did affect more than just the KDE version: the user complaining was using XFCE.  

I really liked the look & feel and configurability of KDE.  And i really liked Manjaro.  I would have tried a more stable version, but just don't have time right now.  The unfortunate reality is that, despite being free, Linux is a rich man's OS.  It's for someone who has the excess **time** and/or **hardware**.

### Install

went pretty smooth.  i opted for the non-free install, so when i booted up, the nvidia drivers were already in place.

the installer had an option to not install a bootloader, which was fantastic.  i'm using rEFInd, so i need to move over the bootloader and init ramdisk, then manually update refind.conf AFAIK.  if there's any convenient automatic way of doing this that'd be great.

### NVidia

Didn't really have to do anything :]

### Wireless

Ugh.  Broadcom.  It wouldn't be so irritating if it wasn't wireless.  Finding an ethernet jack whenever I've needed to start a new install has been a real pain this week.  But once the install was complete, it was fairly simple:

```shell
sudo pacman -S linux-headers broadcom-wl/6.30.223.248
sudo systemctl install 
```

everything was working fine, no restart or init ramdisk changes necessary.  

### Initial Update

`sudo pacman -Syu` - updated everything.  simple, no problems updating. didn't need to update the init ram disk. took forever though.

### Lockscreen

can't log back in after KDE locks.  this is so frustrating.  it kept happening when i left my pacman update unattended, so this took hours for me to complete.  i kept having to restart my laptop.

### Wireless


### xkb

Probably the most frustrating part of using KDE for me.  
- KDE is intercepting keystrokes somewhere outside of X11 ... or something.  
- E.G. the Yakuake terminal opens when i hit F12, instead of typing a plus (+).  
- So i had to disable this shortcut with my custom keyboard.
- These global keyboard shortcuts are being intercepted before being parsed by Xorg

After doing more troubleshooting, i'm able to get the basic digimon layout working 
- that is, without swapping the Command & Control keys.  
- So i'm able to get the `io(basic)` keyboard layout working, but i can't add the broken layout at all
  - into the layouts rotation in the "Keyboard Hardware and Layout" configuration menu at all. 
  - If I do, then the Super/Control modkey shortcuts break for all applications!  
- It's ridiculously confusing, but I think it's happening because KDE Plasma interoperates with Xorg slightly differently.

Also, I'm looking into "Dreymar's Big Bag of Keyboard Configs" 
- which is like an amazing set of example configs for XKB.  
- I'm going to dig more into this later.  
- I think I have an incomplete keyboard config which just happens to work in Gnome/Unity/Cinnamon, 
  - but doesn't in KDE for whatever reason. 
- I can take a look at these examples, but for now, as log as ESC & CAPS are swapped 
  - and my Digimon layout is intact, I can deal with the modkeys being in the wrong position.  
  - For a time anyways, my pinky can't deal with it very long though

### Building software on Manjaro

#### Arch Build System

haven't starting using it, but `abs` is a ports-like system for building software from source.  

to download: `sudo pacman -S abs` ... on second thought, this doesn't seem to work in manjaro

### Manjaro Tools

```shell
sudo pacman -S base-devel
sudo pacman -S manjaro-tools-base manjaro-tools-pkg
```

these are tools used for building packages to be distributed on manjaro (i think?)

### Weechat

building weechat on manjaro. providing this as an example of building, instead of pulling down with pacman.

weechat has the following dependencies:
- CMake
- libncurses
- libcurl
- zlib
- libgcrypt

optional dependencies:
- i18n: gettext
- SSL: gnutls, ca-certificates
- spellchecking: aspelll
- scripting: perl, python, etc...
- docs: ...
- tests: ...

```shell
# install deps
sudo pacman -S ncurses # does this provide the lib? 
sudo pacman -S cmake libcurl-gnutls libcurl zlib libgcrypt

# install optional deps
sudo pacman -S gettext aspell gnutls ca-certificates aspell
```

now build

```shell
# get weechat code
git clone git@github.com:weechat/weechat ~/src/weechat
cd ~/src/weechat

# build
mkdir build
cd build
cmake ..
make
make install
```

still can't quite get my weechat config to work correctly with Linux.  all the color configs get messed up.


### emacs

My emacs configs require version 25
- pacman only offers 24.5, AFAIK

so to build emacs 25, I installed version 24.5 from pacman to get most of the dependencies.  

- This works on ubunto with `sudo apt-get build-dep emacs`
  - this provides the os with all the deps, so you don't have to build them independently

```shell
# get emacs 24.5 for dependencies
sudo pacman -S emacs

# ensure you have autoconf
sudo pacman -S autoconf

# there may be other build dependencies, not sure
# equivalent to build-essential?
```

then clone the emacs codebase:

```shell
git clone git@github.com:emacs-mirror:emacs ~/src/emacs
cd ~/src/emacs
```

now build:

```shell
./autogen.sh
./configure

# if all clear,
make
make install
```

when running make, i ran into problems where `recipe for target 'bootstrap-emacs' failed` when running into a segfault.  trying installing the base-devel tools with pacman, but still couldn't get it working.

since that wasn't working, i needed to install it from AUR, which is a good chance to document how that process works.  Basically, find your package on AUR, look for the clone URL, clone it and `cd` into the dir.  then, after inspecting the PKGBUILD & foo.install files for safety.

```shell
makepkg -sri
```

stll not working. same problems. no time.  missing `texinfo` (makeinfo) ?  what am i missing here?  it was failing on building lisp, but immediately after starting the bootstrap-emacs task

### macbook pro webcam

get deps

```shell
sudo pacman -S linux-headers kmod
```

get code

```shell
cd ~/src
git clone git@github.com:patjak/bcwc_pcie
cd bcwc_pcie
```

... on the other hand the docs in the bcwc_pcie wiki are more than sufficient.  i got the firmware extracted and the shit to build, but then ran into problems where the module didn't load, /dev/video wasn't created and no device was found

Couldn't get it running. This is likely because i didn't unload `bdc_pci` before inserting the `facetimehd`.  Also, i didn't try downloading the fireware from the apple support site, which was automatically taken care of on Ubuntu, when i ran `make` in the firmware folder.  Make didn't automatically request to download this on Manjaro, so I must be missing some software here.  

### In Summary

I hope to finish editing this doc soon and describe my experience using Manjaro on a Macbook Pro 2013 (11,3).  But for now, I've got to get a linux environment running, so I can code.
