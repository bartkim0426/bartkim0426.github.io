---
layout: post
title:  "My neovim settings: 나의 neovim 세팅을 공유합니다."
categories: vim
---

내가 `vim`을 선호하는 가장 큰 이유 중 하나는 `속도`이다. (간지라고는 말 못하니깐...)

그런데 최근 vim에 무분별하게 설치한 플러그인(...)때문에 속도가 조금 저하되는 문제가 발생하여 이것 저것 플러그인을 지우고 최적화를 진행하다가, `neovim`이 vim 자체의 가치에서 크게 벗어나지 않고 성능을 크게 향상시켰다는 말을 듣고 neovim을 사용하기 시작했다. `macvim` 등 프로그램 기반의 다른 vim은 거부감이 많이 들었는데, `nvim`은 내가 사용하던 `vim8`과 많이 다르지 않기 때문에 선택했는데 현재까지는 매우 만족하며 사용중이다.


처음에는 `~/.vimrc` 세팅을 그대로 가져와서 사용하였으나 (귀차니즘) 하루 마음을 먹고 `Plug`를 사용한 플러그인 설치나, 최적화 등을 목적으로 `init.vim`을 작성해 보았다. 작성하면서의 원칙은

- 생산성에 충실: vim 자체도 좋지만 타 IDE보다 너무 생산성이 떨어지면 안된다고 생각하였다.
- 속도를 최대한 느리게 하지 않는다. 자주 사용하는 플러그인 위주로만 설치.
- 기본 vim에 충실: 과도하게 키를 매핑하여 사용하지 않는다.
- 가독성: 기존 `vimrc`가 (나조차도) 읽기 힘들게 난잡하게 작성되었기 때문에 이번에는 최대한 정리하여 작성하였다.
- 호환성: 다른 사람들도 사용하기 용이하게 최대한 범용적으로 사용할 수 있게 작성하였다. (적어도 그렇게 하려고 노력하였다.)

나도 그동한 다른 사람들이 작성한 빔 설정을 보고 많은 도움이 되었기 때문에, 내 것도 누군가에게는 도움이 되었으면 좋겠다는 마음에 미약하지만 올려본다.

다음은 내 `nvim` 사진. 테마는 `NeoSolarized`를 사용. 자동완성은 `deoplete`, syntax 검사는 `ale`을 사용하였다.

