这里使用的插件管理工具vim-plug 各系统安装
#### Unix
```
curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```
#### Windows (PowerShell)
```
iwr -useb https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim |`
    ni $HOME/vimfiles/autoload/plug.vim -Force
```

#### 常用命令
- `:PlugInstall` to install the plugins
- `:PlugUpdate` to install or update the plugins
- `:PlugDiff` to review the changes from the last update
- `:PlugClean` to remove plugins no longer in the list

#### vimrc 配置文件
```

call plug#begin('~/.vim/plugged')
" vim主题
Plug 'dracula/vim', { 'as': 'dracula' }
" 状态光标线
Plug 'vim-airline/vim-airline'
Plug 'vim-airline/vim-airline-themes'
" 文档树插件 (leader +e 映射)
Plug 'preservim/nerdtree'
" 启动界面
Plug 'mhinz/vim-startify'
" 界面跳转(Ctrl + hjkl)
Plug 'christoomey/vim-tmux-navigator'
" 注释 leader cc  (解开)leader cu
Plug 'scrooloose/nerdcommenter'
" golang
Plug 'fatih/vim-go', { 'do': ':GoUpdateBinaries' }
" 语法检查
Plug 'dense-analysis/ale'

call plug#end()


" =========================================vim=================================================
" 重新打开文件，光标返回之前所在的位置
if has("autocmd")
    au BufReadPost * if line("'\"") > 1 && line("'\"") <= line("$") | exe "normal! g'\"" | endif
endif
" 开启代码高亮
syntax on
" 启用文件类型检测和相关插件
filetype on
"+y 复制
"+p 
set clipboard=unnamed "和操作系统共享剪切板"  
filetype plugin on
filetype indent on
" 设置编码
set encoding=utf-8
set splitbelow "表示打开的新窗口将位于当前窗口的下方"
set laststatus=2 "表示总是显示状态栏"
set scrolloff=4 "光标移动到窗口底部时，始终保持两行的距离"
set cursorline "字不会超出边框"
set noerrorbells visualbell t_vb= "关闭提示音"
" 设置文件编码
set fenc=utf-8
set number "显示行号"
set cursorline "所在行高亮显示"
set incsearch "高亮选中"
" 进入时执行
exec "nohlsearch" 
" 光标修复
if &term =~ "xterm"
    let &t_SI = "\<Esc>[6 q"
    let &t_SR = "\<Esc>[3 q"
    let &t_EI = "\<Esc>[2 q"
endif
" 按键映射 ============================
" nomal模式
let mapleader=","
" 正常模式下
nnoremap <leader>e :NERDTreeToggle<CR>
nnoremap <leader>pi :PlugInstall<CR>
nnoremap <leader>ps :PlugStatus<CR>
nnoremap <leader>pc :PlugClean<CR>
" 打开新的终端
nnoremap <Leader>t :terminal<CR>
nnoremap H 0
nnoremap K i<CR><ESC>
nnoremap L g_
nnoremap S :w<CR>
nnoremap Q :q<CR>
nmap <leader>l :bnext<CR>
" Move to the previous buffer
nmap <leader>h :bprevious<CR>
"重新载入vimrc
nnoremap R :source $MYVIMRC<CR> 
"取消高亮
nnoremap <leader><CR> :nohlsearch<CR> 
" 上下分屏
nnoremap <leader>s :split<CR>
" 左右分屏
nnoremap <leader>vs :vsplit<CR>


" view模式
vmap  H 0
vmap L g_

" =========================================Plug=================================================
" 界面主题 ============================
syntax enable
colorscheme dracula

"  光标线配置 =========airline=======================
set laststatus=2  "永远显示状态栏
let g:airline_powerline_fonts = 1  " 支持 powerline 字体
let g:airline#extensions#tabline#enabled = 1 " 显示窗口tab和buffer
let g:airline_theme='bubblegum'  " murmur配色不错
if !exists('g:airline_symbols')
let g:airline_symbols = {}
endif
" 图标修改
let g:airline_left_sep = '▶'
let g:airline_left_alt_sep = '❯'
let g:airline_right_sep = '◀'
let g:airline_right_alt_sep = '❮'
let g:airline_symbols.linenr = '¶'
let g:airline_symbols.branch = '⎇'
" buff
let g:airline#extensions#tabline#enabled = 1
let g:airline#extensions#tabline#tab_nr_type = 1 " tab number
let g:airline#extensions#tabline#show_tab_nr = 1
let g:airline#extensions#tabline#formatter = 'default'
let g:airline#extensions#tabline#buffer_nr_show = 0
let g:airline#extensions#tabline#fnametruncate = 16
let g:airline#extensions#tabline#fnamecollapse = 2
let g:airline#extensions#tabline#buffer_idx_mode = 1
" buff 切换
nmap <leader>1 <Plug>AirlineSelectTab1
nmap <leader>2 <Plug>AirlineSelectTab2
nmap <leader>3 <Plug>AirlineSelectTab3
nmap <leader>4 <Plug>AirlineSelectTab4
nmap <leader>5 <Plug>AirlineSelectTab5
nmap <leader>6 <Plug>AirlineSelectTab6
nmap <leader>7 <Plug>AirlineSelectTab7
nmap <leader>8 <Plug>AirlineSelectTab8
nmap <leader>9 <Plug>AirlineSelectTab9
nmap <leader>0 <Plug>AirlineSelectTab0

" go插件配置 ==============go========================
" 保存文件时自动格式化
let g:go_fmt_command = "goimports"
autocmd BufWritePre *.go :silent! execute 'GoFmt'
" 语法检测
" 检测语法错误，并在 Vim 编辑器中突出显示
let g:go_highlight_types = 1
let g:go_highlight_fields = 1
let g:go_highlight_functions = 1
let g:go_highlight_function_calls = 1
let g:go_highlight_extra_types = 1
let g:go_highlight_operators = 1
let g:go_highlight_build_constraints = 1
" 配置 Go 语言标识符自动补全
" 安装 gocode 并将其添加到 $PATH 环境变量中
" 然后设置以下选项
let g:go_complete_unimported = 1
let g:go_auto_sameids = 1
let g:go_auto_type_info = 1
let g:go_auto_type_sameids = 1
" 自动补全
autocmd FileType go imap <C-x> <C-x><C-o>
" 测试
"autocmd FileType go nmap <leader>gt :GoTest<CR>
" 测试函数
"autocmd FileType go nmap <leader>gf :GoTestFunc<CR>
" 开始 debug
autocmd FileType go nmap <leader>gd :GoDebugStart<CR>
" 打断点
autocmd FileType go nmap <leader>gb :GoDebugBreakpoint<CR>
" 继续
autocmd FileType go nmap <leader>gc :GoDebugContinue<CR>
" 停止
autocmd FileType go nmap <leader>gs :GoDebugStop<CR>
" 跳转到下一个断点
autocmd FileType go nmap <leader>gn :GoDebugNext<CR>
" " break point 高亮
let g:go_highlight_debug = 1

" GoCallers 查看函数被使用情况



" ale插件配置 ==============ale========================
" 配置错误提示窗口
" 错误提示窗口在右侧显示，而不是默认的下方显示
let g:ale_sign_error = '>>'
let g:ale_sign_warning = '>'
let g:ale_error_sign = '✖'
let g:ale_warning_sign = '⚠'
```