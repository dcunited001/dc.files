dc.files on Linux Mint 17.3
===========================

Taking some notes while I configure my environment for Linux Mint.  Mostly doing this for myself, since
my dotfiles are slightly different for Linux than for Mac.  That way, if don't have to scour the
nets again if I have to blow away my Linux install, which happens quite often.

The main reason I'm installing Linux alongside OSX is because CUDA only works natively and the GPU build of
TensorFlow doesn't work with OSX at the moment. I also want to switch to Linux for a myriad of minor reasons,
but there's just as many reasons I'd like to stay on OSX.

### Application List

This is a brief list of software that I'm installing.  I'm going to be working in OSX and Linux, but
I'm really going to miss **iTerm2** and **SourceTree**, as well as lots of other software as well.

#### Karabiner/Seil (KeyRemap for Macbook)

In OSX, these programs give you the ability to completely remap your keyboard with absolutely zero limitations.
I'm going to try out `xkb`, which is a command line tool for creating custom keyboard layouts.  I've used `xmodmap` in
the past, but I've found that getting it to reliably load and function on startup is frustrating.

I found some `xkb` articles I hadn't seen before.  I last looked in 2013, so I don't know how I missed these.


- [XKB config](http://www.dotkam.com/2007/06/25/custom-keyboard-layout-in-ubuntu-or-just-linux-2/)
- [Changing Keyboard Layouts with XKB](http://hack.org/mc/writings/xkb.html)
- [Keyboard Config in Xorg (Arch Linux)](	https://wiki.archlinux.org/index.php/Keyboard_configuration_in_Xorg)

I hope I don't spend too much time getting `xkb` to work.  I really just need my
[digimon keyboard layout](https://github.com/dcunited001/dc.files.kbd#digimon-layout)
to work. It's awesome. I couldn't live without it as a developer.

The one piece of behavior from Karabiner that I know to be nigh-impossible to get in Linux
are the simultaneous keypresses.  I can simulatneously press the arrow keys in lieu of `home` and `end`.
I can also get easier access to chars like `< > { } ~` without having to hit a modifier key, which is kinda nice.
I don't use these "key chords" very often, except for angle brackets.  Not the same thing as emacs "key chords" of course.
These are like true chords.

> There is one limitation to the Karabiner software: the ability for
> one application to intercept and replace a modifier key to be used
> as a different modifier key in another application.  Such a feature
> would be impossible, but it'd allow me to use this awesome pedal
> hardware, so i can define modifier keys for my feet.
>
> Yes, I actually bought one of these pedals just for that.  And no,
> there's no way to configure it to work how I want.  Trust me, I
> spent hours trying to get it to work ... womp womp =[

#### PasswordSafe for Linux

Ugh ... running into some frustrating issues installing the linux pwsafe debian packages.  Seriously?

Running into dependency version mismatch issues with this one.  Not really sure why the newest versions
of the libwxgtk3.0 are missing from the standard Debian/Ubuntu repositories.  I can get `3.0.0`, but not
`3.0.2-1+b1`.  Also, the .deb that's being distributed (it's a beta) depends on a beta from another
window manager?  That's sketchy.

Spent about 2 hours on this one.  Going to just use some older scripts and move on.

#### GUI Git Client

SourceTree is a great Git client for OSX/Windows with a GUI.  No idea why a cross platform app isn't available on linux,
but is available on both windows and mac.  That's strange but I'm sure there's a good reason for it.

There's a beta app for a new GUI Git client called **Git Kraken**.  It's completely cross platform.  I hope
to be able to use it.  Otherwise, I'm going to look at smartgit, gitg, giggle, git-cola.

Sweet, i got approved for the Git Kraken Beta.

#### Quicksilver/Alfred

I'm not sure what to go with here either.  Quicksilver allows me to map globally interpreted keypresses to launch/show
applications.  So I never have to hit `alt-tab`.  I know, I know, Quicksilver and Alfred have tons of other features too.
But this is the main feature I want, since by using it, I never have to retain the state of the OS user interface in my limited
short term memory.  It helps, a ton.

I've tried Gnome DO in the past, which worked pretty well, but I'd like to try something else.
Maybe [Synapse](https://code.launchpad.net/synapse-project).  Yeh, going to try Synapse.

However, upon reading [this article](http://www.noobslab.com/2015/02/synapse-launcher-new-version-released.html)
and hearing that Synapse works by logging user activity and searching/indexing those logs, I'm
not sure I would want to use it.  I'll try it out i guess.

Also, it installs a language called `vala`.  Weird.  Were other
languages not good enough? A little-known lanuage that, with the
Synapse app, accesses your user activity logs and has it's own Virtual
Machine according to the wiki page:

> Rather than compiling directly to machine code or assembly language,
> it compiles to a higher level intermediate language.

Sketchy. No offense. Not saying there's a plot afoot here.  I'm just saying ... and
i feel bad saying it too.  It's not like writing your own language is easy or anything.

Just going with Gnome Do for now.

#### IRC (Weechat)

**IRC: weechat**.  I like my irc in a terminal, with miami vice 80s colors

```
sudo apt-get install weechat weechat-plugins # ..... JUST KIDDING
```

#### XMPP Chat Service (Profanity)

Profanity isn't available through the Ubuntu 14.04 standard package
lists.  To install, you'll need to pull it down from git (or download
the tarball from [Profanity's homepage](https://profanity.im)) and
build from source.

You'll need to get `libncursesw5-dev`. If you don't have it,
`profanity` will build, but won't run due to wide-character issues.

```shell
sudo apt-get install libncursesw5-dev
```

You'll also need to install either `libmesode` or `libstrophe0` and
`libstrophe-dev`, none of which are available via debian's 14.04
Ubuntu package managers.  You'll have to build these from source too.

Try apt-get first, as these packages are listed on launchpad, so they
may work on some debians. Though, really, it's better if you compile all
your software.  It takes a lot longer, but everything is optimized for
your architecture and hardware, which translates into better
performance and better power consumption!  You can also force apt-get
to compile software.

```shell
sudo apt-get install libmesode
sudo apt-get install libstrophe0 libstrophe-dev
```

If both those commands fail to locate packages to install, you'll need
to build one yourself.  I went with `libmesode` as it's from the same
author as `profanity`.

```shell
cd $HOME/$MY_SRC_DIR
git clone git@github.com:boothj5/libmesode
cd libmesode
./bootstrap.sh
./configure
make
sudo make install
```

That should do it for `libmesode`.

Now you need to update the shared library cache using `ldconfig`.
Otherwise, you'll get runtime errors when you run `profanity` because
it can't find `libmesode.so`

```shell
sudo ldconfig
```

OK, now you're ready to build `profanity`.

```shell
cd $HOME/$MY_SRC_DIR
git clone git@github.com:boothj5/profanity
cd profanity

# since you cloned it, you also need to run the bootstrap script:
./bootstrap.sh
./configure

# if you didnt get errors in the configure script, you're good to go
make
sudo make install
```

Great, now you should have profanity.  Additionally, you'll need to
download GNU's `secret-tool` if you want to configure it to stor your
credentials securely.  Refer to the
[Profanity FAQ](http://www.profanity.im/faq.html#keychain) for more
info on adding your XMPP service passwords to the `secret-tools`
keychain and configuring `profanity` to reference them, securely.

```shell
sudo apt-get install libsecret-tools
```

K, now you're ready.  Run `profanity` and refer to the
[docs](http://www.profanity.im/basic.html) for more info on
[themes](http://www.profanity.im/themes.html),
[config files](http://www.profanity.im/files.html), and
[room configuration](http://www.profanity.im/roomconf.html).
Configuration is stored in `$HOME/config/profanity/profrc`.

#### Other applications

**Hex Editor**: wxHexEditor.  Cross platform hex editor.

```
# easy
sudo apt-get install wxhexeditor
```

It could be that simple, folks.  but its not.  not only does ubuntu offer an
old version through the debian package manager, but it also comes bundled with linux
mint? i think?  which is nice, but it's and old version, so the config files break.
which isn't so bad, except the old version overwrites your config files on install. yay!
so then you spend a ton of time figuring out what you need and what you don't before
you finally realize: oh even though i removed it from apt-get, it's still on my system
and it's way old, this isn't a new version that's breaking my shit, it's an old one....

## so yeh: time wasted, lesson-learned:

> just compile everything from source

... oh, and just run everything on a geeked out arch linux distro, terminal only,
with maybe an ncurses-only gui.  maybe.

> on the flipside, the GUI IRC client bundled with Linux Mint looked interesting.
> I almost used it, except, again ... 80s miami vice colors make me look liek a l33t
> bell labs hacker.

and also, text interfaces are almost more fun to configure.
configs are almost always self-explanatory and portable. for most
GUI apps, a portable human-readable config file is a second thought.
this is actually the exception and not the rule with linux, though. kudos.

**OBS**

my iSight webcam don't work without a struggle.  did i mention that yet?  i think i did
but i can't remember. lulz.

**Installed Vivaldi browser**

Facebook in chrome asked to integrate with Mint/Ubuntu's notification system.  This is not
something i'd normally enabled, but i was intrigued seeing this is an OS like Mint/Ubuntu.
So I enabled it.  I'll likely eventually disable it, as it makes me uncomfortable.  I want
my browser sessions to end at the browser.  Sorry.

### Getting Linux Installed

If you are unfamiliar with the soul-sucking, self-ablating, hellish cyclocosm of mundanity that is
attempting to install Broadcom Wifi drivers without ethernet, you may not be able to relate to this section.
Skip ahead to Initial Environment Configuration.  Otherwise, proceed ahead, but beware - you are risking
your sanity by simply reading the next few paragraphs.

Installed linux mint on a 2013 Macbook Pro.  I ran into some issues with wifi and bootloader.
I only had one USB drive with me and could access the net, as I didn't have a wired connection.
Kind of a bummer since I didn't want to walk home.  Goddammit I hate Broadcom.  I swear to god,
there is space in the 8th circle of hell reserved just for Broadcom Wireless.  Right beside the
pedophile-apologist pope from the 12th century.  But I digress.

Tried to keep Mint running while I manually
downloaded the broadcom-wl drivers on another laptop.  But cinnamon crashed when I tried to get Broadcom
drivers installed.  Bullshit.  Couldn't keep linux installed because I couldn't download and run `efibootmgr` with
out wifi.  Had to install multiple times.  Not a big deal, but still bullshit for something that should
take 5 minutes.

Thought about creating a FAT partition on my hard drive, so I could cache
some files.  But I was just tired of dicking with linux and trying to hack on wifi.  I didn't realize
that the Broadcom drivers were already included with Linux Mint -- I think no d/l necessary??  Which
is, once again, bullshit because I never actually had a problem to begin with.  All in all,
I wasted like 2 hours of time wrestling with a dumb problem that wasn't real.  I gave in
and started reading some papers I needed to catch up to learn about computer vision and
machine learning algorithms applied towards diagnostics.

Moving on...

### Initial Environment Configuration

So later, i had ethernet access and everything took 5 minutes to install like it's supposed to. I had
some issues with the hidpi resolution being way to high, since the MBP 2013 screen is retina.  I needed
to go to `Preferences > General` to get Cinnamon to adjust UI elements for.

Note: retina-display resolution became a problem once I wanted to use the GPU for computing.  Over 700GB of GPU RAM are in use at all times on my system.  Tensorflow blows up if it tries to allocate more memory than is available.  Livestreaming over OBS requires even more memory.


>  Things that are awesome: apparently shitty wifi drivers are bundled with the Linux Mint ISO and *may not actually require internet*.
>  I may just be an idiot.
>
>  Equally awesome: NVIDIA drivers work out of the box.  I just had to click a button.  That's fantastic.

I also set up the `English(Macintosh)` keyboard, which helps a little bit.  Some global hotkeys are restored,
but most applications still recognize `Control` as `Super`, which is just really confusing.  Chromium does this.  Honestly, if
that's the case, I'm going to just use the `English(International)` keyboard.  Linux's Mac keyboard layout doesn't help at all.

Just needed to get some new SSH keys configured on the system and registered with Github/Bitbucket,
so I could start cloning my dotfiles, which have submodules registered with git protocol instead of HTTPS.
So, now that i've got my .ssh folder configured.  I can download repos.

```
cd $HOME
sudo apt-get install git emacs vim curl

# now I can clone things and edit text
git clone git@github.com:dcunited001/dc.files .files
```

I haven't used the ubuntu version of my dotfiles in some time, so I've got my fingers crossed.  I hope I don't have spent
too much time on this. I'm hoping to hurry up and get to this computer vision project.

### dc.files

- ran install scripts
- made a few changes
- needed to change the location of /etc/zshenv to /etc/zsh/zshenv
- otherwise the scripts in dc.files.init worked fairly well
- emacs prelude did not get set up, but i don't think I'm using it anymore
- dicked around with powerline for wayyyy too long
    - couldnt get the stuff installed
    - something's messed up with python3 install
    - can't install anything without sudo and even then it breaks
    - tried virtualenv, but then i have to do everything (like powerline) under that
- so any other tools i'd use are messed up in the shell
    - and i dont' want to install stuff like this as sudo
- so i just reverted to the agnoster zsh theme
    - python's still dicked up but i think i can deal with it.

### fonts

Installing the `Symbola` font gave me emoji text in ubuntu/mint.

```
sudo apt-get install ttf-symbola
```

This allows emoji to be rendered as text in applications, like emacs
and in the text-entry for chrome.  You don't actually have to set this
as your default font.  Ubuntu has font fallback, which searches
through its library to find a font which contains a missing char

### terminal app

TODO: try gnome terminator

### misc hardware issues

#### Brightness Adjustment Keys

for the most part, i didn't have any problems with my macbook pro 2013
- i did need to configure backlight pci keys
- simple `setpci -v -H1 -s 00:01.00 BRIDGE_CONTROL=0`


... these no longer work.  brightness levels appear to adjust, but the actual brightness of the screen does not.  yay.  oh well.

#### Mac OSX Function & Media Keys

In order to get my digimon keyboard layout to work, i had to change how
the function and media keys work by overwriting the `fnmode` file.

This is described in more detail on [this S/O question](http://unix.stackexchange.com/questions/121395/on-an-apple-keyboard-under-linux-how-do-i-make-the-function-keys-work-without-t).

```
# "2" means that pressing top row of keys results in F1, F2, F3
echo 2 > /sys/module/hid_apple/parameters/fnmode
```

#### Built-in Webcam

These kinds of issues are frustrating.  There are at least two main
vendor incentives to make it a little bit more difficult to try Linux
and OS alternatives.  Both Microsoft and Apple are guilty of these.

1. First, by make it difficult to use all your hardware features on
   another platform, this makes it more difficult to leave that
   platform and have a great experience.  This is not the primary
   incentive though, but it does prevent some people from running more
   than a Linux live cd. 😫 And it does prevent users from leaving,
   there is a definite benefit to that.

2. Secondly, and this is the primary incentive, supporting third party
   OS's is a lot of work.  This is an investment of time and may
   preclude a 'pure' design or featureset or implementation.  For
   example, if Apple produces a driver for a device, then making sure
   that driver is compatible with a third-party OS may mean the design
   they *want* doesn't really work.  But mostly, why should a first
   party hardware/software company need to invest time in to
   supporting third-party products.  And then supporting it implies
   they are willing to support it to a degree.  It's unnecessary
   time/effort, it detracts from flexibility in hardware/software
   design, it increases product development time.

However, this is incredibly frustrating.  And not because I want to
switch to Linux completely, but because I want to run Linux
side-by-side and I don't want to be essentially penalized with dozens
of hours of work when I do that.  Hardware support has always been an
issue with Linux, but it's improved magnanimously.  But mainly, the
problems I'm having require more time to correct due to lack of
experience in Linux and in the specific hardware set I'm working with,
which is MacbookPro 15" October 2013.  These problems are difficult to
search for, especially for noobs, because there's so much content out
there that is similiar.  I didn't instantly determine that i didn't
have a iSight camera, for example.  That wasted a lot of time!

The webcam didn't work out of the box.  I tried messing around in OBS
to find it, but the device wasn't being found.  `sudo apt-get install
cheese` and `cheese` to troubleshoot.

Looked at a lot of information on the iSight camera.  It seems like i
may need access to the HFS partition, which i didn't realize was even
possible.  Actually it's stupid to think that an HFS+ partition from
OSX would ever be inaccessible from Linux, but I was beyond certain
that it was.  Dammit, i wish that when i did stupid shit like bitch
about HFS+ access in Linux, there was someone right beside me who
would be there to smack my ass before i wasted hours on a problem that
was never a problem at all.

oh, but it's soooo easy to get
[iSight webcam access](https://help.ubuntu.com/community/MactelSupportTeam/AppleiSight).
if you can mount the HFS+ filesystem.  mine's encrypted.  this seems
to be causing some problems.

#### Built-in Webcam (Round 2)

Apparently, I don't have an iSight webcam and I'm an idiot for
thinking I do.  One one level this is a small hit to my ego (since
even though this is not at all obvious in Linux or OSX) but also
exciting bc maybe i can get my webcam working from linux.

Someone has written some drivers for the `facetimehd` camera, which is
manufactured by `Broadcom`, whose very name makes me tremble with
anger.  This package is experimental, but also, the following quote
gives me a glorious feeling of righteousness:

> You can contribute and test this module without touching your Mac's Hard Disk.

Praise be to the gods of open source, indeed.  First off, when following the [Getting Started](https://github.com/patjak/bcwc_pcie/wiki/Get-Started). Apparently, a mistake, but I'm going to describe this in the order in which i proceeded.  So here's what i did, following the [Installation](https://github.com/patjak/bcwc_pcie/wiki/Get-Started#installation) section.

```shell

# get dependencies
sudo apt-get install linux-headers-3.19.0-32-generic git kmod checkinstall

# clone bcwc_pcie
cd ~/$YOUR_SRC_FOLDER
git clone git@github.com:patjak/bcwc_pcie
cd bcwc_pcie

# build
make

# and install
sudo checkinstall

# (but i realized later, that i need to configure it for my hardware)
# that is, i skipped the "firmware extraction" step

```

However, it still wasnt working.  For Gnome users, you have the `cheese` GUI tool for using your webcam.  If it's not available `sudo apt-get install cheese`.



I needed to refer to the TLDP page on [webcams](http://www.tldp.org/HOWTO/Webcam-HOWTO/hardware.html), which contained some very handy info on troubleshooting general linux device issues.  I've used `lsmod`, `modprobe`, etc before, but this is a great resource in learning the ins and outs of dealing with linux device issues.

To check if a device is loaded, run `dmesg | less` and search for your device. At this point, if you don't see it in the output, your device driver is not loaded by linux and applications won't be able to discover it either.

My webcam driver was not loaded.  I needed to discover if the driver was loadable and to do that, run `find /lib/modules -name $DRIVER_NAME`.  I stored these commands as aliases and put them in my dotfiles, so I can more quickly reference them later.  Better than using Google every time and this helps me remember them.  That, and blogging about it.

In case, my driver was reported as loadable.

You need to check if your device module is loadable, so run `





#### Getting access to HFS

The following command did not work.  This hardware compatibility thing is getting to be more of a problem then I thought.

```
sudo mount -t hfsplus /dev/sda2 /media/machd
```

I think this is not working bc my OSX partition is encrypted... i think?  I hope that's why but wait ... that still sucks.  Now i have to figure this shit out.  Guys, I just want to stream on OBS.  Thought I was going to be able to do that at 10:00pm.  Now it's 12:00am.

Yeh ... encyrpted HFS+ partition [just doesn't look fun at all](https://t.co/Yg83OExgjG).

nope.  nope. definitely not.

## FUCK IT, GUESS I"M NOT_STREAMING TONIGHT"


#### power management tools

in the short time that i've been using linux mint 17
- and running a **minimal** number of apps
- i've notice that my battery is being sucked dry
- mint 17 is draining my MBP battery in less than half the time as OSX
    - when in OSX, i'm constantly building complex swift applications, etc
- so, in mint, i'm getting around 120-150 minutes on a full charge
    - single monitor and just a few shells running
    - i really wonder what the hell is going on.

started looking for power monitoring and management tools
- found linux mint's set of power management tools to be lacking
    - power management in settings just offers the basics
    - power statistics doesn't offer any process-level information

found that `laptop-mode-tools` was recommended
- but this package has been integrated into the kernel (i'm running 3.19)
    - attempting to install it resulted in errors, so i removed it

browsed around the net and found TLP and Powertop
- `sudo apt-get install powertop` to install powertop
    - `powertop` gives you a energy-consumption overview similar to `top`
    - but still wasn't giving me enough information
    - and wasnt doing anything about the consumption problems

so i looked into installing tlp and followed [this guide](http://www.webupd8.org/2015/02/advanced-power-management-tool-tlp-sees.html)
- tlp helps reduce unnecessary power usage by devices & processes
- tlp-rdw achieves similar goal, but for radio devices
- tlp can be configured by editing `/etc/default/tlp`.  changes take effect on service restart or device reboot
- more details on [tlp configuration](http://linrunner.de/en/tlp/docs/tlp-configuration.html) available at linrunner

```
sudo apt-get purge laptop-mode-tools

sudo add-apt-repository ppa:linrunner/tlp
sudo apt-get update
sudo apt-get install tlp tlp-rdw

# and for radio device management
sudo apt-get install

sudo tlp start
```

### Livestreaming/OBS

Basically, followed the instructions at
[Livecoding.tv's Linux Guide](https://www.livecoding.tv/obsguidelinux/).
Fairly straightforward to get OBS running.  Started it up with `obs`.
But the webcam didn't work.  Described in the misc hardware issues
section above.


... except there's lots of tearing from desktop streaming....

need to build obs-studio from source... which requires ffmpeg to be
built from source ... since ubuntu 14.04 doesn't include proper
ffmpeg.......

### install tensorflow


#### install bazel

- required java 8.
    - java 8 isnt availble on standard apt repoś for ubuntu 14.04
    - but the .deb package from oracle was straightforward enough
- after getting java, bazel install was straigtforward

#### CUDA 7.0

- needed to download and set this up
- fairly straightforward, no dependencies (yay!)
- after running dpkg on the cuda deb, i ran apt-get update
- then `sudo apt-get install cuda` ended up installing 7.5
  - not sure this will work with my graphics card compute capability (3.0)
  - but we'll see

#### cuDNN 6.5

- nvidia requires registration?  wtf lol
  - two days later, i'm still waiting for this
- where is 6.5? i only see cuDNN 3.0
- finally. i'm approved for cuDNN
  - trying to figure out which cuDNN version i need.
    - both 3 and 4 require CUDA compute capability 3.0+
    - going to with 4
- downloaded and unzipped
  - moved the headers and .so's to the /usr/local/cuda folders

#### install docker

- moving on to install docker and get started with tensorflow
    - while i wait for nvidia to respond (they're taking their time)
- in ubuntu 14.04, the standard ppa's only list docker 1.15
    - i don't want to mess with gpu stuff in a container if i'm not going on the most recent version of docker
- mostly, i just needed to follow the [docker install guide for ubuntu](https://docs.docker.com/engine/installation/ubuntulinux/)
    - to ensure that i'm pulling docker from the right repo's to get `docker 1.19`

#### installing tensorflow docker images



### Installing OpenCV with GPU

wow what a pain in the ass.  QtOpenGL kept fucking it up.  eventually
needed to install without GUI `-DWITH_QT=NO -DWITH_GDK=NO
-DWITH_VTK=NO`

[OpenCV build option reference here](https://github.com/Itseez/opencv/blob/master/CMakeLists.txt)

build options below.  takes about 30 minutes. i should probably have
set some of this stuff off.

```shell
cmake \
 -DCMAKE_BUILD_TYPE=RELEASE \
 -DCMAKE_INSTALL_PREFIX=/usr/local \
 -DWITH_TBB=ON \
 -DBUILD_NEW_PYTHON_SUPPORT=ON \
 -DWITH_V4L=OFF \
 -DINSTALL_C_EXAMPLES=OFF \
 -DINSTALL_PYTHON_EXAMPLES=OFF \
 -DBUILD_EXAMPLES=OFF \
 -DWITH_OPENGL=ON \
 -DMAKE_VERBOSE_MAKEFILE=ON \
 -DWITH_FFMPEG=OFF \
 -DCUDA_ARCH_BIN=3.0 \
 -DWITH_QT=OFF \
 -DWITH_GTK=OFF \
 -DWITH_VTK=OFF .. > cmake.log
```

built without python libraries: see cmake.log below. hope i don't have
to rebuild again.

```
--   Python 2:
--     Interpreter:                 /usr/bin/python2.7 (ver 2.7.6)
--     Libraries:                   NO
--     numpy:                       /usr/local/lib/python2.7/dist-packages/numpy/core/include (ver 1.10.2)
--     packages path:               lib/python2.7/dist-packages
```

of course, it fails building the GPU examples...  . . . finally got it
to work, disabled building the examples.

i can read images in, but can't display anything **womp womp**
... "GUI??1 i don't need no stinkin' GUI!!" - direct quote from 2
hours ago

trying again with GUI.  searched from qt5 opengl package.  hopefully
qt gui doesn't !@#$ it up this time.  i guess i'll find out in 30
minutes when the build finally breaks.

```shell
cmake \
 -DCMAKE_BUILD_TYPE=RELEASE \
 -DCMAKE_INSTALL_PREFIX=/usr/local \
 -DWITH_TBB=ON \
 -DBUILD_NEW_PYTHON_SUPPORT=ON \
 -DWITH_V4L=OFF \
 -DINSTALL_C_EXAMPLES=OFF \
 -DINSTALL_PYTHON_EXAMPLES=OFF \
 -DBUILD_EXAMPLES=OFF \
 -DWITH_OPENGL=ON \
 -DMAKE_VERBOSE_MAKEFILE=ON \
 -DWITH_FFMPEG=ON \
 -DCUDA_ARCH_BIN=3.0 \
 -DWITH_QT=ON \
 -DWITH_GTK=OFF \
 -DWITH_VTK=OFF .. > cmake.log
```

ugh... finally, that did it.  i can open images and stuff.  must have
had the wrong version of qt5-opengl (probably qt4-opengl from a
copy&paste...)

### misc todo:
- keyboard config
- launcher config (gnome do, synergisticafdsafoewa?)
- identify files in .config/* to port to dotfiles
  - e.g. terminator in .config/terminator/config
- fix gnome-do (turned off as a startup service)
  - the application seems to start up,
  - but then process dies after one or two invocations

### emacs notes:
- undo-tree: hit q to complete undo's
- markdown preview?
- help window pops up to suggest other viable key bindings
    - if you wait after pressing a key
- using magit

### emacs todo:
- setup thesaurus
- figure out a system of saving workspaces
- setup ein (emacs-ipython-notebook)
