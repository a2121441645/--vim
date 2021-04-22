# --vim
vi m
"基础配置
" 不与 Vi 兼容（采用 Vim 自己的操作命令）
set nocompatible

" 设置编码自动识别, 中文引号显示
" set fileencodings=utf-8,cp936,big5,euc-jp,euc-kr,latin1,ucs-bom
set fileencodings=utf-8,gbk
set ambiwidth=double

" 启用鼠标
set mouse=a

"编译界面正常颜色显示
let &t_ut=''


"外观
"编译界面正常颜色显示
let &t_ut=''

" 行号
set nu
set relativenumber

" 字体大小
set guifont=Courier\ new:h20

" 行线
set cursorline

" 设置行宽
set textwidth=80

" 自动折行
set wrap

" 遇指定符号折行，而单词不折
set linebreak

" 垂直滚动，光标距离底部/顶部的位置
set sidescrolloff=5

" 显示键入指令
set showcmd

" 文件类型检测，文本缩进
filetype on
filetype indent on
filetype plugin on
filetype plugin indent on

"缩进显示
set expandtab
set tabstop=2
set shiftwidth=2
set softtabstop=2

set tw=0
set indentexpr=
set backspace=indent,eol,start
set foldmethod=indent
set foldlevel=99 

"如果行尾有多余的空格（包括 Tab 键），该配置将让这些空格显示成可见的小方块。
set list
set listchars=tab:#\ ,trail:#

" 命令行输入提示（按TAB）
set wildmenu

" 高亮
syntax on

"光标不同显示模式au BufReadPost * if line("'\"") > 1 && line("'\"") <= line("$") | exe "normal! g'\"" | endif
let &t_SI = "\<Esc>]50;CursorShape=1\x7"
let &t_SR = "\<Esc>]50;CursorShape=2\x7"
let &t_EI = "\<Esc>]50;CursorShape=0\x7"

"状态栏
set laststatus=2

"vim在当前目录下执行想执行的命令
set autochdir

"神器
au BufReadPost * if line("'\"") > 1 && line("'\"") <= line("$") | exe "normal! g'\"" | endif


"搜索
" 光标遇到圆、方、大括号，自动高亮对应另一个~
set showmatch 

" 搜索时，高亮显示匹配结果
set hlsearch
exec "nohlsearch"

" 输入搜索模式时，每输入一个字符，就自动跳转第一个匹配结果
set incsearch

" 搜索时忽略大小写
set ignorecase

" 搜索大小写精准匹配
set smartcase

" 清除高亮"
let mapleader=" "


"编辑
" 打开英语单词的拼写检查
set spell spelllang=en_us

" 自动切换工作目录
set autochdir

" 出错时，不发出声响
set noerrorbells

" 出错时，发出视觉提示，通常是屏幕闪烁
set visualbell

" Vim需要记住多少次历史操作
set history=1000

" 打开文件监视.如果在编辑过程中文件发生外部改变（比如被别的编辑器编辑了），就会发出提示。
set autoread

" 避免Vim窗口闪动
set novb


"按键自定义
" 非递归
noremap j h
noremap 9 k
noremap k j
noremap U 5j
noremap E 5k
noremap N 7h
noremap I 7l

noremap = nzz
noremap - Nzz
noremap <LEADER><CR> :nohlsearch<CR>

" 递归
map s <nop>
map S :w<CR>
map Q :q<CR>
map R :source $MYVIMRC<CR>
map ; :

map mm :e $MYVIMRC<CR>

"  分屏
map sl:set splitright<CR>:vsplit<CR>
map sj :set nosplitright<CR>:vsplit<CR>
map si :set nosplitbelow<CR>:split<CR>
map sk :set splitbelow<CR>:split<CR>

map <LEADER>l <C-w>l
map <LEADER>i <C-w>k
map <LEADER>j <C-w>h
map <LEADER>k <C-w>j

map sn <C-w>t<C-w>H
map sm <C-w>t<C-w>K

map <down> :res +5<CR>
map <up> :res -5<CR>
map <left> :vertical resize-5<CR>
map <right> :vertical resize+5<CR>

" 新建标签
map tu :tabe<CR>
map tj :-tabnext<CR>
map tl :+tabnext<CR>

