## Ubuntu 15.10 Install

This is all installed on a 15" MacbookPro 2013.  That's the `11,3`,
which was released in October 2013. this will be nearly the identical
config for the `11,2` as well, although I think the `11,1` is missing
an nvidia graphics card .. maybe?  i donno.  it will be mostly the
same the `12,1` and `12,x` series. honestly .. i'm just bullshitting
here and trying to cover my SEO bases lol.

### initial software

download build stuffs

```shell
sudo apt-get install build-essential cmake autoconf git vim
```

copy ssh keys and `ssh-add ~/.ssh/id_rsa`

setup git config:

```
git config --global user.name "David Conner"
git config --global user.email "dconner.pro@gmail.com"
git config --global core.excludesfile ~/.gitignore_global

# after cloning .files:
ln -s ~/.files/git/gitignore_global.ubu ~/.gitignore_global
```

### privacy

Hit `Super` and type `privacy` and hit `Enter`.  Navigate to the
`Files & Applications` tab.  Turn it off.  If you're a programmer, you
don't need that.  Uncheck all the boxes.  Click 'Clear Usage Data'.

Use Find+Grep:
`grind() { find . -type f -exec grep --color -nH -e $1 {} +`

> One reason I love emacs: it uses mostly GNU tools and is magnificent
> in exposing it's interface to you to show you the options available
> and how it does things. Learn how to use apropos to discover it's
> functionality.

Next: Click the `Search` tab and tell it to exclude online results.
Also, you may want to exclude error reports.

Now, install `Unity Tweal Tool` with `sudo apt-get install
unity-tweak-tool`.  Open it.

There are some duplicate configurations here, but also more options.
To me, it's a bit irritating that the default Unity interface is so
simplified, but I guess I understand why.  However, for me personally,
I like all my configuration for an OS/App `in one place` and
`searchable`.  IntelliJ and Android are two excellent examples of
configuration UI's that are robust and easily discoverable.


### loxodo password safe

It is a small tragedy, but there are no pwsafe managers for linux that:

#### (1) I trust.  A password manager written in high-level language such as TCL really needs some thought.
#### (2) Build easily without sketchy dependencies

loxodo is a small command-line and GUI capable system for opening
password files.

```shell
# i've got this one memorized sudo apt-get install
git@github.com:sommer/loxodo ~/src/loxodo
```

now put your password file where you want it and keep it on USB. also,
you can keep loxodo on USB and load it as needed. luks that ish.

`./loxodo.py -i [psafe3file]` to run in interactive mode.  this can
also be run in GUI mode, which I feel is less trustworthy.  close the
app when you're not using it.  you can set a shortcut to open it, but
be careful.

installing loxodo with GUI:

```shell
sudo apt-get install python-wxgtk2.8
cd ~/src/loxodo
sudo ./setup.py install
```

### dotfiles

clone dotfiles

```shell
git clone git@github.com:dcunited001/dc.files ~/.files
cd ~/.files
git submodule init
git submodule update # and if you're not me, my repos from bitbucket will fail to d/l
```

run init scripts with `./init/ubu.sh` and follow instructions. this
isn't is a complete config, but it's close enough.  some of the stuff
is outdated.  i don't completely configure vim any more.  the emacs
init scripts are also outdated.  i mostly just use it to set up links
for zsh.

```shell
./init/ubu.sh
```

link emacs.  i now use `purcell/emacs.d`

```shell
ln -s ~/.files/emacs.d ~/.emacs.d
```

now link the customizations



download and update the thesaurus



### fonts

i like having symbola fonts and setting all fonts to deja vu w/
powerline.  these powerline patched fonts can be found in my dotfiles.
copying these fonts to `~/.fonts` should make them available to the
system.

```shell
cd ~/.files/
mkdir ~/.fonts
cp -R ~/.files/powerline-fonts/**/*.ttf ~/.fonts
```

now, just hit the super key, type fonts and hit enter.  that should
open the Font Viewer.  verify that everything is there.

### terminator profiles

```shell
sudo apt-get install terminator
```

Here, i just

TODO:

### HiDPI

instead of tweaking fonts for each setting, go to `Display` under
`System Settings` and change the font scaling to `1.5` or `2.0`,
whatever you prefer.  For most apps, this fixes most problems
for UI font size.  Emphasis on **MOST** !@#$ ! $@#! $@ $!.

