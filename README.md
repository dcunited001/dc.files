dc.files
==========

working on centralizing some scripts and profile data.

Install:
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
- remove plist symlinks
- mac install gnu-getopts
- finish output-error-or-die()
- finish convert-plist()
- convert iterm plist to xml
- convert keyremap plist to xml

### general
- consolidate folders inside ~/.files/
- licenses?
- add function to list modules?
- find a good place for misc scripts
- manage confs for different platforms in branches?
- gitignores (necessary?)

### automator
- find way to symlink these services
- automator/applescripts/services to switch focus to apps (osx only)
- find shortcuts configuration files
- document shortcuts & automate configuration
- git module?
- copy-automator-services(): init script function

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
- mac osx spaces pref.plisp

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

### aliases
- complete?

### subl
- grab sublime profile (ubuntu)
- link sublime profiles
- best way to manage sublime packages?
- manage sublime package preferences?

