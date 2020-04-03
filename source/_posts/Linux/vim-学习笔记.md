---
title: vim 学习笔记
date: 2016-04-15 14:02:32
categories: Linux
description: 记录 vim 的使用方法及技巧
feature: /images/Linux/vim_logo.png
tags: [vim]
toc: true
---

## vim 常用命令图解

图文并茂地记录 `vim` 各模式下的操作方法及相应快捷键，附带 `vim` 地配制方法及插件安装。

<!--more-->

![20160415_0.png](/images/Linux/20160415_0.png)

![vi-vim-cheat-sheet-sch.gif](/images/Linux/vi-vim-cheat-sheet-sch.gif)

## 命令（快捷键）详解
	1. 编辑模式  

|命令|说明|
|:---:|:---:|
|i,I|i: 从光标所在处开始插入；<br>I: 在光标所在行的第一个非空格字符处开始插入。|  
|a,A|a: 从光标所在处的下一个字符开始插入；<br>A: 在光标所在行的最后一个字符处开始插入。|  
|o,O|o: 在光标所在行的下一行插入新行；<br>O: 在光标所在行的上一行插入新行。|  
|r,R|r: 只取代光标所在处的字符一次；<br>R: 一直取代光标所在处的文字，直到按下 ESC为止。|  

	2. 指令模式
|命令|说明|
|:---:|:---:|
|Esc|进入指令模式|
|:w|将编辑的数据写入到硬盘档案中|
|:w!|若文档属性为 **只读** 时，强制写入文档。能否写入成功与对该文档的权限有关。|
|:q|离开vi|
|:q!|强制离开，不存储文档。|
|:e|重新加载已打开的文件。|
|:e!|放弃所有修改，并打开原来的文件|
|:wq|存储后离开,若为 **:wq!** 则强制存储后离开。|
|ZZ|大写的 **Z**<br>若文档没有改动，则不存储离开；<br>若文档被改动过，则存储后离开。|
|:w [filename]|将文档存储成另一个文档【另存为】。
|:r [filename]|在编辑的数据中读入另一个文档，将【filename】内容添加到所在行后面。|
|:n1,n2 w [filename]|将n1到n2的内容存储到filename文档。|
|:! command|暂时离开vi到指令列模式，执行 command 并显示结果。|
|:set nu/nonu|vim 环境的变更<br>显示行号/取消行号。|

	3. 移动光标
|命令|说明|
|:---:|:---:|
|h 或 ←|光标向左移动一个字符|
|j 或 ↓|光标向下移动一个字符|
|k 或 ↑|光标向上移动一个字符|
|l 或 →|光标向右移动一个字符|
|[Ctrl]+[f]|屏幕【向下】移动一页 == PageDown|
|[Ctrl]+[b]|屏幕【向上】移动一页 == PageUp|
|[Ctrl]+[d]|屏幕【向下】移动半页|
|[Ctrl]+[u]|屏幕【向上】移动半页|
|+|光标移动到下一行首个非空格符处|
|-|光标移动到上一行首个非空格符处|
|n`space`|光标向右移动n个字符（包括空格），当前行剩余字符不足n时，跳至下一行|
|0 或 home|光标移动到当前行的首字符|
|$ 或 end|光标移动到当前行的末字符|
|H|光标移动到屏幕首行首字符|
|M|光标移动到屏幕中间行首字符|
|L|光标移动到屏幕末行首字符|
|G|光标移动到文档末行首字符|
|nG|光标移动到文档第n行首字符|
|gg|光标移动到文档首行首字符|
|n`Enter`|光标向下移动n行|
|b|将光标移动到单词开头|
|e|将光标移动到单词末尾|
|n&#124;|将光标移动到当前行的第 n 列|
|[[ 或 ]]|跳转到函数 `开头` 或 `结尾`|
|{[ 或 ]}|跳转到区块 `开头` 或 `结尾`|

	4. 复制与粘贴