![my-nvim-capture](https://i.imgur.com/lSSV7W6.png){:class="img-responsive"}


```vim
"Seul neovim setting
"Author: seulchankim, bartkim0426@gmail.com
"I usually use Python. Sometimes use Html, Css, Javascript (vue.js) and
"Markdown.

"================================= General Config ===================================

filetype plugin on

let g:mapleader = ","           "set leader key to comma

set number                      "line numbers
set hlsearch                    "highlight searching result
set ignorecase                  "ignore Case sensitive when searching
set showmatch                   "highlight matched bracket ()

"================================= Optional setting =================================
set relativenumber              "show relative number from current line
                                "Highly recommended
set history=1000                "store :cmd history
set title                       "change the terminal title
set cursorline                  "highlight cursor line
set mouse=h                     "Enable mouse in help mode
                                "'a' to all mode, n, v, i, c to Normal, Visual, Insert, Command mode
set wrap
let g:onedark_terminal_italics = 1

syntax sync minlines=200        "For speed up vim
set clipboard=unnamed           "yank, paste to system clipboard

"================================= Turn of swap =====================================
set noswapfile
set nobackup
set nowb

"================================= Indentation ======================================
set smartindent
set tabstop=4
set shiftwidth=4
set expandtab
set autoindent

"================================= Tap & Space ======================================
nmap <leader>l <Esc>:set list!<CR>
highlight SpecialKey cterm=None ctermfg=grey
map <leader>2 <Esc>:retab<CR>:set ts=4<CR>:set noexpandtab<CR>:retab!<CR>:set expandtab<CR>:set ts=2<CR>:set sw=2<CR>:retab!<CR>
map <leader>4 <Esc>:retab<CR>:set ts=2<CR>:set noexpandtab<CR>:retab!<CR>:set expandtab<CR>:set ts=4<CR>:set sw=4<CR>:retab!<CR>
set listchars=tab:>·
set listchars+=trail:·
set listchars+=extends:»
set listchars+=precedes:«
set listchars+=nbsp:·
set listchars+=space:·
" set listchars=tab:>·
" set listchars+=trail:·          "show for tab, trail char at the end of the line
" set listchars+=extends:»
" set listchars+=precedes:«
set fillchars+=vert:\│          "Make vertical split separator full line

" ================================ Persistent Undo ===================================
" Keep undo history across sessions, by storing in file.
silent !mkdir ~/.config/nvim/backups > /dev/null 2>&1
set undodir=~/.config/nvim/backups
set undofile

" ================================ Auto command ===================================

autocmd InsertEnter * :set nocul                  "Remove cursorline highlight
autocmd InsertLeave * :set cul                    "Add cursorline highlight in normal mode
autocmd FileType html setlocal sw=2 ts=2 "Set indentation to 4 for html, css, scss, js
autocmd FileType css setlocal sw=2 ts=2
autocmd FileType scss setlocal sw=2 ts=2
autocmd FileType js setlocal sw=2 ts=2

"================================= Plugins ========================================
call plug#begin('~/.vim/plugged')
if has('nvim')
  Plug 'Shougo/deoplete.nvim', { 'do': ':UpdateRemotePlugins' }
else
  Plug 'Shougo/deoplete.nvim'
  Plug 'roxma/nvim-yarp'
  Plug 'roxma/vim-hug-neovim-rpc'
endif
" ----- For deoplete autocomplete -----
Plug 'zchee/deoplete-jedi'
Plug 'zchee/deoplete-zsh'
Plug 'Shougo/neco-syntax'
Plug 'Shougo/neco-vim'
Plug 'carlitux/deoplete-ternjs', { 'do': 'npm install -g tern' } "https://github.com/carlitux/deoplete-ternjs
Plug 'davidhalter/jedi-vim'
" ----- end -----
Plug 'w0rp/ale', { 'do': 'npm install -g prettier' }

Plug 'nightsense/simplifysimplify'
Plug 'wakatime/vim-wakatime'            "check coding time
Plug 'scrooloose/nerdtree'
Plug 'jistr/vim-nerdtree-tabs'          "Toggle nerd tree with one key
Plug 'mattn/emmet-vim'                  "Trigger: <C-y><leader>
Plug 'tpope/vim-commentary'             "Trigger: gc
Plug 'jiangmiao/auto-pairs'             "Auto pair for ',), }, ]...
Plug 'airblade/vim-gitgutter'           "Show git status in vim
Plug 'tpope/vim-fugitive'               "syntax error
Plug 'ctrlpvim/ctrlp.vim'               "Ctrl + P for search file
Plug '~/.fzf'                           "https://github.com/junegunn/fzf.vim
Plug 'junegunn/fzf.vim'
Plug 'mhinz/vim-startify'               "fancy start page for empty vim
Plug 'posva/vim-vue'
Plug 'iCyMind/NeoSolarized'

call plug#end()

" User deoplete
let g:deoplete#enable_at_startup = 1

"----- useful but not installed -----
" vim airline: Many information about file, git and buffer
" Plugin 'vim-airline/vim-airline'
" Plugin 'vim-airline/vim-airline-themes'

" ----- Will install later -----
" Ctag

"================================= Plugins setting ==================================
"----- Nerd Tree -----
map nt <ESC>:NERDTreeToggle<CR>
"map nt <ESC>:NERDTree<CR>
let NERDTreeIgnore=['\.pyc$', '\~$']    "ignore files in NERDTree

"----- deoplete -----
" use tab to forward cycle
inoremap <silent><expr><tab> pumvisible() ? "\<c-n>" : "\<tab>"
" use tab to backward cycle
inoremap <silent><expr><s-tab> pumvisible() ? "\<c-p>" : "\<s-tab>"
" For deoplete-ternjs
let g:deoplete#sources#ternjs#filetypes = [
                \ 'vue',
                \ ]
" ----- jedi vim -----
let g:jedi#show_call_signatures = "0"   "jedi-vim slowdown

" ----- ale -----
let g:ale_lint_on_save = 1              "Lint when saving a file
let g:ale_sign_error = '✖'                                                      "Lint error sign
let g:ale_sign_warning = '⚠'                                                    "Lint warning sign
let g:ale_statusline_format =[' %d E ', ' %d W ', '']                           "Status line texts
let g:ale_linters = {'javascript': ['eslint']}                                  "Lint js with eslint
let g:ale_fixers = {'javascript': ['prettier', 'eslint']}                       "Fix eslint errors
" ALEFix, ALEFixSuggest로 fix 사용 가능

" ----- NeoSolarized -----
colorscheme NeoSolarized
set background=dark
```
