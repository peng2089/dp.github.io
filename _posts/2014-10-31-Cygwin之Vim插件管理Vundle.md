---
layout: post
title: Cygwin 之 Vim插件管理Vundle
description: Cygwin 之 Vim插件管理Vundle
tags: [ Cygwin,Vundle ]
---

> Vundle - vim插件管理工具

**安装**

    # git clone https://github.com/gmarik/Vundle.vim.git ~/.vim/bundle/Vundle.vim


报错:

    fatal: destination path '/home/ioioj5/.vim/bundle/Vundle.vim' already exists and is not an empty directory.


如果仅仅是这个错误的话,到没什么.可以换个方式进行.但是在安装完成后要安装vundle插件的时候 同样出现了啦这个问题,要知道好多插件都是git方式安装的.

在虚拟机(CentOS)上装了一遍,很正常没有任何错误,不论是vundle还是插件的安装都很正常.

百度 谷歌了好多,有说是Cygwin的bug,有说是git clone 非空目录的问题,总之有很多说法.最后终于找到了原因: 因为本机已经装了git,而Cygwin中没有装.由于环境变量共享,所以虽然Cygwin下也是可以用git,但用的是本机的git,只要在cygwin中重新安装一下git就好了.

来源:http://computers.findincity.net/view/635399295212048058637800/git-clone-does-not-work-in-using-cygwin-to-run-git

**配置**

将下面的配置放入到~/.vimrc文件的顶部, 删除不需要的插件:

    set nocompatible              " be iMproved, required
    filetype off                  " required

    " set the runtime path to include Vundle and initialize
    set rtp+=~/.vim/bundle/Vundle.vim
    call vundle#begin()
    " alternatively, pass a path where Vundle should install plugins
    "call vundle#begin('~/some/path/here')

    " let Vundle manage Vundle, required
    Plugin 'gmarik/Vundle.vim'

    " The following are examples of different formats supported.
    " Keep Plugin commands between vundle#begin/end.
    " plugin on GitHub repo
    Plugin 'tpope/vim-fugitive'
    " plugin from http://vim-scripts.org/vim/scripts.html
    Plugin 'L9'
    " Git plugin not hosted on GitHub
    Plugin 'git://git.wincent.com/command-t.git'
    " git repos on your local machine (i.e. when working on your own plugin)
    Plugin 'file:///home/gmarik/path/to/plugin'
    " The sparkup vim script is in a subdirectory of this repo called vim.
    " Pass the path to set the runtimepath properly.
    Plugin 'rstacruz/sparkup', {'rtp': 'vim/'}
    " Avoid a name conflict with L9
    Plugin 'user/L9', {'name': 'newL9'}

    " All of your Plugins must be added before the following line
    call vundle#end()            " required
    filetype plugin indent on    " required
    " To ignore plugin indent changes, instead use:
    "filetype plugin on
    "
    " Brief help
    " :PluginList       - lists configured plugins
    " :PluginInstall    - installs plugins; append `!` to update or just :PluginUpdate
    " :PluginSearch foo - searches for foo; append `!` to refresh local cache
    " :PluginClean      - confirms removal of unused plugins; append `!` to auto-approve removal
    "
    " see :h vundle for more details or wiki for FAQ
    " Put your non-Plugin stuff after this line


**安装插件**

启动vim 并运行:PluginInstall 即可安装

Vundle : https://github.com/gmarik/Vundle.vim
