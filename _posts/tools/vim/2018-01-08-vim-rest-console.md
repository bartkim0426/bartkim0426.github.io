---
layout: post
title:  "vim에서 rest url에 request 요청 보내기: 'vim-rest-console'"
categories: vim
---


django rest framework를 사용하면서 `curl`, `httpie` 등을 사용해서 request 요청을 보낼 일이 많아졌따.


하지만 자주 사용하는 request 요청을 보내려면 매번 명령어를 입력해야하는 불편함이 있었다. 파이썬 테스트를 이용해서 이를 검증하는것도 방법이지만 따로 값을 확인하기가 애매해서 찾아보던 중 vim에서 rest url에 request를 보낼 수 있게 해주는 `vim-rest-console`을 발견하여 소개한다.


자세한 설명은 [vim-rest-console github](https://github.com/diepm/vim-rest-console)에서 확인 가능하다.

## 설치
우선 `vim-rest-console`을 사용하려면 `curl`이 설치되어 있어야 한다. `curl`은 `apt-get install curl`이나(ubuntu 등) `brew install curl`(mac)으로 쉽게 설치 가능하다.


`VRC`(vim-rest-console)은 `vundle`이나 `pathogen`을 사용하여 설치할 수 있다. 나는 `vundle`을 사용하여 설치하였다.

다음 내용을 `~/.vimrc`에 추가해준다.
```
Plugin 'diepm/vim-rest-console'
```

이후 vundle을 사용해서 플러그인으 설치한다.

```
# 다음 명령어를 vimrc 창 안에서 실행시켜준다.
# vimrc를 source
:so %
:PluginInstall
```

## 사용법

### 1.  새로운 `vim` 파일에 확인하고자 하는 url을 적어준다.  

다음은 vim 파일 예시
```
http://localhost:8000
GET /apis/jobs
```

### 2. 해당 vim파일의 buffer의 파일타입을 `rest`로 변경
위의 vim 창 내에서 다음 명령어를 입력
```
:set ft=rest
# :set filetype=rest 도 동일하다.
```

### 3. trigger key로 request요청 (`<C-j>`)
default trigger key는 `Control-j`로 되어 있다. (본인이 원하는 키로 매핑이 가능하다)  

새 vim buffer가 열리면서 결과값이 나오는 것을 볼 수 있다.

### 4. `.rest` 형태로 저장

매번 새로운 탭을 열고 url을 적는 것이 번거롭기 때문에 `.rest` 확장자로 저장해두고 이를 사용 가능하다.

`api.rest` 파일을 만들고 다음 내용을 입력해준다.
```
http://localhost:8000
GET /apis/jobs
```

이제 별다른 설정 없이 핫키(`<C-j>`)를 누르면 작동하는 것을 볼 수 있다.



## 설정