|命令|说明|
|:---:|:---:|
|x,X|x向后(X向前)删除一个字符，Delete(Backspace)|
|nx|向后删除n个字符|
|dd|剪切光标所在的那一整行|
|ndd|剪切光标所在处向下n行|
|d1G|剪切光标所在处到第一行的所有数据|
|dG|剪切光标所在处到最后一行的所有数据|
|d$|剪切光标所在处到该行最后一个字符的所有数据|
|d0|剪切光标所在处到该行第一个字符的所有数据|
|D|剪切光标处到行末的所有数据|
|y|复制选中的内容到剪贴板|
|yy|复制光标所在的那一行|
|nyy|复制光标所在处向下n行|
|y1G|复制光标所在行到第一行的所有数据|
|yG|复制光标所在行到最后一行的所有数据|
|y0|复制光标所在处到该行行首的所有数据|
|y$|复制光标所在处到该行行尾的所有数据|
|p,P|p将已复制的数据粘贴到光标下一行；<br>P将已复制的数据粘贴到光标上一行.|
|J|将光标所在行与下一行的数据结合成一行|
|c|重复删除多个数据。eg:[10cj],向下删除10行|
|u|复原前一个动作|
|[Ctrl]+r|重做上一个动作|
|.|重复前一个动作|
|5.|重复5次前一个动作|
|:1,10 d|将1-10行剪切，可用 p 命令粘贴|
|:1,10 m 20|将 1-10 行移动到 20 行之后|
|:1,10 co 20|将 1-10 拷贝到 20 行之后|
|:1,$ co $|将整篇文档拷贝一份添加到文件末尾|

	5. 搜索与替代
|命令|说明|
|:---:|:---:|
|/word|从光标处向下查找 word 字符串<br>【:nohlsearch 关闭搜索高亮】|
|?word|从光标处向上查找 word 字符串|
|\*|全文查找光标处的单词|
|n|重复前一个搜索动作|
|N|反向重复上一个搜索动作|
|:s/old/new|用 new 替换 old，当前行第一个匹配|
|:s/old/new/g|用 new 替换 old，当前行所有匹配|
|:%s/old/new|用 new 替换 old，所有行第一个匹配|
|:%s/old/new/g|用 new 替换 old，整篇文档的所有匹配|
|:n1,n2s/word1/word2/g<br>:n1,n2s/word1/word2/gc|在n1行与n2行之间寻找word1字符串，并将该字符串替换为word2。<br>在替换前提示用户确认是否需要替换|
|:3,5s/^/#/g<br>:3,5s/^/\/\//g|将3到5行注释掉|
|:3,5s/^#//g<br>:3,5s/^\/\///g|取消3到5行的注释|
|:3,5s/^#/&nbsp;&nbsp;&nbsp;&nbsp;/g|在3到5行行首添加4个空格，用于缩进|
|:%s/^/#/g|注释整个文档，更快|
|%|括号匹配|
|[Ctrl]+a|增加光标处的数字|
|[Ctrl]+x|减小光标处的数字|

    6. 窗口命令
|命令|说明|
|:---:|:---:|
|:open file|在当前窗口打开一个新文件|
|:split file|在新窗口中打开文件（水平分割）|
|:new file|在新窗口中打开文件（水平方向）|
|:vsplit file|在新窗口中打开文件（垂直分割）|
|:bn/:bp|在当前窗口中切换至下/上一个已打开的文件|
|[Ctrl]+ww|移动到下一个窗口|
|[Ctrl]+wh|移动到左方的窗口|
|[Ctrl]+wj|移动到下方的窗口|
|[Ctrl]+wk|移动到上方的窗口|
|[Ctrl]+wl|移动到右方的窗口|
|:only|关闭所有窗口，只保留当前窗口|

    7. 对齐命令
|命令|说明|
|:---:|:---:|
|:%!fmt|对齐所有行|
|!}fmt|对齐光标处的所有行|
|5!!fmt|对齐光标所在处向下 5 行|