Jetbrains IntelliJ (and Rubymine) was still acting up. The problem was
that UI fonts scaled properly, but not the code I was reading.  I
needed to create a custom theme in order to change it's font size.
I scaled it to 22 and this fixed my problems.

Emacs was also giving me shit.  So i needed to fix the fonts in that
application on a one-off basis as well.  Sorry, this is just dumb and
means that when i reinstall my system, i have two dozen random system
preferences to apply in random places in order to get things back to
the way the were.  And this makes me very very angry.

### xkb

yeh, turns out the problem was the format of xml (forgot to put
configItem tag inside variant tag)

- moved io to `/usr/share/X11/xkb/symbols`
- configured evdev.xml in `/usr/share/X11/xkb/rules/evdev.xml`
- swapped values for <CAPS> and <ESC> in `/usr/share/X11/xkb/keycodes/evdev`
- `echo 2 | sudo tee /sys/module/hid_apple/parameters/fnmode` to fix fn keys for session
- `echo options hid_apple fnmode=2 | sudo tee -a /etc/modprobe.d/hid_apple.conf` to fix function keys for subsequent sessions
- after updating `hid_apple.conf` need to copy this config to initramfs with `sudo update-initramfs -u -k all`
- after reboot, everything seems to work

### touchpad fixes

when you use a macbookpro outside of OSX, you really begin to
appreciate whatever magic apple works with those touchpad drivers,
because this shit sucks out of the box.  it works pretty well in
linux, i guess.  but you really need to get some custom drivers
running in Ubuntu, bc that shit drives me crazy.  i'm always
misclicking things and scrolling/highlighting and i enjoy the
multitouch drivers occasionally.

download `mtrack` & remove `synaptics`

```shell
gsettings set org.gnome.settings-daemon.plugins.mouse active false
sudo apt-get install xserver-xorg-input-mtrack
sudo apt-get autoremove xserver-xorg-input-synaptics
sudo rm /usr/share/X11/xorg.conf.d/50-synaptics.conf
```

