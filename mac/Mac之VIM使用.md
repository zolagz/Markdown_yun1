###安装独立的vim替换掉OSX默认

brew install mercurial 
brew install vim


打开高亮渲染

在 ~/.vimrc 中配置

set nonu
syntax on

修复 mac 下 vim 无法使用delete删除文本的问题

在 ~/.vimrc 中配置

" fix mac vim delete error, so as set backspace=indent,eol,start
set backspace=2

安装 Vundle

它的使用方法很简单，安装一个插件只需要在.vimrc按照规则中添加 Plugin 的名称，某些需要添加路径，之后在 Vim 中使用:PluginInstall既可以自动化安装。

    git 克隆 Vundle 工程到本地

git clone https://github.com/gmarik/Vundle.vim.git ~/.vim/bundle/Vundle.vim

    修改.vimrc配置 Plugins。在.vimrc文件中添加如下内容
    
    
    
```

set nocompatible              " be iMproved, required
filetype off                  " required

" set the runtime path to include Vundle and initialize
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()
" alternatively, pass a path where Vundle should install plugins
"call vundle#begin('~/some/path/here')

" let Vundle manage Vundle, required
Plugin 'VundleVim/Vundle.vim'

" The following are examples of different formats supported.
" Keep Plugin commands between vundle#begin/end.

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

```

保存后进入 vim 运行命令

:PluginInstall

#vundle 命令

## 安装插件
:BundleInstall

## 更新插件

:BundleUpdate

## 清除不需要的插件

:BundleClean

## 列出当前的插件

:BundleList

## 搜索插件

:BundleSearch

####注意

插件配置不要在 call vundle#end() 之前，不然插件无法生效
如果配置错误，需要重新配置后，在vim中运行 :PluginInstall

####安装 YouCompleteMe
使用 Vundle 安装 YouCompleteMe

   在.vimrc中添加如下内容 位置在call vundle#begin()和call vundle#end()之间

Bundle 'Valloric/YouCompleteMe'

###在vim中运行命令

:BundleInstall

###编译 YouCompleteMe

编译过程需要CMake

>brew install CMake

