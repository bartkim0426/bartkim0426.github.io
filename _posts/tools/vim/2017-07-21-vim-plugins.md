---
layout: post
title:  "vim plugins"
categories: vim
---

## 추가한 라이브러리들

### vim-devicon
빔 nerd-tree 등에서 파일 모양 만들어주는 plugin.
nerd fonts 등 필요한 의존 라이브러리가 많은듯.. 속도 저하는 모르겠고 얼른 nerd font를 완전히 설치해서 써봐야겠다. 현재 nerd fonts가 제대로 불러와지지 않는 문제

### vim-startify
vim을 처음 불러주면 버퍼? 열었던 파일? 등을 쭉 나열...해주는 좋은 기능!
꽤 좋은 라이브러리같다. 설치/사용법은 매우 간단한거같고 필요하면 
```
:h startify
:h startify-faq
```

### [jedi-vim](https://github.com/davidhalter/jedi-vim)
vim 자동완성. pymode, syntastic 등이랑 고민하다가 jedi-vim으로 선택.
설치 방법
`pip install jedi`로 jedi를 설치,
vundle을 활용해 jedi-vim 설치

사용방법
```
<C-space> # 자동완성 키 => 나는 alfrad랑 겹쳐서;; <C-n>으로 바꿈 (+추후 supertab 설치 후 tab 사용)
<leader>g # Goto assignments
<leader>d # goto Definition
K # show documentation
<leader>r # rename
<leader>n # show all usage Name
:Pyimport os # open module? 해당 모듈을 찾아서 띄우줌... 굿
```
설정들
```
let g:jedi#completions_enabled = 0 # 자동완성 끄기
NOTE: subject to change!
let g:jedi#goto_command = "<leader>d"
let g:jedi#goto_assignments_command = "<leader>g"
let g:jedi#goto_definitions_command = ""
let g:jedi#documentation_command = "K"
let g:jedi#usages_command = "<leader>n"
let g:jedi#completions_command = "<C-Space>"
let g:jedi#rename_command = "<leader>r"
```

### supertap
jedi-vim을 tab으로 사용하기 위해서 설치. Vundle로 설치 후 PluginInstall

### CtrlP 설정
`<C p>`후 검색

### Buffergator
```
<leader>jj: 이전 버퍼
<leader>kk: next buffor
<leader>bl: see evary buffor
<leader>T: enew
<leader>bq: close buffor
```