[Vim 命令合集](http://www.cnblogs.com/softwaretesting/archive/2011/07/12/2104435.html "http://www.cnblogs.com/softwaretesting/archive/2011/07/12/2104435.html")
[超过130个你需要了解的 vim 命令](http://www.oschina.net/news/43167/130-essential-vim-commands)
[vim 命令](http://lxs647.iteye.com/blog/1245948)

## vim 配置
### 必知必会
* .vimrc example

``` vim
set autoindent
set cindent
set hlsearch
set incsearch
set number
set nocompatible
set noswapfile
set nobackup
set ruler
set showmatch
set expandtab
set tabstop=4
set softtabstop=4
set shiftwidth=4
set smarttab
set showmatch
set laststatus=2
set wildmenu
set scrolloff=3
set nowrap

set completeopt=longest,menu
autocmd FileType python set omnifunc=pythoncomplete#Complete
autocmd FileType javascript set omnifunc=javascriptcomplete#CompleteJS
autocmd FileType html set omnifunc=htmlcomplete#CompleteTags
autocmd FileType css set omnifunc=csscomplete#CompleteCSS
autocmd FileType xml set omnifunc=xmlcomplete#CompleteTags
autocmd FileType java set omnifunc=javacomplete#Complet

set cursorline
"set cursorcolumn

set fileencodings=utf-8,ucs-bom,gb18030,gbk,gb2312,cp936
set termencoding=utf-8
set encoding=utf-8

syntax enable
syntax on
filetype off

set statusline=[%F]\ line=%l/%L\ col=%c]\ [%p%%]

set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()
    Plugin 'VundleVim/Vundle.vim'
    Plugin 'majutsushi/tagbar'
    Plugin 'pboettch/vim-cmake-syntax'
    Plugin 'scrooloose/nerdtree'
    Plugin 'octol/vim-cpp-enhanced-highlight'
    Plugin 'Xuyuanp/nerdtree-git-plugin'
    Plugin 'jistr/vim-nerdtree-tabs'
call vundle#end()

filetype indent on
filetype plugin on
filetype plugin indent on

"------------------ Tagbar Configuration --------------------"
let g:tagbar_ctags_bin='/usr/local/bin/ctags'
let g:tagbar_width=35
autocmd BufReadPost *.cpp,*.c,*.h,*.hpp,*.cc,*.cxx,*.java,*.py call tagbar#autoopen()
"------------------ NERDTree Configuration --------------------"
let NERDTreeShowBookmarks=1
let NERDTreeShowLineNumbers=1
let NERDTreeAutoCenter=1
let NERDTreeShowHidden=1
let NERDTreeWinSize=35
let g:nerdtree_tabs_open_on_console_startup=1
let NERDTreeIgnore=['\.pyc$','\.class$','\~$','\.swp$','\.o$','\.a$','\.so$']
```

## vim nerdtree
``` vim
NERDTree 说明
和编辑文件一样，通过h j k l移动光标定位
打开关闭文件或者目录，如果是文件的话，光标出现在打开的文件中
go 效果同上，不过光标保持在文件目录里，类似预览文件内容的功能
i和s可以水平分割或纵向分割窗口打开文件，前面加g类似go的功能
t 在标签页中打开
T 在后台标签页中打开
ctrl+w+w 光标在左右窗口切换
ctrl+w+r 切换当前窗口左右布局
ctrl+p 模糊搜索文件
gT 切换到前一个tab
g t 切换到后一个tab
p 到上层目录
P 到根目录
K 到同目录第一个节点
J 到同目录最后一个节点
m 显示文件系统菜单（添加、删除、移动操作）
r: 刷新光标所在的目录
R: 刷新当前根路径
? 帮助
q 关闭

扩展插件：
vim-nerdtree-tabs
nerdtree-git-plugin
```

## vim 更多插件
[我的Shell + VIM配置](https://www.cnblogs.com/edward2013/p/5295004.html)

## vim 疑难

### 换行符处理

{% label 问题描述 danger %}
vim 保存后整个文档都被标记为更改，差异对比显示每行末尾出现`^M`字样。

{% label 问题根源 primary %}
Unix 只需一个换行符(\n) `0A`，
DOS/Windows 需要一个回车符加一个换行符(\r\n) `0D0A`。

{% label 解决方案 success %}
``` c
:e ++ff=unix         /*设置 fileformats 为 unix，0D \r 被显示 ^M*/
:%s/[^[:print]]$//g  /*清除行末不可打印字符但不限于 ^M*/
:%s/[[:space]]*$//g  /*清楚行末空白字符*/
:%s/\^M//g           /* ^M == Ctrl+v, Ctrl+m */
```
