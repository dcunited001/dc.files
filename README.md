dc.files
==========

working on centralizing some scripts and profile data.

Install
---------

everything is contained in submodules:
- git clone https://github.com/dcunited001/dc.files
- git submodule init
- git submodule update
- cd ~/.files
- bash init/mac.sh

TODO:
--------

### next
- mac install gnu-getopts
- finish output-error-or-die()
- finish convert-plist()

### general
- finalize format for settings common to all (.aliases.all,
.zshrc.all, etc)
- consolidate folders inside ~/.files/
- licenses?
- move init scripts to modules (and function to list modules?)
- find a good place for misc scripts
- manage confs for different platforms in branches
- way to manage aliases for both bash/zsh in one location
  (necessary?)

### bash
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

### emacs
- basic config files
- create submodules
- basic key bindings
- xiki config
- plugins/scripts
- link: .el4r? (xiki,emacs..)
- aquamacs?

### gitconf
- script gitconfig erb
- FIX GITHUB CONF CREDS

### init
- script to autoload all for mac
- script to autoload all for ubuntu/linux
- convert-mac-pref-file()
- .plist - store both bin and xml?
- possibly migrate scripts to ruby/chef/puppet??
- yaml/json to configure vars needed?
- functions to backup config files
 
### iterm
- convert plist to xml?
- add fonts/colors to repo?
- link fonts/colors?
- find other color profiles
- change iterm plist to load prefs from folder

### janus
- move to vim folder
- best way to link janus' vim plugins?

### kbd
- find old key-remap files
- find os-level mac osx keyboard shortcut files
- any way to find app-level mac osx keyboard shortcut files
- are these linkable in mac osx
- find os-level ubuntu keyboard shortcut files
- app level ubuntu keyboard shortcut files
- link to KR.private.xml
- point to documentation for KeyRemap.private.xml?
- script setup all mac shortcuts
- script setup all ubuntu shortcuts
- add KeyRemap multitouch plist
- possible to change esc to .. something useful?
  (change to add'l mod key for workspace switching/app mgmt)
- document janus bindings in vim bindings file

### omz
- rename ~/.files/zsh/.omz to .oh-my-zsh?
- move omz submodule inside ~/.files/zsh?

### ryanb
- move submodule? where?

### subl
- grab sublime profile (ubuntu)
- link sublime profiles
- best way to manage sublime packages?
- manage sublime package preferences?

### tmux
- check out tmux-powerline https://github.com/erikw/tmux-powerline
- finish scripts/configs
- fix scrolling
- fix copy&paste
- move key bindings (source external file?)
- setup tmuxinator
- tmuxinator templates
- where 2 keep tmuxinator configs (should this be managed?)

### vim
- list of vim commands to focus on learning
- list of vim commands from janus
- fix behavior of repeat-cmd (.)
- turn off word wrap?
- plugins folder (~/.janus?)
- vim-markdown-preview plugin
- config tpope vimmarkdown
- basic cmds http://www.tuxfiles.org/linuxhelp/vimcheat.html
- sub/change http://jeetworks.org/node/84
- word-wrap off

### zsh
- basic aliases
- stub remaining scripts
- permissions
- add .all scripts (zshrc.all, etc)
- script to link .aliases.all
- script to link .zshrc.all
- .zshenv
- .zprofile
- .zshrc
- .zlogin
- $ZDOTDIR/.zlogout
- /etc/zshenv
- /etc/zprofile
- /etc/zshrc
- /etc/zlogin
- /etc/zlogout
- automatically configure .zsh profile fonts/colors
- zsh add new keys for navigating tab-complete
- zsh add new char for !last-cmd-autocomplete?
- add modkey for hard to reach symbol chars?

### other files to manage
- good location for misc files? (preserve modularity while
simplifying)
- .inputrc
- top config
- mac osx spaces pref.plisp

### misc scripts/functions
- clear all history
- ideas?