### pymode
```
zf#j creates a fold from the cursor down # lines.
zf/string creates a fold from the cursor to string .
zj moves the cursor to the next fold.
zk moves the cursor to the previous fold.
zo opens a fold at the cursor.
zO opens all folds at the cursor.
zc close a fold at the cursor.
zm increases the foldlevel by one.
zM closes all open folds.
zr decreases the foldlevel by one.`
zR decreases the foldlevel to zero -- all folds will be open.
zd deletes the fold at the cursor.
zE deletes all folds.
[z move to start of open fold.
]z move to end of open fold.
```

### easymotion
```
<Leader>f{char}: 현재 커서의 오른쪽으로 {char}를 찾는다.
<Leader>F{char}: 현재 커서의 왼쪽으로 {char}를 찾는다.
<Leader>t{char}: 현재 커서의 오른쪽으로 {char} 바로 앞
<Leader>T{char}: 현재 커서의 왼쪽으로 {char}바로 앞
<Leader>w: 현재 커서의 오른쪽으로 단어의 시작(Beginning of word forward)
<Leader>W: 현재 커서의 오른쪽으로 단어의 시작(Beginning of WORD forward). ‘w’랑 살짝 다르다.
<Leader>b: 현재 커서의 왼쪽으로 단어의 시작(Beginning of word backward)
<Leader>B: 현재 커서의 왼쪽으로 단어의 시작(Beginning of WORD backward)
<Leader>e: 현재 커서의 오른쪽으로 단어의 끝(End of word forward)
<Leader>E: 현재 커서의 오른쪽으로 단어의 끝(End of WORD forward)
<Leader>ge: 현재 커서의 왼쪽으로 단어의 끝(End of word backward)
<Leader>gE: 현재 커서의 왼쪽으로 단어의 끝(End of WORD backward)
<Leader>j : 아래로 각 줄의 첫번째
<Leader>k: 위로 각 줄의 첫번째
<Leader>s{char}: 현재 화면에서 {char}를 찾는다. f, F를 합쳤다고 볼 수 있다.
```

### [git commentary](https://github.com/tpope/vim-commentary)

사용방법`gc{motion}`

### vim emmet  
`<C y>,`


### [ fugitive.vim ](https://github.com/tpope/vim-fugitive)
- vim에서 git 명령어를 사용 가능하다! wow...
- 사용가능한 명령어들
`:Gedit, Gsplit, Gvsplit, Gtabedit`
`:Gdiff`
`:Gstatus`: 이후 `-`를 눌러 add/reset 가능, p를 눌러 add`reset --patch 가능
`:Gcommit`
`:Gblame`
`:Gmove, Gdelete`
`:Ggrep`: work tree에서 검색가능
`:Glog, Gread, Gwrite,, Gbrowse`
[공식 깃헙](https://github.com/tpope/vim-fugitive)
[vimrc 설정해놓은 stackoverflow](https://stackoverflow.com/questions/15369499/how-can-i-view-git-diff-for-any-commit-using-vim-fugitive)
+ [vim-rhubarb](https://stackoverflow.com/questions/15369499/how-can-i-view-git-diff-for-any-commit-using-vim-fugitive)
> :Gbrowse로 현재 github를 띄울 수 있따.
``` (도움말들)
:help cmdline-special
:help :_%
ctlr-n/ctrl-p keyword autocompletion
:help 'complete'
:help :Git
:help :Gwrite
:help :Gread
:help :Gremove
:help :Gmove
:help :Gcommit
:help :Gblame
```


### [vim ale](https://github.com/w0rp/ale)
- [여기](http://liuchengxu.org/posts/use-vim-as-a-python-ide/)서 추천한 syntax check plugin.
- 써보니 괜찮은것같아서 syntastic을 대체할예정
- => 현재 안됨... 특정 venv 들어가면 안되는덧같은데..왜?
- ==> flake8이 세팅이 안되어있어서... `pip install flake8`로 매번 다 세팅을 해줘야할듯하다 (모든 프로젝트에서)

- 다음은 다양한 세팅들

```
# 특정 linter 끄기
let g:ale_linters = {
\   'javascript': ['eslint'],
\}
# sign gutter always open? 왼쪽에 조금씩 열려있음
let g:ale_sign_column_always = 1
# 기호 바꾸기 및 하이라이팅
let g:ale_sign_error = '>>'
highlight clear ALEErrorSign
highlight clear ALEWarningSign
# vim airline과의 integration =>개굿..추천
function! LinterStatus() abort
    let l:counts = ale#statusline#Count(bufnr(''))

    let l:all_errors = l:counts.error + l:counts.style_error
    let l:all_non_errors = l:counts.total - l:all_errors

    return l:counts.total == 0 ? 'OK' : printf(
    \   '%dW %dE',
    \   all_non_errors,
    \   all_errors
    \)
endfunction

set statusline=%{LinterStatus()}
# 에러메세지 변경하기?
let g:ale_echo_msg_error_str = 'E'
let g:ale_echo_msg_warning_str = 'W'
let g:ale_echo_msg_format = '[%linter%] %s [%severity%]'

#에러사이에 쉽게 이동하기: 이렇게 하면 C-k, C-j 쓰는게 없어야될듯
nmap <silent> <C-k> <Plug>(ale_previous_wrap)
nmap <silent> <C-j> <Plug>(ale_next_wrap)

# quickfix? 바로바로 확인이 가능
let g:ale_set_loclist = 0`
let g:ale_set_quickfix = 1
```

### [yapf](https://github.com/google/yapf#usage)
- 파이썬 코드 자동수정 (autofomatter)
- 설치는 pip로 `pip install yapf`
- 사용법은 yapf를 command에서 입력함 된다. 
- vim에서 사용하려면 
`:0,$!yapf`: 명령행을 부른다음에 전체 범위에서 외부 명령 yapf 실행
이를 좀 더 쉽게 쓰기 위해서 ~/.vimrc 파일에
`autocmd FileType python nnoremap <Leader>= :0,$!yapf<CR>` 를 추가해줬다.
나는 leader key가 `,`로 되어있기 때문에 `,=`로 쉽게(?) 사용이 가능
다양한 설정은 github에 설명이 나와있..는데 잘 모르겠다 아직


## scheme 추가
- janha? 아톰이랑 비슷한 colorscheme임... 굿