" cscope快捷键
noremap <silent> <leader>s :cs f s <C-R><C-W><cr>
noremap <silent> <leader>g :cs f g <C-R><C-W><cr>
noremap <silent> <leader>c :cs f c <C-R><C-W><cr>
" -----------------------------------------------------------------------------
"  < 编译、连接、运行配置 >
"------------------------------------------------------------------------------



if v:version >= 800

    let $GTAGSLABEL = 'native-pygments'
    " 实现gtags的多语言支持，必须是绝对路径
    let $GTAGSCONF = '/path/to/gtags.conf'

    " Gutentags : update tags database automatically
    let g:gutentags_project_root = ['.root', '.svn', '.git', '.hg', '.project']
    " 指定tag文件保存路径为~/.tags/
    let g:gutentags_ctags_tagfile = '.tags'
    let g:gutentags_modules = []
    if executable('gtags-cscope') && executable('gtags')
		let g:gutentags_modules += ['gtags_cscope']
    endif
    let g:gutentags_cache_dir = expand('~/.cache/tags')
    let g:gutentags_ctags_extra_args = ['--fields=+niazS', '--extra=+q']
    let g:gutentags_ctags_extra_args += ['--c++-kinds=+px']
    let g:gutentags_ctags_extra_args += ['--c-kinds=+px']
    let g:gutentags_ctags_extra_args += ['--output-format=e-ctags']
    " Vim打开后自动连接cscope数据库。当同一Vim打开多个project时易发生冲突
    let g:gutentags_auto_add_gtags_cscope = 1
endif


" Use the internal diff if available.
" Otherwise use the special 'diffexpr' for Windows.
if &diffopt !~# 'internal'
  set diffexpr=MyDiff()
endif
function MyDiff()
  let opt = '-a --binary '
  if &diffopt =~ 'icase' | let opt = opt . '-i ' | endif
  if &diffopt =~ 'iwhite' | let opt = opt . '-b ' | endif
  let arg1 = v:fname_in
  if arg1 =~ ' ' | let arg1 = '"' . arg1 . '"' | endif
  let arg1 = substitute(arg1, '!', '\!', 'g')
  let arg2 = v:fname_new
  if arg2 =~ ' ' | let arg2 = '"' . arg2 . '"' | endif
  let arg2 = substitute(arg2, '!', '\!', 'g')
  let arg3 = v:fname_out
  if arg3 =~ ' ' | let arg3 = '"' . arg3 . '"' | endif
  let arg3 = substitute(arg3, '!', '\!', 'g')
  if $VIMRUNTIME =~ ' '
    if &sh =~ '\<cmd'
      if empty(&shellxquote)
        let l:shxq_sav = ''
        set shellxquote&
      endif
      let cmd = '"' . $VIMRUNTIME . '\diff"'
    else
      let cmd = substitute($VIMRUNTIME, ' ', '" ', '') . '\diff"'
    endif
  else
    let cmd = $VIMRUNTIME . '\diff'
  endif
  let cmd = substitute(cmd, '!', '\!', 'g')
  silent execute '!' . cmd . ' ' . opt . arg1 . ' ' . arg2 . ' > ' . arg3
  if exists('l:shxq_sav')
    let &shellxquote=l:shxq_sav
  endif
endfunction


" Specify a directory for plugins
" - For Neovim: stdpath('data') . '/plugged'
" - Avoid using standard Vim directory names like 'plugin'
call plug#begin('~/.vim/plugged')

Plug 'mhinz/vim-startify'

Plug 'vim-airline/vim-airline'

Plug 'Yggdroot/indentLine'

Plug 'connorholyday/vim-snazzy'

" Python
Plug 'vim-scripts/indentpython.vim'

" Bookmarks
Plug 'kshenoy/vim-signature'


" File navigation
Plug 'preservim/nerdtree'
Plug 'Xuyuanp/nerdtree-git-plugin'

" Markdown
Plug 'iamcco/markdown-preview.vim'
Plug 'dhruvasagar/vim-table-mode', { 'on': 'TableModeToggle' }
Plug 'vimwiki/vimwiki'

 " 安装vim-gutentags
    Plug 'ludovicchabant/vim-gutentags'

Plug 'puremourning/vimspector'

" Initialize plugin system
call plug#end()
