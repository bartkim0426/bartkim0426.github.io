---
layout: post
title:  "VIM: change vim commit message color"
categories: vim
---
 
You can change by modify `gitcommit.vim`

You can find your `gitcommit.vim` in `syntax/` dir. 

> you can easily find vim dir by `:echo $VIMRUNTIME` inside your vim.


Inside `gitcommit.vim`, you have to find gruop you want to change.

In my setting, it's `gitcommitSelectedType`. Here's gitcommitSelectedType match - it use `\t\@<=[[:lower:]][^:]*[[:lower:]]: ` for matching `    modified: ` and `    new file: ` in commit template.


```
syn match   gitcommitSelectedType	"\t\@<=[[:lower:]][^:]*[[:lower:]]: "he=e-2	contained containedin=gitcommitComment nextgroup=gitcommitSelectedFile skipwhite
```


For simple example, I clear `gitcoomitSelectedType` and add new match for `gitcommitNew` and `gitcommitModified`. (simple match for example. You can use regex for your own)


```git
syn clear gitcommitSelectedType
" match for new file and modified
syn match gitcommitNew	"\t\@<=new file: " contained containedin=gitcommitComment nextgroup=gitcommitSelectedType skipwhite
syn match   gitcommitModified	"\t\@<=modified: "he=e-2	contained containedin=gitcommitComment nextgroup=gitcommitModified skipwhite

" give other color type for these group
hi link gitcommitNew	Type
hi link gitcommitModified	Special
" add two groups we made to gitcommitSelected
syn region  gitcommitSelected	start=/^# Changes to be committed:/ end=/^#$\|^#\@!/ contains=gitcommitHeader,gitcommitHead,gitcommitSelectedType,gitcommitNew,gitcommitModified fold
```

Maybe there's better solution - not using group but using exception after gitcommitSelectedType - but I don't know how. Maybe you can find it.

### Before

[![Before change color][1]][1]

### After
[![enter image description here][2]][2]


## ADD

Also, it will added for future. Check [here](https://github.com/tpope/vim-git/pull/58)


  [1]: https://i.stack.imgur.com/UdP1u.png
  [2]: https://i.stack.imgur.com/jpfHj.png<Paste>
