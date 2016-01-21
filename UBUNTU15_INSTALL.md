## Ubuntu 15.10 Install

I originally installed this one, then couldn't get the xkb keyboard configuration running.  it kept causing errors, but i think I had invalid XML, so I'm trying it again.

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

Hit `Super` and type `privacy` and hit `Enter`.  Navigate to the `Files & Applications` tab.  Turn it off.  If you're a programmer, you don't need that.  Uncheck all the boxes.  Click 'Clear Usage Data'.

Use Find+Grep: `grind() { find . -type f -exec grep --color -nH -e $1 {} +` 

> One reason I love emacs: it uses mostly GNU tools and is magnificent 
> in exposing it's interface to you to show you the options available 
> and how it does things. Learn how to use apropos to discover it's functionality.

Next: Click the `Search` tab and tell it to exclude online results.  Also, you may want to exclude error reports.

Now, install `Unity Tweal Tool` with `sudo apt-get install unity-tweak-tool`.  Open it.

There are some duplicate configurations here, but also more options.  To me, it's a bit irritating that the default Unity interface is so simplified, but I guess I understand why.  However, for me personally, I like all my configuration for an OS/App `in one place` and `searchable`.  IntelliJ and Android are two excellent examples of configuration UI's that are robust and easily discoverable. 


### loxodo password safe

It is a small tragedy, but there are no pwsafe managers for linux that:

#### (1) I trust.  A password manager written in high-level language such as TCL really needs some thought.
#### (2) Build easily without sketchy dependencies

loxodo is a small command-line and GUI capable system for opening password files.  

```shell
# i've got this one memorized
sudo apt-get install git@github.com:sommer/loxodo ~/src/loxodo
cd ~/src/loxodo
```

now put your password file where you want it and keep it on USB. also, you can keep loxodo on USB and load it as needed. luks that ish.

`./loxodo.py -i [psafe3file]` to run in interactive mode.  this can also be run in GUI mode, which I feel is less trustworthy.  close the app when you're not using it.  you can set a shortcut to open it, but be careful.

### dotfiles

clone dotfiles

```shell
git clone git@github.com:dcunited001/dc.files ~/.files
cd ~/.files
git submodule init
git submodule update # and if you're not me, my repos from bitbucket will fail to d/l
``` 

run init scripts with `./init/ubu.sh` and follow instructions. this isn't is a complete config, but it's close enough.  some of the stuff is outdated.  i don't completely configure vim any more.  the emacs init scripts are also outdated.  i mostly just use it to set up links for zsh.

```shell
./init/ubu.sh
```

link emacs.  i now use `purcell/emacs.d`

```shell

```

### fonts

i like having symbola fonts and setting all fonts to deja vu w/ powerline.  these powerline patched fonts can be found in my dotfiles.  copying these fonts to `~/.fonts` should make them available to the system.

```shell
cd ~/.files/
mkdir ~/.fonts
cp -R ~/.files/powerline-fonts/**/*.ttf ~/.fonts
```

now, just hit the super key, type fonts and hit enter.  that should open the Font Viewer.  verify that everything is there.

### terminator profiles

```shell
sudo apt-get install terminator
```

Here, i just

TODO:

### xkb

yeh, turns out the problem was the format of xml (forgot to put configItem tag inside variant tag)

- moved io to `/usr/share/X11/xkb/symbols`
- configured evdev.xml in `/usr/share/X11/xkb/rules/evdev.xml`
- swapped values for <CAPS> and <ESC> in `/usr/share/X11/xkb/keycodes/evdev`
- `echo 2 | sudo tee /sys/module/hid_apple/parameters/fnmode` to fix fn keys for session
- `echo options hid_apple fnmode=2 | sudo tee -a /etc/modprobe.d/hid_apple.conf` to fix function keys for subsequent sessions
- after updating `hid_apple.conf` need to copy this config to initramfs with `sudo update-initramfs -u -k all`
- after reboot, everything seems to work

### emacs

i need emacs 25 for several Melpa package dependencies included in `purcell/emacs.d`

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

Open emacs and it should automatically download the necessary packages.  

