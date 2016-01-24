## Ubuntu 15.10 Install

I originally installed this one, then couldn't get the xkb keyboard
configuration running.  it kept causing errors, but i think I had
invalid XML, so I'm trying it again.

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

```shell # i've got this one memorized sudo apt-get install
git@github.com:sommer/loxodo ~/src/loxodo cd ~/src/loxodo ```

now put your password file where you want it and keep it on USB. also,
you can keep loxodo on USB and load it as needed. luks that ish.

`./loxodo.py -i [psafe3file]` to run in interactive mode.  this can
also be run in GUI mode, which I feel is less trustworthy.  close the
app when you're not using it.  you can set a shortcut to open it, but
be careful.

TODO: setup loxodo with GTK for GUI (requires pip & pygtk?)

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

```

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
your initramdisk.  this is done with

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

### nvidia

installed with run.sh because that works better with CUDA.  downloaded
at [CUDA Downloads](https://developer.nvidia.com/cuda-downloads)

```shell
sudo apt-get install kernel-headers-`uname -r`
sudo sh cuda_7.5.18_linux.run
```

So this installed the nvidia drivers and CUDA software

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

if you need to remove these, simply `sudo apt-get remove` the
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

apply patches

```shell
patch -p1 < 0001*.patch
patch -p1 < 0002*.patch
patch -p1 < 0003*.patch
```

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

update EFI

### ffmpeg

refer to [FFMPEG docs](https://trac.ffmpeg.org/wiki/CompilationGuide/Ubuntu)

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

build yasm. yasm is for optimization.

```shell
git clone git@github.com:yasm/yasm ~/src/yasm
cd ~/src/yasm
./autogen.sh
./configure
make
make install
make distclean
```

build libx264-dev.

```shell
git clone https://git.videolan.org/git/x264.git ~/src/x264
cd ~/src/x264
./configure --enable-shared --enable-pic
# make fprofiled?
make
sudo make install
```

probably don't need h265

libfdk-aac.  want this.

```shell
git clone git@github.com:mstorjo/fdk-aac ~/src/fdk-aac
cd ~/src/fdk-aac
./autogen.sh
./configure --with-pic --disable-shared # may want to locally install
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
../configure --enable-pic --disable-examples --disable-unit-tests
make
sudo make install
make clean
```

ffmpeg

```shell
git clone git@github.com:FFmpeg/FFmpeg ~/src/ffmpeg
cd ~/src/ffmpeg
./configure --pkg-config-flags="--static" \
  --cc="gcc -m64 -fPIC" \
  --enable-gpl \
  --enable-libass \
  --enable-libfdk-aac \
  --enable-libfreetype \
  --enable-libmp3lame \
  --enable-libopus \
  --enable-libtheora \
  --enable-libvorbis \
  --enable-libvpx \
  --enable-libx264 \
  --enable-nonfree
make
#sudo make install
sudo checkinstall --pkgname=FFmpeg --fstrans=no --backup=no \
  --pkgversion="$(date +%Y%m%d)-git" --deldoc=yes
make distclean
hash -r
```

ran into problems building obs with this ffmpeg build, where make was
complaining about "-fPIC" flags.  i needed to add --enable-pic or --with-pic
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
  -DCMAKE_C_FLAGS="-fPIC" \
  -DCMAKE_CXX_FLAGS="-fPIC" \
  ..

```


### python

So after i configured python in Linux Mint, somehow I ended up needing
to sudo for every friggin `pip` command...  So i really gotta get this
right.  Having that issue is really irritating to deal with after the
fact.

As of now, I have `ls -l $(which python)` pointing to `/usr/bin/python
-> python2.7` and `ls -l $(which python3)` pointing to
`/usr/bin/python3 -> python3.4`, with neither `pip` or `pip3`.  So,
clean install of both `python2` and `python3`.

TODO: pip without sudo

TODO: virtualenv?

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
```

### docker



### tensorflow


