dc.files
==========

working on centralizing some scripts and profile data.

Install:
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

other things i needed to do to setup:
- backed up all configs generated through pivotal sprout (bash,zsh,vim,etc)
- manually set up git config (you can use the erb in init/mac.sh)
- disabled most OSX shortcuts and (reconfigured through applications, quicksilver & key remap)
- installed Divvy
- installed PCKeyboardHack 
  - configured escape, caps & command_r
- installed KeyRemapForMacbook 
  - copied over Key Remap plists to "/Users/dc/Application\ Support & private.xml
  - enabled config options
- installed Quicksilver and a few plugins
  - configured Quicksilver with Shortcuts to open apps
- ran into problems with rake on init/mac.sh setup for janus.
- link .zsh/prompt => ~/.files/zsh/prompt
- setup Agnoster theme for iTerm
  - added Powerline Fonts with Font Book (fonts in .files/iterm/fonts)
  - setup a powerline font in iTerm for the Agnoster ZSH Theme
  - setup Solarize Color Theme for the Agnoster ZSH Theme
  - setup iTerm prferences to load from ~/.files/iterm/com.googlecode.iterm.plist
- installed emacs packages
  - 
- configured Site-Specific Browsers:
  - Gmail (Voice, Calendar, Etc)
  - Dev (Github, Airbrake, Semaphore, Heroku, Amazon, etc)
  - Rails (localhost:3000)
  - RspecWeb (localhost:4567)

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

### general
- consolidate folders inside ~/.files/
- licenses?
- add function to list modules?
- find a good place for misc scripts
- manage confs for different platforms in branches?
- gitignores (necessary?)
- add .hushlogin?

### aliases
- turn OSX ssh on/off for user
- turn OSX filesharing on/off for user?

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

