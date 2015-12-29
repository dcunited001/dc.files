dc.files
==========

working on centralizing some scripts and profile data.

I'm hoping to restructure these dotfiles soon and, most crucial, update
the mess of documentation here.  In the meantime, check out the [awesome
dotfiles](https://github.com/webpro/awesome-dotfiles) and [github
dotfiles](https://dotfiles.github.io/) repo's for ideas on how to manage
your own dotfiles.

I like some of the ideas i used to organize mine -- like using git
submodules to organize them.  And now, in 2015, i'm seeing a ton more
of that than I did in 2012.  I'm hoping to find a tool that allows me to
manage the changes to dotfiles in submodules much easier than it is now.
As they are organized now, managing changes to dotfile modules for 
multiple platforms and especially pushing
distributing out the changes to my multiple platforms is really time
consuming.

Ideas
-----

I'm thinking about breaking dc.files and each submodule into branches for each platform,
instead of having different files for each platform.  So, for most submodules like alias/init/etc,
all of the platform-specific vars would be abstracted into a single file which is ran beforehand.
Then the vars are used where they can be and Platform-Specific aliases/functions can be sourced in
their own files.

I would also like to add some kind of dependency-management, so you can load in just the functions/etc
that you need.  But there's got to be another tool to help manage these things.

OSX Install Notes:
---------

i recently reinstalled OSX onto a fresh SSD.  I configured first through pivotal-sprout, then merged in my configs.  I detail this process below.

> Note: These are my personal dotfiles.  There's a ton of good resources and ideas in here, but don't expect everything to just work!  Besides, templates and dotfiles are only useful if you have a mental model of them in your head.  That badass emacs/vim config is worthless if you don't know it.  So read teh codez!!1

setup OSX with [https://github.com/pivotal-sprout/sprout-wrap](Pivotal Sprout):
1. install git
1. restore ssh keys
1. `git clone git@github.com:pivotal-sprout/sprout-wrap sprout-wrap`
1. `cd !$` #thank me later if you didn't know that one
1. `sudo gem install bundler`
1. `bundle`
1. `bundle exec soloist`
1. buy a beer for the guys at pivotal

everything is contained in submodules:
1. `git clone https://github.com/dcunited001/dc.files .files`
1. `cd ~/.files`
1. `git submodule init`
1. `git submodule update`
1. `bash init/mac.sh` #run init scripts - links most files - some errors =/
1. `chsh -s /bin/zsh` #set zsh as default
1. `cp ~/.files/zsh/zshenv.mac /etc/zshenv` # sets .zsh as $ZDOTDIR

other setup:
- backed up all configs generated through pivotal sprout (bash,zsh,vim,etc)
- manually set up git config (you can use the erb in init/mac.sh)
- ran into problems with rake on .files/init/mac.sh setup for janus.
- disabled most OSX shortcuts and (reconfigured through applications, quicksilver & key remap)
- installed misc apps:
  - [PCKeyboardHack](https://pqrs.org/macosx/keyremap4macbook/pckeyboardhack.html.en)
  - [KeyRemap4MacBook](https://pqrs.org/macosx/keyremap4macbook/)
  - [Quicksilver](http://qsapp.com/)
  - [Emacs for OSX](http://emacsformacosx.com/) `brew install --cocoa emacs`
  - [Choosy](http://www.choosyosx.com/) ($12/trial)
  - [Divvy](http://mizage.com/divvy/) ($15/trial)
  - [FluidApps](http://fluidapp.com/) ($5/trial) (mostly using my create-ssb function now, ~/.files/func.mac.sh)
  - [Paragon ExtFS Drivers for OSX](http://www.paragon-software.com/home/extfs-mac/) ($40/trial)
- Configured PCKeyboardHack
  - Caps => Escape
  - Escape => Command R
  - Command R => Command L
- Configured KeyRemapForMacbook
  - `cp ~/.files/kbd/KeyRemap4Macbook/*.xml ~/Library/Application\ Support/KeyRemap4MacBook/`
  - copied over Key Remap plists to "/Users/dc/Application\ Support & private.xml
  - enabled config options
- Configured Quicksilver and a few plugins
  - configured Quicksilver with Shortcuts to open apps
- Configured Choosy
- link .zsh/prompt => ~/.files/zsh/prompt
- setup Agnoster theme for iTerm
  - added Powerline Fonts with Font Book (fonts in .files/iterm/fonts)
  - setup a powerline font in iTerm for the Agnoster ZSH Theme
  - setup Solarize Color Theme for the Agnoster ZSH Theme
  - setup iTerm prferences to load from ~/.files/iterm/com.googlecode.iterm.plist
- Configured Emacs
  - `brew install --cocoa emacs`
  - `ln -s /usr/local/Cellar/emacs/24.3/Emacs.app /Applications/Emacs.app` #check version
  - `echo "alias emacs='/Applications/Emacs.app/Contents/MacOS/Emacs -nw'" >> .zsh/.zshrc` #now exists
- Installed Emacs Packages (most are not configured in init.el yet)
  - ergoemacs-mode
  - rvm
  - rinari
  - ess
  - yasnippet
  - twittering-mode
  - ack
  - aes
  - bundler
  - bitly
  - closure-mode
  - cmake-mode
  - coffee-mode
  - db-pg
  - flymake packages
  - gnugo
  - spaces
  - sml-mode
  - slime
  - scss-mode
  - sass-mode,
  - ruby-test-mode
  - ruby-mode
  - ruby-compilation
  - json

- configured Site-Specific Browsers:
  - Gmail (Voice, Calendar, Etc)
  - Dev (Github, Airbrake, Semaphore, Heroku, Amazon, etc)
  - Rails (localhost:3000)
  - RspecWeb (localhost:4567)

recommended key remap settings
- Uber Key
  - remap CMD_R to Uber Key
- Digits & Symbols:
  - ConsumerKeys => Digits
  - Digits => Symbols
  - Uber+ConsumerKeys => ConsumerKeys
- Function Key
  - Fn => PC Application Key (iTerm Only)
- Unithumbability
  - Shift+Space => Underscore (if you don't like this, you don't like ruby)
  - Simultaneous Space => <{~"|}> (sweet!)

Ubuntu **13.04** Install Notes:
----------

NVidia Drivers - *applicable to laptops both integrated graphics and a GPU*

- `sudo apt-get install build-essential linux-source linux-headers`
  - i had to do linux-headers-\`uname -r\` for 13.04
- `sudo apt-get install nvidia-current`
  - installs nvidia drivers, installation passed, config failed bc nouveau drivers were already there
- `sudo /sbin/lsmod | grep nvidia`
  - check whether the nvidia drivers were configured to be used, they weren't
- `sudo depmod -a`
  - ensure dependencies are satisfied
- `sudo modprobe nvidia_current`
- after this point, i believe a restart kicked in the nvidia drivers

Dependencies (using sudo apt-get install)

- build-essential linux-source linux-headers
- autoconf vim git curl perl tmux
- guake terminator terminology 
  - terminology requires a ppa repo to be added
  - looks cool, but isnt worth it
- compizconfig-settings-manager
- gawk libreadline6-dev zlib1g-dev libssl-dev
- libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev
- libgdbm-dev libncurses5-dev libtool bison libffi
- launchy

Dotfiles

1. clone repo
1. `git submodule init`
1. `git submodule update`
1. ensure curl, git, ruby & perl are installed (dep-checks fail if not)
1. change username & home path in `init/support`
1. `bash init/ubu.sh`
  - setup-emacs
  - setup-kbd
  - setup-ryanb
  - setup-omz
1. `chsh -s /bin/zsh` #set zsh as default
1. `cp ~/.files/zsh/zshenv.mac /etc/zshenv` # sets .zsh as $ZDOTDIR

Keyboard Layout

> Note the keyboard layouts are hardware specific and this one is for an Asus UL50vt.

1. `echo "xmodmap ~/.Xmodmap" >> ~/.xinitrc`
1. restart or logout/login or `xmodmap ~/.Xmodmap`

You can set up Xmodmap to load for all users, but i was running into some issues:
1. `sudo cp ~/.files/kbd/Xmodmap.ubu /etc/X11/xinit/Xmodmaprc`
1. `sudo vim /etc/X11/xinit/xinitrc`
  - add `xmodmap /etc/X11/xinit/Xmodmaprc` to this file
1. restart
  - then find out that this doesn't work hmmmm..
1. `xmodmap ~/Xmodmap` after logging in


Terminal & Emacs

- `mkdir ~/.fonts`
- `cp ~/.files/iterm/fonts/* ~/.fonts`
- set default shell & set terminal to run a login shell, if u want
- `rm ~/.files/emacs/support/bindkeys.el`
- `ln -s ~/.files/emacs/support/bindkeys.el ~/.files/kbd/bindkeys.emacs.mac`
  - i'm working on putting most of the config into json
  - also probably moving to use branches for separate platforms
- install the emacs packages listed above.
  - the init-script will crash until you do
  - `emacs --debug-init .` until it passes
  - `M-x list-packages` to show packages, pick the right ones
  - shift-left/up/down/right to change buffers
- install sshmenu
  - `sudo apt-get install sshmenu`
  - `git clone git@github.com:jowolf/sshmenu`

## NOTE:
> i've recently noticed that every repo/submodule config in the project
> is set up using SSH, instead of HTTPS.  So you will be able to clone
> the repo, but you will will not be able to update the submodules,
> unless git/github have your SSH pub/private keys properly set up.
> you'll notice in the git configs that everything is set up with
> git@github.com, instead of https://github.com.

TODO:
--------

### next
- remove plist symlinks
- mac install gnu-getopts
- finish output-error-or-die()
- finish convert-plist()
- convert iterm plist to xml
- convert keyremap plist to xml
- create osx repo and add defaults scripts
- move automator repo to osx

### tasks
- [X] complete
- [ ] incomplete

### general
- turn todo lists into Github Markdown Tasks
- consolidate folders inside ~/.files/
- licenses?
- add function to list modules?
- find a good place for misc scripts
- manage confs for different platforms in branches?
- gitignores (necessary?)
- add .hushlogin?
- convert to chef repos

### aliases
- turn OSX ssh on/off for user
- turn OSX filesharing on/off for user?
- alias to commit changes to dc.files git modules & auto commit dc.files parent repo
### osx
- ichat plugin for campfire (http://g-off.github.com/Campfire/)
- osx init script?
- document settings missing from osx.sh
- osx init script?
- add new automator services
- create list of plist files
- add system plists
- add application plists
- script to automate backing up all plists and individual plists
- document installed applications
- find way to symlink automator services
- automator/applescripts/services to switch focus to apps (osx only)
- find shortcuts configuration files
- document shortcuts & automate configuration
- will automator scripts even work if they are simply copied??
- copy-automator-services(): init script function
- automator bluetooth proximity script
http://emmanuelbernard.com/blog/2010/04/01/automatically-lock-your-computer-when-you-go-away/
- fix script to reload private.xml (this isn't working?)
- automator script to automate reloading keyremap private.xml

### zsh
- remove case sensitivity?
- review oh-my-zsh aliases
- finish basic aliases
- permissions
- /etc/zshenv (link or copy?)
- /etc/zprofile (link or copy?)
- /etc/zshrc (link or copy?)
- /etc/zlogin (link or copy?)
- /etc/zlogout (link or copy?)
- automatically configure .zsh profile fonts/colors
- zsh add new keys for navigating tab-complete
- zsh add new char for !last-cmd-autocomplete?
- add modkey for hard to reach symbol chars?

### init
- osx init script?
- script to autoload all for mac
- script to autoload all for ubuntu/linux
- .plist - store both bin and xml?
- possibly migrate scripts to ruby/chef/puppet??
- yaml/json to configure vars needed?
- functions to backup config files

### iterm
- setup to work with external settings, but backup .plist
- add fonts/colors to repo?
- link fonts/colors?
- find other color profiles
- change iterm plist to load prefs from folder

### kbd
- address issue with plist files
- find os-level mac osx keyboard shortcut files
  (com.apple.symbolichotkeys.plist)
- any way to find app-level mac osx keyboard shortcut files
- are these linkable in mac osx
- find os-level ubuntu keyboard shortcut files
- app level ubuntu keyboard shortcut files
- link to KR.private.xml?
- point to documentation for KeyRemap.private.xml?
- script setup all mac shortcuts
- script setup all ubuntu shortcuts
- possible to change esc to .. something useful?
  (change to add'l mod key for workspace switching/app mgmt)
- document janus bindings in vim bindings file

### vim
- make the janus link function also create the .vim dir
- list of vim commands to focus on learning
- list of vim commands from janus
- plugins folder (~/.janus?)
- config vim-markdown-preview
- config tpope vimmarkdown
- basic cmds http://www.tuxfiles.org/linuxhelp/vimcheat.html
- sub/change http://jeetworks.org/node/84

Later
-----------

### other files to manage
- good location for misc files? (preserve modularity while simplifying)
- .inputrc
- top config
- mac osx spaces pref.plist

### misc scripts/functions
- clear all history
- ideas?

### tmux
- check out tmux-powerline https://github.com/erikw/tmux-powerline
- finish scripts/configs
- fix scrolling
- fix copy&paste
- move key bindings (source external file?)
- setup tmuxinator
- tmuxinator templates
- where 2 keep tmuxinator configs (should this be managed?)

### emacs
- basic config files
- create submodules
- basic key bindings
- xiki config
- plugins/scripts
- link: .el4r? (xiki,emacs..)
- aquamacs?

### bash
- update to use a bash dot dir
- basic aliases
- stub remaining scripts
- permissions
- add .all scripts (zshrc.all, etc)
- bash_profile
- bashrc
- bash_logout (clear, history?)
- /etc/bashrc
- /etc/profile script?
- /etc/profile.d folder? http://www.linuxfromscratch.org/blfs/view/6.3/postlfs/profile.html
- /etc/dircolors? dircolors -p > /etc/dircolors

### omz
- rename ~/.files/zsh/.omz to .oh-my-zsh?
- move omz submodule inside ~/.files/zsh?

### ryanb
- move submodule? where?

### janus
- move to vim folder & update scripts
- best way to link janus' vim plugins?

### gitconf
- script gitconfig erb

### subl
- grab sublime profile (ubuntu)
- link sublime profiles
- best way to manage sublime packages?
- manage sublime package preferences?
