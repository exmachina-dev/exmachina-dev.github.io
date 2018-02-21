---
layout: post
title:  "Vim installation"
author: "WillyKaze"
date:   2018-01-11 11:24:00 +0100
thumbnail: file-code
categories: Tutorial
tags: Vim IDE
---
This is a quick tutorial to install the Vim text editor with a set of useful plugins.
![Vim, the most powerful editor](http://www.vim.org/images/vim_drill_small.JPG)

# Requirements

* [Vim 8.0 for windows][vim80-win]: Select the
  `gvim80.exe`
* [Python 3.5][python35]:
  Be sure to select Python 3.5 release, Vim 8.0 is compiled with Python 3.5 support
* [Git][git]: Add git to your `PATH`

# Installation

## Installing requirements

- Start by installing Python 3.5.
  Be sure to select `Install for all users` and `Add Python to your PATH`.
- Install Vim
- Install Git. Don't forget to check `▢ Add git to your PATH`  
  You may have to logout/reboot to update the PATH variable.

## dotfiles git repository

This repository contains all the configuration file for Vim and plugins. 
Its installation is optional but recommended. Those configuration files have been
heavily tested and provide a good user experience from scratch.

You can fork the [willykaze/dotfiles.git][WK-dotfiles] repository.
Once it is done, clone it in your repos directory:
```sh
$ cd ~/repos
$ git clone git@gitlab.leone.exm:USERNAME/dotfiles.git --recursive
```

This will clone the repository and fetch all plugins for you.

Autoloaded plugins are listed in [`dotfiles/vim/bundle`][WK-dotfiles-bundle]

## Vim configuration

Open the `vimrc.lunes` file at line 25 and replace `WillyKaze` by your username.
Also adjust the repo folders:
```vimscript
if has("win32")
    let user_dir = "C:/Users/Willykaze"
    let repos_dir = "D:/Users/Willykaze/repos"
    let &runtimepath = expand(repos_dir.'/dotfiles/vim').','.expand(&runtimepath)
else
    let user_dir = "/c/Users/Willykaze"
    let repos_dir = "/d/Users/Willykaze/repos"
    let &runtimepath = expand(repos_dir.'/dotfiles/vim').','.expand(&runtimepath)
endif
```

Update the template variables in line 225:
```vimscript
" Configure templates
let g:user      = "Name FamilyName
let g:email     = "your.mail@mail.com"
```


You will also want to comment lines 192 and 211 if you don't use a bépo keyboard.

```vimscript
if has('win32')
    " Smart tab completion
    " execute 'source '.fnameescape(repos_dir."\\dotfiles\\vim\\vimrc.tabcompletion")

    " Change some keybinds for bépo layout
    "execute 'source '.fnameescape(repos_dir."\\dotfiles\\vim\\vimrc.bepo")

    " Custom shortcuts
    execute 'source '.fnameescape(repos_dir."\\dotfiles\\vim\\vimrc.shorts")

    " Python mode configuration
    execute 'source '.fnameescape(repos_dir."\\dotfiles\\vim\\vimrc.pymode")

    " Shell configuration
    execute 'source '.fnameescape(repos_dir."\\dotfiles\\vim\\vimrc.shell")
    "set shell=powershell
    "set shellcmdflag=-command

    set wildignore+=*\\tmp\\*,*.swp,*.zip,*.exe  " Windows
else
    " Smart tab completion
    " execute 'source '.fnameescape(repos_dir."/dotfiles/vim/vimrc.tabcompletion")

    " Change some keybinds for bépo layout
    "execute 'source '.fnameescape(repos_dir."/dotfiles/vim/vimrc.bepo")

    " Custom shortcuts
    execute 'source '.fnameescape(repos_dir."/dotfiles/vim/vimrc.shorts")

    " Python mode configuration
    execute 'source '.fnameescape(repos_dir."/dotfiles/vim/vimrc.pymode")

    " Shell configuration
    execute 'source '.fnameescape(repos_dir."/dotfiles/vim/vimrc.shell")

    set wildignore+=*/tmp/*,*.so,*.swp,*.zip     " MacOSX/Linux
endif
```

Commit and copy this file to `C:/Users/LOGIN/.vimrc`

Finally you can start vim and check if everything is OK.  
On windows, it's best to run vim in graphical mode: `gvim.exe`

[vim80-win]: https://bintray.com/micbou/generic/vim
[python35]: https://www.python.org/downloads/release/python-354/
[git]: https://git-scm.com/downloads/
[WK-dotfiles]: https://gitlab.exmachina.fr/willykaze/dotfiles
[WK-dotfiles-repo]: git@gitlab.exmachina.fr:willykaze/dotfiles.git
[WK-dotfiles-bundle]: https://gitlab.exmachina.fr/willykaze/dotfiles/tree/master/vim/bundle