configure `/usr/share/X11/xorg.conf.d/50-synaptics.conf`. example
config below.  more info found in
[Ubuntu docs for MacbookPro 11,1](https://help.ubuntu.com/community/MacBookPro11-1/utopic)

yeh, by the way, fucking mtrack stomped on my webcam.  so now i gotta figure out why that shit doesn't load...

wow, this took me 3 hours to configure and i still don't understand
what the values for `ClickFingerX` do... so pissed right now.

```
Section "InputClass"
        MatchIsTouchpad "on"
        Identifier "Touchpads"
        Driver "mtrack"
        Option "IgnoreThumb" "true"
        Option "ThumbSize" "25"
        Option "IgnorePalm" "true"
        Option "DisableOnPalm" "false"
        Option "BottomEdge" "30"
        Option "TapDragEnable" "true"
        Option "Sensitivity" "0.6"
        Option "FingerHigh" "3"
        Option "FingerLow" "2"
        Option "ButtonEnable" "true"
        Option "ButtonIntegrated" "true"
        Option "ButtonTouchExpire" "750"
        Option "ClickFinger1" "3"
        Option "ClickFinger2" "2"
        Option "ClickFinger3" "0"
        Option "TapButton1" "0"
        Option "TapButton2" "0"
        Option "TapButton3" "0"
        Option "TapButton4" "0"
        Option "TapDragWait" "100"
        Option "ScrollLeftButton" "7"
        Option "ScrollRightButton" "6"
        Option "ScrollDistance" "100"
EndSection
```

enable natural scrolling:

```
# in general, don't use xmodmap if you don't have to.  just don't.  use xkb
echo "pointer = 1 2 3 5 4 6 7 8 9 10 11 12" >> .Xmodmap
```

restart lightdm with `sudo restart lightdm` or `sudo service lightdm
restart`. it's seriously fucking lame to have to restart your window
manager to adjust mouse settings.  fucking stupid.

### emacs

i need emacs 25 for several Melpa package dependencies included in
`purcell/emacs.d`

deps

```shell
sudo apt-get build-dep emacs
sudo apt-get install texinfo libx11-dev libxext-dev libgtk-3-dev \
  libncurses5-dev libjpeg-dev libpng-dev libtiff5-dev libjpeg-dev \
  xaw3dg-dev libxmu-dev libxmuu-dev libxpm-dev libxt-dev libxtst-dev \
  libxv-dev libgif-dev

# libungif4-dev libxtrap-dev
# x-dev xlibs-static-dev
```

build

```shell
cd ~/src/emacs
./autogen.sh
./configure
make
make install
```

link configs

```shell
ln -s ~/.files/emacs.d ~/.emacs.d
```

link custom configs

```shell
ln -s ~/.files/emacs/init-local.el ~/.emacs.d/lisp/
```

copy thesaurus to ~/.files/emacs



### A brief detour into emacs

Open emacs and it should automatically download the necessary
packages.

```shell emacs ```

This will take about 5 minutes.  There's a lot of software.  This
makes me nervous.  If you didn't build or download `Emacs 25` the
elisp packages won't download and you're emacs will be baroque like a
harpsichord.

You'll want to configure a theme because the default one is ugly, so
hit `M-x`, type `customize-themes` and hit `Enter`.  This brings you
to the themes configuration panel.  Some themes are a security risk,
so it may be wiser to stick with some of the defaults.  Uncheck the
themes you don't want and check the one you do, this save the
configuration by navigating over the button and hitting enter.

I like the `Cyberpunk` theme, which I have to download.  If you need
to download a theme, hit `M-x list-packages` and `Enter`.  This lists
the packages available to download from Melpa.  Now hit `C-s` to begin
`I-search`.  Now, type `cyberpunk` and `Enter` to bring your curser
over the `cyberpunk-theme` line.  Now, hit `?` to see your options for
the list packages screen.  This works for most emacs menus.  Notice,
you mark packages with `i.  So, hit `q` to escape the packages screen.
If you're hovering over the `cyberpunk-theme` line, hit `i` to mark
it, then `x` to download all marked packages.  You'll also need to hit
`y` to accept them.

`Magit` is your best friend.  It's the emacs git package and you can
do anything.  Again, Magit exposes the git command line interface to
you and if you don't know that very well, you can easily learn it with
`Magit`.  You can send emails from `emacs`

If you want to learn Emacs (or Linux) the first thing you need to know
is how to discover information.  If you don't learn this, you're going
to have problems for a long, long time.  For Linux, this means
learning things like how to use `less` commands to search `man` pages.
Every time you hit Google to search StackOverflow, there is a way to
discover that information without the internet that is 20x faster!
Another thing to learn with Linux is where log files are stored and
where configuration files are stored.  A great quick tip for linux
beginners is to learn to search your history with `history | grep
XXXXX`.  This allows you to rediscover commands you've used before.

For Emacs, this means learning to use Apropos.  If you need to search
for a function, type `C-h a` to open apropos search.  Type the partial
name of the command you're looking for.  `EVERY BUFFER IS TEXT!` so
you can search anywhere with `C-s` to open `I-search.  If you run into
problems on a menu screen, hit `?` to see your options.  You can run
shell commands on `dired` screens, which contain file listings using
`!` and typing the command.

The greatest advantage of Emacs for programming is the REPL.  For
small programs in scripting languages, you can run a repl and
re-evaluate classes on the fly.  It gets a bit more complicated if
you're trying to build a module in python.  There are even greater
advantages to using the REPL if you're building a lisp like clojure
and with clojure, you can even connect to remote REPL's!

With the REPL, you can also dynamically re-evaluate a class and run
it's tests without having to re-evaluate your environment.  For large
frameworks like Ruby on Rails, this translates into MASSIVE TIME
SAVINGS.  But the configuration and learning curve of emacs often
prevents people from getting to that point.

Also, emacs is configured via `elisp`, which is an awesome
introduction to lisp and functional programming.  It's magical.  So by
learning emacs, you're learning to be a better programmer.  But often,
it's better for beginners to just use a common IDE.

Anyways, moving on... (i'll like convert the above to a blog or
something)

### wireless

`sudo apt-get install bcmwl-kernel-source`

restart after wireless finishes installing.  you may need to upgrade
your initramdisk.  this is done with.

### webcam

deps

```shell
uname -r # to identify kernel version for linux-headers
sudo apt-get install linux-headers kmod checkinstall
```

get source

```shell
git clone git@github.com:patjak/bcwc_pcie ~/src/bcwc_pcie
cd ~/src/bcwc_pcie
```

then build firmware.  `make` should download the drivers

```shell
cd firmware
make
sudo make install
```

build

```shell
cd ~/src/bcwc_pcie
make
```

Make sure you run `sudo modprobe -r bdc_pci` to remove the `bdc_pci`
module **before** running `checkinstall` or any other commands.
Otherwise, you'll run into some really irritating issues where the
`facetimehd` module is installed (so `checkinstall` won't do anything)
but not loaded.  In other words, you'll need to undo all changes from
the subsequent script block in order to reinstall everything properly
and end up with a `/dev/video` device.  If you don't have a
`/dev/video` you failz and `cheese` won't load with a webcam.

```shell
sudo checkinstall
sudo depmod
sudo modprobe facetimehd
```

apparently there are problems with keeping the cam on if the laptop
sleeps. see the wiki for more info

also, if there are startup problems with `bdc_pci` being loaded first,
this should force `facetimehd` to be loaded first, blocking `bdc_pci`.

```shell
echo "facetimehd" | sudo tee -a /etc/modules
```


### nvidia

installed with run.sh because that works better with CUDA.  downloaded
at [CUDA Downloads](https://developer.nvidia.com/cuda-downloads)

```shell
sudo apt-get install kernel-headers-`uname -r`
sudo sh cuda_7.5.18_linux.run
```

So this installed the nvidia drivers and CUDA software

### Macbook Pro Brightness Keys

Do this after configuring nvidia.  Otherwise it will be reset.

```shell
sudo setpci -v -H1 -s 00:01.00 BRIDGE_CONTROL=0
```

### rEFInd Config

after nvidia and other updates, init ram disk needs to be updated.
you'll need to do this whenever you upgrade the kernel and sometimes
when you add new devices.

So if you're using refind, this means you'll need to update the
vmlinuz and init ram disk images on your EFI partition.  Since it's
small, you'll need to delete the old ones.  If you're EFI is signed
(and it should be) or if your drives are encrypted (and they should
be) this process will be a bit more involved...

I wish I knew of an automated way to do this.  I'm sure there is one,
but since your rEFInd config is personally configured, it's hard for
an OS on your system to update it arbitrarily.  See notes about rEFInd
security.  It is incredibly important because if not properly
configured, you get locked out, or you provide an adversary with
physical access and limited time (< 15 mins) with super-root access.

For more info on properly restricting access to rEFInd, see my
[blog post](http://te.xel.io/posts/2016-01-21-bootloader-and-refind-security.html)
for a high level overview.

### monitoring tools

installed perf to see why kworkers were being spawned which consumed 100% of one core, constantly.

install perf dependencies:

```shell
sudo apt-get install linux-tools-common linux-tools-`uname -r` linux-cloud-tools-`uname -r`
```

run perf to monitor for 10s

```shell
sudo perf record -g -a sleep 10
```

view perf report

```shell
sudo perf report
sudo perf report --sort comm # for higher level view
```

still wasn't able to deduce what was causing this wierd CPU behavior though

### updating kernel from mainline

i wanted to get the latest kernel version (at least 4.4) so I
configured my system to install an upstream kernel from
[mainline](https://wiki.ubuntu.com/Kernel/MainlineBuildsxs).

so, i connected to the
[mainline index](http://kernel.ubuntu.com/~kernel-ppa/mainline/) and
found the
[v4.4 listing](http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.4-wily/),
then downloaded the proper deb's for my arch to `~/src/mainline/4.4`
with `wget`

```shell
cd ~/src/mainline/4.4

wget http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.4-wily/linux-headers-4.4.0-040400-generic_4.4.0-040400.201601101930_amd64.deb

wget http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.4-wily/linux-image-4.4.0-040400-generic_4.4.0-040400.201601101930_amd64.deb

wget http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.4-wily/linux-headers-4.4.0-040400_4.4.0-040400.201601101930_all.deb
```

and install them with `dpkg`

```shell
sudo dpkg -i linux-headers-4.4*.deb linux-image-4.4*.deb
```

After installation, I updated the vmlinuz and initrd images on my EFI
partition, rebooted and everything seemed to work alright.  no need to
update/reinstall nvidia drivers or wireless drivers, which i expected to.
I may still need to reinstall CUDA though.

however, i did need to rebuild `bcwc_pcie` and reinstall the `facetimehd` camera module

if you need to remove these mainline kernels, simply `sudo apt-get remove` the
corresponding packages

### updating kernel with build

this is a bit more advanced, but worth it, I think. the readme
included in the folder for the mainline kernel deb's describes the
build process.

basically, the kernel source is downloaded via git and a tag is
checked out.  for ubuntu, there are several patches which are applied
in order, the third of which contains the kernel config diffs. so I
can easily match the configuration used by ubuntu developers in
their build of the kernel and make minor modifications if i'd like.

so, start by cloning the repo

```shell
cd ~/src/mainline

# DOWNLOAD THE OFFICIAL TORVALDS TREE (instead of the below tree, the bandwidth is terrible)
git clone https://git.launchpad.net/~ubuntu-kernel-test/ubuntu/+source/linux/+git/mainline-crack linux-4.4

# wait for it ... wait for it ... lulz
```

while you're waiting (repo > 1GB), download the build deps:

```shell
sudo apt-get install libncurses5-dev gcc make git exuberant-ctags bc libssl-dev
sudo apt-get install fakeroot kernel-wedge
```

also, download the patchfiles from the ubuntu mainline

```shell
wget http://kernel.ubuntu.com/.../*.patch
```

now checkout v4.4

```shell
cd linux-4.4
git tags --list | grep 4.4
git checkout v4.4
```

apply Ubuntu patches


```shell
patch -p1 < 0001*.patch
patch -p1 < 0002*.patch
patch -p1 < 0003*.patch
```

AUFS

download `aufs4-standalone` and `aufs-util`

```shell
git clone git@github.com:sfjro/aufs4-standalone.git ~/src/aufs4-standalone
git clone git@git.code.sf.net/p/aufs/aufs-util ~/src/aufs-util
```

TODO: finish documenting AUFS process for Docker

- apply AUFS patches
- build aufs-util
- install aufs-tools

docs to [patch AUFS into kernel v3.19ish](http://zackreed.me/articles/89-compile-aufs-with-3-18-6-kernel-hnotify-and-nfs-exportability)

update permissions

```shell
# fakeroot was failing until i updated these
chmod a+x debian/rules
chmod a+x debian/scripts/*
chmod a+x debian/scripts/misc/*
```

build

```shell
make clean
make mrproper
make oldconfig
make -j8 deb-pkg
```

install

```shell
sudo dpkg -i ~/src/mainline/linux-*-4.4.0*.deb
```

upkeep: update initramfs whenever `apt-get` makes `DKMS` changes. my wifi seemed to lose it's wireless-n capability, so I needed to `sudo apt-get install bcmwl-kernel-source --reinstall`.  however, this didn't autogenerate an initramfs for my custom kernel, so i had to manually generate it with the following and then update my EFI.  (on the other hand, this might be speculation on my part)

```shell
sudo update-initramfs -u -k all
```

update EFI: copy /boot/* images and make conf changes


### ffmpeg

refer to [FFMPEG docs](https://trac.ffmpeg.org/wiki/CompilationGuide/Ubuntu)

> NOTE: you're really going to want to build these shared library dependencies
> in a location that's bundled together with FFMpeg and OBS.  Otherwise, your
> OS won't hesitate to step on them when you update.  Bye Bye lib-@#$%.so.
> No more OBS.

required packages:

```shell
sudo apt-get update
sudo apt-get -y --force-yes install autoconf automake build-essential libass-dev libfreetype6-dev \
  libsdl1.2-dev libtheora-dev libtool libva-dev libvdpau-dev libvorbis-dev libxcb1-dev libxcb-shm0-dev \
  libxcb-xfixes0-dev pkg-config texinfo zlib1g-dev
```

optional packages:

```shell
sudo apt-get install yasm libx264-dev libfdk-aac # build these
sudo apt-get install libmp3lame-dev libopus-dev # bin these
```

set `$INSTALLHOME`

```shell
export INSTALLHOME=/usr
export BINHOME=/bin
```

build yasm. yasm is for optimization.

```shell
git clone git@github.com:yasm/yasm ~/src/yasm
cd ~/src/yasm
./autogen.sh
./configure
make --prefix=$INSTALLHOME --bindir=$BINHOME
sudo make install
make distclean
```

build libx264-dev.

```shell
git clone https://git.videolan.org/git/x264.git ~/src/x264
cd ~/src/x264
./configure --enable-shared --enable-pic --enable-static --enable-swscale \
  --prefix=$INSTALLHOME --bindir=$BINHOME
# TODO: try --enable-static
# make fprofiled?
make
sudo make install
make distclean
```

probably don't need h265

libfdk-aac.  want this.

```shell
git clone git@github.com:mstorjo/fdk-aac ~/src/fdk-aac
cd ~/src/fdk-aac
autoreconf -fiv
# tried changing to --enable-shared
./configure --with-pic --disable-shared --prefix=$INSTALLHOME
make
sudo make install
make distclean
```

libvpx.  sounds cool.  free.

```shell
# i also needed to remove libvpx2 ... i think?

git clone git@github.com:webmproject/libvpx ~/src/libvpx
cd ~/src/libvpx
mkdir build
cd build
./configure --enable-pic --prefix=$INSTALLPATH --disable-examples --disable-unit-tests
make
sudo make install
make distclean

# note: this lib ignored --prefix and installed in /usr/local/lib
```

ffmpeg

```shell
git clone git@github.com:FFmpeg/FFmpeg ~/src/ffmpeg
cd ~/src/ffmpeg
PKG_CONFIG_PATH="$INSTALLHOME/lib/pkgconfig" ./configure \
  --prefix=$INSTALLHOME \
  --extra-cflags="-I$INSTALLHOME/include" \
  --extra-ldflags="-L$INSTALLHOME/lib" \
  --pkg-config-flags="--static" \
  --enable-gpl \
  --enable-pic \
  --enable-shared \
  --enable-nonfree \
  --enable-libx264 \
  --enable-libfdk-aac \
  --enable-libass \
  --enable-libfreetype \
  --enable-libmp3lame \
  --enable-libopus \
  --enable-libtheora \
  --enable-libvorbis \
  --enable-libvpx \

make -j8
#sudo make install #use checkinstall on debian
sudo checkinstall --pkgname=FFmpeg --fstrans=no --backup=no \
  --pkgversion="$(date +%Y%m%d)-git" --deldoc=yes
make distclean
hash -r
```

Ran into problems building obs with this ffmpeg build, where make was
complaining about "-fPIC" flags.  I needed to add --enable-pic or --with-pic
to each built dependency.  or --enable-shared.

### obs

refer to [OBS docs](https://github.com/jp9000/obs-studio/wiki/Install-Instructions)

clone repo and checkout the latest release

```shell
git clone --recursive git@github.com:jp9000/obs-studio ~/src/obs-studio
cd ~/src/obs-studio
git checkout -b 0.12.4 # or latest release
```

build deps:

```shell
sudo apt-get install build-essential pkg-config cmake git checkinstall
```

required packages:

```shell
# libx264-dev already built for ffmpeg
sudo apt-get install libx11-dev libgl1-mesa-dev libpulse-dev libxcomposite-dev \
        libxinerama-dev libv4l-dev libudev-dev libfreetype6-dev \
        libfontconfig-dev qtbase5-dev libqt5x11extras5-dev \
        libxcb-xinerama0-dev libxcb-shm0-dev libjack-jackd2-dev libcurl4-openssl-dev
```

build

```shell
cmake -DUNIX_STRUCTURE=1 \
  -DCMAKE_INSTALL_PREFIX=/usr \
  ..
make -j8
sudo checkinstall --pkgname=obs-studio --fstrans=no --backup=no \
       --pkgversion="$(date +%Y%m%d)-git" --deldoc=yes
```

Note: it helped to open a new terminal... I guess something didn't get set right with the ffmpeg install.  `cmake` couldn't detect the ffmpeg version number until I opened a new terminal. I don't think setting `--prefix` on each of the dep installs really helped.  instead,
i think it was the fact that i had skipped `pkg-config` with `ffmpeg`

### configure OBS

this always seems a bit harder on linux than osx. also, various streaming services likely have various requirements.  the livecoding.tv doc is pretty good

### configure RTMP multiplexer with live video effects

haha j/k j/k.  one day.

### python

So after i configured python in Linux Mint, somehow I ended up needing
to sudo for every friggin `pip` command...  So i really gotta get this
right.  Having that issue is really irritating to deal with after the
fact.

As of now, I have `ls -l $(which python)` pointing to `/usr/bin/python
-> python2.7` and `ls -l $(which python3)` pointing to
`/usr/bin/python3 -> python3.4`, with neither `pip` or `pip3`.  So,
clean install of both `python2` and `python3`.

```shell
sudo apt-get install python-pip
sudo pip install virtualenv virtualenvwrapper
```

#### configuring virtualenvwrapper

to configure `virtualenvwrapper` add the following to your
`~/.bash_profile` or `~/.zprofile` (or whatever)

```shell
export WORKON_HOME=$HOME/.virtualenv
export PROJECT_HOME=$HOME/dev/python
source /usr/local/bin/virtualenvwrapper.sh
# or load virtualenvwrapper_lazy for lazy loading
```

install pythons and pips:

```shell
# create a new virtaulenv
mkvirtualenv foo_env
# and thest
workon foo_env
pip install django # no sudo
```

strangely, i have a bunch of errors that show up concerning missing
scripts that actually exist.  i ran into this last time.

```
mkvirtualenv fooenv
New python executable in /home/dc/.virtualenvs/fooenv/bin/python
Installing setuptools, pip, wheel...done.
virtualenvwrapper.user_scripts creating /home/dc/.virtualenvs/fooenv/bin/predeactivate
virtualenvwrapper.user_scripts creating /home/dc/.virtualenvs/fooenv/bin/postdeactivate
virtualenvwrapper.user_scripts creating /home/dc/.virtualenvs/fooenv/bin/preactivate
virtualenvwrapper.user_scripts creating /home/dc/.virtualenvs/fooenv/bin/postactivate
virtualenvwrapper.user_scripts creating /home/dc/.virtualenvs/fooenv/bin/get_env_details
virtualenvwrapper.user_scripts could not run "/home/dc/.virtualenvs/premkvirtualenv": [Errno 2] No such file or directory
virtualenvwrapper.user_scripts could not run "/home/dc/.virtualenvs/preactivate": [Errno 2] No such file or directory
virtualenvwrapper.user_scripts could not run "/home/dc/.virtualenvs/fooenv/bin/preactivate": [Errno 2] No such file or directory
deactivate:unset:1: no such hash table element: pydoc
```

### jupyter



### EIN (emacs-ipython-notebook)



### scipy/numpy



### cuda

TODO: before tensorflow, i need to ensure CUDA is installed properly.
in linux mint, this meant reinstalling nvidia drivers using the run.sh
script.

### tensorflow

going to build/install tensorflow GPU version:

#### install bazel

for Ubuntu 15.10, install open jdk 8 and ensure other deps are installed

```shell
sudo apt-get install openjdk-8-jdk pkg-config zip g++ zlib1g-dev unzip
```

clone bazel:

```shell
git clone git@github.com:bazelbuild/bazel ~/src/bazel
~/src/bazel
git tags --list # find most recent tag
git checkout 0.1.4

# ... nevermind, just download the bin, don't feel like figuring it out

BAZEL_VERSION=0.1.4
BAZEL_INSTALLER=bazel-$BAZEL_VERSION-installer-linux-x86_64.sh
wget "https://github.com/bazelbuild/bazel/releases/download/$BAZEL_VERSION/$BAZEL_INSTALLER"
wget "https://github.com/bazelbuild/bazel/releases/download/$BAZEL_VERSION/$BAZEL_INSTALLER.sha256"

# ssh downgrade attacks make me nervous about wget'ing
# or curling any executable script
# ... i realllly lil to see that lil green icon in the browser

BAZEL_SHA=$(sha256sum $BAZEL_INSTALLER)
BAZEL_CHECK=$(cat $BAZEL_INSTALLER.sha256)
[[ "$BASEL_SHA" == "$BAZEL_CHECK"]] &&
  sudo bash $BAZEL_INSTALLER ||
  echo "WARNING: checksums don't match!"
```

#### config GPU support for TF

TODO: describe installing CUDA (using )

note: installing CUDA 7.5 and just linking 7.5 to 7.0, *supposedly*
works.

TODO: describe installing CUDNN toolkit 6.5 (2.0)

#### install tensorflow

clone tensorflow and init git submodules

```shell
git clone --recurse-submodules git@github.com:tensorflow/tensorflow ~/src/tensorflow
cd ~/src/tensorflow
# git tags --list # nah, ride this one on master, baby
# make sure you included --recurse-submodules, or TF doesn't include protobuf
# and you'll need to `git submodule init && git submodule update`
```

install system deps:

just going to build tf with system python.  mostly because i dont'
feel like learning how to config the equivalent of python-dev with
virtualenv. does that mean i need to build python? and setup up libs
in /usr/.../wherever?  these are the things i don't actually want to
spend the time learning.  it'd be wonderful if i could find a python
export to answer all these questions.

however, i'll be using tf with a virtualenv python, so i'll need to
ensure these libs are set up there. another alternative is to build TF
within a docker container. which just seems to be the way to go all
around, for many, many reasons.

also, i could just download a pre-built TF docker image, but where's
the fun in that? and besides, you can't [easily] add custom ops to TF
if you're using a pre-built docker image.  So no, no ops based on
OpenCV api.

and HA!, i said 'easily'.  maybe i should have said
'quickly' instead. although, that's a joke, if you're using GPU
support in TF.  the bazel build times are ridiculous with GPU!! i
think the key here is to learn how to perform differential builds
with bazel.

```shell
sudo apt-get install python-numpy swig python-dev
```

also, note: if performance really is an issue, building the deps for
python numpy should probably boost performance significantly.  but, so
does using c++ instead of python.  and in the end, your TF code will
be written in c++ if it does anything crucial.

also, nvidia offers libs with GPU support for Lapack/BLAS, etc.
Really, there is a ton of room for improvement on most apps with
scientific computing and/or machine learning dependencies.

### opencv


### ruby

ruby-install

```shell
git clone git@github.com:postmodern/ruby-install ~/src/ruby-install
cd ~/src/ruby-install
sudo make install
```

chruby

```shell
git clone git@github.com:postmodern/chruby ~/src/chruby
cd ~/src/chruby
sudo make install
echo "source /usr/local/share/chruby/chruby.sh" >> ~/.zshrc
source ~/.zshrc
```

ruby

```shell
ruby-install --latest
ruby-install ruby 2.3.0

# TODO: set system default?
```

### swift

why? because swift+swig is probably going to rock.  i'm just saying.

[ray wenderlich guide](https://github.com/apple/swift). swift.org
[repository list](https://swift.org/source-code/#cloned-repositories)
and
[dev snapshots](https://swift.org/download/#latest-development-snapshots).

but, i'm going to want to build swift-lang on linux, so that process
is described on the main [swift repo](https://github.com/apple/swift).
also, need to build the
[swift package manager](https://github.com/apple/swift).  and download
some of the core libs.

swig will be needed if you want to do anything useful with swift. swig
helps create interfaces to c++ from many different programming
langs. it appears that someone has already tried to create a project
to bridge `swift` to `c++` libs using `swiq`, but sited some problems
a bit over my head (non-re-entrant?) as to why it wouldn't work well.

it is possibly to extend swift with the functionality of c++ libs, but
they need to be wrapped with c header files. i don't know enough about
this domain to understand the best way to do this.

one of the reasons I like swift so much is because it's like a
`functional c`.  you can craft low-level, memory structing,
bitcrunching apps and it feels fast.  yet, you can easily employ your
favorite functional programming patterns.  and it easily interops
with c if you need it.

### docker

docker's own documentation is sufficient.  mostly documenting this
because, when using a custom kernel in Ubuntu, you can't `apt-get` a
package for `linux-image-extra`, so ... no AUFS.  also, i want all of
my set up documentation to be centralized.

setup the docker ppa

```shell
echo "deb https://apt.dockerproject.org/repo ubuntu-wily main" > sudo tee -a /etc/apt/sources.list.d/docker.list
sudo apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
sudo apt-get update
sudo apt-get purge lxc-docker
sudo apt-cache policy docker-engine # package docker-engine doesn't exist...
```

.... welllll looks like i have to patch the kernel and rebuild. fuck
that right now.

### weechat (linux)

for some reason, my osx weechat config doesn't work on linux.  it just
freaks out and loses the colors.  THE COLORS!!

TODO: document weechat setup

NOTE: it makes me nervous running weechat with any priviledges at all,
knowing that all the plugins rely on scripting languages and
user-based configuration of string interpolation.
euuugghhh. seriously, you should probably run weechat under another
user or some shit, though ... that's not really going to help. run it
on another laptop.  that's what i always do. or within a vm/docker.

sometimes i wake up at night in a cold sweat and i could swear there's
a weechat plugin vuln under the bed and i freak out and check it
... but there's never anything there.

### prodigy XMPP chat

Available via apt-get on ubuntu 15.10. If you want to build yourself,
you'll need to build some dependencies as well.

```shell
sudo apt-get install profanity
```

To manage passwords in your profanity config, use GNU `secret-tools`:

```shell
sudo apt-get install libsecret-tools
```