```shell
emacs
```

This will take about 5 minutes.  There's a lot of software.  This makes me nervous.  If you didn't build or download `Emacs 25` the elisp packages won't download and you're emacs will be baroque like a harpsichord.  

You'll want to configure a theme because the default one is ugly, so hit `M-x`, type `customize-themes` and hit `Enter`.  This brings you to the themes configuration panel.  Some themes are a security risk, so it may be wiser to stick with some of the defaults.  Uncheck the themes you don't want and check the one you do, this save the configuration by navigating over the button and hitting enter.

I like the `Cyberpunk` theme, which I have to download.  If you need to download a theme, hit `M-x list-packages` and `Enter`.  This lists the packages available to download from Melpa.  Now hit `C-s` to begin `I-search`.  Now, type `cyberpunk` and `Enter` to bring your curser over the `cyberpunk-theme` line.  Now, hit `?` to see your options for the list packages screen.  This works for most emacs menus.  Notice, you mark packages with `i.  So, hit `q` to escape the packages screen.  If you're hovering over the `cyberpunk-theme` line, hit `i` to mark it, then `x` to download all marked packages.  You'll also need to hit `y` to accept them.  

`Magit` is your best friend.  It's the emacs git package and you can do anything.  Again, Magit exposes the git command line interface to you and if you don't know that very well, you can easily learn it with `Magit`.  You can send emails from `emacs`

If you want to learn Emacs (or Linux) the first thing you need to know is how to discover information.  If you don't learn this, you're going to have problems for a long, long time.  For Linux, this means learning things like how to use `less` commands to search `man` pages.  Every time you hit Google to search StackOverflow, there is a way to discover that information without the internet that is 20x faster!  Another thing to learn with Linux is where log files are stored and where configuration files are stored.  A great quick tip for linux beginners is to learn to search your history with `history | grep XXXXX`.  This allows you to rediscover commands you've used before.

For Emacs, this means learning to use Apropos.  If you need to search for a function, type `C-h a` to open apropos search.  Type the partial name of the command you're looking for.  `EVERY BUFFER IS TEXT!` so you can search anywhere with `C-s` to open `I-search.  If you run into problems on a menu screen, hit `?` to see your options.  You can run shell commands on `dired` screens, which contain file listings using `!` and typing the command.  

The greatest advantage of Emacs for programming is the REPL.  For small programs in scripting languages, you can run a repl and re-evaluate classes on the fly.  It gets a bit more complicated if you're trying to build a module in python.  There are even greater advantages to using the REPL if you're building a lisp like clojure and with clojure, you can even connect to remote REPL's!  

With the REPL, you can also dynamically re-evaluate a class and run it's tests without having to re-evaluate your environment.  For large frameworks like Ruby on Rails, this translates into MASSIVE TIME SAVINGS.  But the configuration and learning curve of emacs often prevents people from getting to that point.  

Also, emacs is configured via `elisp`, which is an awesome introduction to lisp and functional programming.  It's magical.  So by learning emacs, you're learning to be a better programmer.  But often, it's better for beginners to just use a common IDE.

Anyways, moving on... (i'll like convert the above to a blog or something)

### wireless

`sudo apt-get install bcmwl-kernel-source`

restart after wireless finishes installing.  you may need to upgrade your initramdisk.  this is done with

### webcam

deps

```shell
sudo apt-get install 
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

and install.  unload `bdc_pci` before running `checkinstall`.  otherwise the `facetimehd` module will already be loaded, which can be a bit confusing.

```shell
sudo checkinstall
sudo depmod
sudo modprobe facetimehd
```

apparently there are problems with keeping the cam on if the laptop sleeps. see the wiki for more info

### nvidia

installed with run.sh because that works better with CUDA.  downloaded at [CUDA Downloads](https://developer.nvidia.com/cuda-downloads) 

```shell
sudo apt-get install kernel-headers-`uname -r`
sudo sh cuda_7.5.18_linux.run
```

### rEFInd config

after nvidia and other updates, init ram disk needs to be updated. 

### ffmpeg


### obs



### python

TODO: make sure i get python right


