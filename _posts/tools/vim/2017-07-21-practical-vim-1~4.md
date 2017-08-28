---
layout: post
title:  "practical vim 1~4장"
categories: vim
---
## Practical vim 다양한 명령어들

### 준비운동

### 유용한 명령어들

- vim factory setting
$ vim -u NONE -N

## Practical vim 다양한 명령어들

### 준비운동
- vim factory setting
`$ vim -u NONE -N`
- vimrce 대신 특정 파일을 사용하여 vim 구동
`$vim -u code/essential.vim`
- vim 기능 확인 명령어
`:h +feature-list`
`:version`

### 01. vim의 방식

**점 명령: 마지막 변경을 반복한다**
`var foo = "method("+argument1+","+argument2+")"`에서 
`var foo = "method(" + argument1 + "," + argument2 + ")"`로 바꾸기
1. f+: + 찾아가기, `f{string}`- search string workd
2. s + <Esc>: s로 기존에 있던 +를 지우고 space + space를 입력
3. ;를 통해 다음 +를 찾고 .으로 반복 작업 실행

#### Tip4. 실행하기, 반복하기, 되돌리기: 대부분은 u (실행 취소)로 명령을 되돌릴 수 있고 그 외 각 기능별로 사용 가능
- 행에서 다음(이전)문자 찾기: ; - ,
- 문서에서 다음(이전)일치 찾기: n - N

#### Tip5. 직접 찾고 치환
- 치환 동작하기:
```
iting for content before the site can go live...
...If you are content with this, let's go ahead with it...
...We'll launch as soon as we have the content...
```
다음 문단에서 content -> copy로 바꾸는 방법
`:%s/content/copy/g` > 여기서 g가 무슨 뜻인지 잘 모르겠다.
각각의 content를 확인하면서 copy로 바꾸는 방법
`*`: content에 커서를 놓고 `*`를 입력하면 해당 단어가 모두 선택
`cwcopy<Esc>`: cw로 해당 단어를 지우고 insert 모드로 변경 후 copy를 적어줌`n`: 원하는 단어를 선택
`.`: 치환을 원하는 단어를 변경


### 02. 일반 모드

#### Tip8. 덩어리로 실행 취소하기
- insert mode에서 다음 줄로 갈때 <CR>보다 <ESC>o 를 사용하는 게 더 좋음: 이 작업을 변경점으로 사용하기 때문 -> 다눙에 실행 취소를 하는 상황도 미리 고려 가능하다.
- "지금 끼워넣기 모드를 벗어나야 하나?"라는 질문에 약간이라도 머뭇거린다면 그 순간에 벗어나자

#### Tip9. 반복 가능한 변경 조합하기

vim은 반복에 최적화: 어떤 방식으로 수정, 조합하는지를 염두에 두고 해야함 -> 여러 기법 중에 키를 가장 적게 입력하는지 따져볼 수 있다: 이를 [vim golf](http://vimgolf.com/)라고 부름

`The end is nigh`
커서가 단어 끝 (nigh)를 지울 때 가능 한 조합
1. `db`(이전 단어 역방향 지우기) + `x`(남은 단어 지우기)
2. `b`(이전 단어로 돌아가기) + `dw`(정방향 지우기)
3. `daw` (단어전체 삭제하기)
> 세 방법 모두 세 자를 입력하기 때문에 3점. 그러면 뭐가 가장 좋은 방법일까? 
vim은 반복에 최적화되어 있음 -> 세 기법에서 . 명령으로 동일한 명령을 실행시 완전 다른 결과
즉 마지막 명령인 `daw`를 사용하면 점 명령을 그대로 사용 가능 

=> 점 명령을 효율적으로 사용하려면 이후 어떤 작업을 할 지 미리 예상
반복 할 예정이라면 점 명령을 사용 가능하도록 적절한 명령을 잘 조합해서 사용하는 것이 좋다. 
=> 가능한 일상적인 모든 작업에서 명령을 반복할 수 있는 방식으로 명령을 조합해서 사용하는 습관을 기르자

#### Tip10. 간단한 산술에 실행 횟수 사용하기
`<C-a>`: 커서 밑에 있는 숫자 증가
`<C-x>`: 커서 밑에 있는 숫자 감소
ex) 10<C-a> : 10 증가
> 현재 tmux 세팅이 C-a 로 되어있기 때문에, `<C-a>a`를 입력하면 된다.
```
.blog, .news { background-image: url(/sprite.png); }
.blog { background-position: 0px 0px }
```
이전 행을 복제하고 내용을 변경하는 예제 (커서가 두 번째 줄 처음에 있다고 가정)
1. `yyp`: yy (한줄복사) + p(붙여넣기) - `yy5p`로 여러 줄 붙여넣기 가능
2. `cW.news<Esc>`: cW (change next word) + .news insert
3. `180<C-x>`: 다음에 나오는 숫자(?)를 입력한 후 감소

#### Tip.11 직접 반복할 수 있다면 실행 횟수 사용하지 않기
`Delete more than one word`
여기서 more than을 지우려고 할 때, `d2w`, `2dw` 모두 사용 가능하다. 하지만 뜻이 다름. 또한 `dw.`도 사용 가능하다. 뭐가 최선일까?
> d2w, 2dw는 u를 누른 경우 두 단어가 나타남.
> dw.는 uu를 눌러야 두 글자가 생김
만약 상황이 세 단어를 제거하는 것이라면 `dw.`을 사용했을 경우에 훨씬 좋다. 
단어를 7개 제거하려고 했을 때에도 마찬가지: d7w를 사용할 수도 있고 `dw......`을 사용할 수도 있다. => 책에서는 dw......을 추천함! "실행하고, 반복하고, 되돌려라": 이 말을 실천하자
**횟수가 중요할 때에는 실행 횟수를 사용하자**
`I have a couple of questions`을 `I have some more questions`라고 바꾸려고 한다면 다음과 같이 처리 가능
`c3wsome more<ESC>`: c3w(change 3 words)
> 이 시나리오는 점 명령을 이용하기에는 적합하지 않기 때문에 한 단어 수를 세서 작업함.

#### Tip.12 분할 정복
vim은 operator, motion 기능을 조합해서 사용할 수 있다는 점에서 강력한 힘.

**"Operator + motion = 행동"**
`d{motion}`은 본문을 제거할 때 사용 가능한 옵션. motion 범위에 따라
- `dl`(one letter- letter)
- `daw`(one word- a word)
- `dap`(one paragraph)
이와 같이 명령의 범위는 모션을 통해 정의.
c{motion}, y{motion}에도 유용하게 적용 가능: 이런 명령을 **operator**라고 부른다. (operator의 모든항목은 `:h operator`에서 확인 가능)
다음은 많이 쓰이는 오퍼레이터 명령어들

|trigger|effect|
|c|change|
|d|delete|
|y|yank to register|
|g~|Cap(대소문자변환)|
|gu|소문자로 변환|
|gU|대문자로 변환|
|>|tab to right|
|<|tab to left|
|=|indent auto|
|!|[모션]에 해당하는 행을 외부 프로그램을 사용해 여과(?)|

`daw`로 단ㅇ너 삭제, `gU`는 대문자. -> gUaw를 실행하면 현재 단어를 모두 대문자로 전환 가능 (gU + aw: a word?)
`ap` motion(문단 단위로 적용-a paragraph)을 배운다면 이 모션을 d, gU에 조합하는 것으로 새로운 명령 사용 가능.
- `dap`: 문단 삭제
- `gUap`: 문단 전체를 대문자로

Vim의 또다른 규칙: 오퍼레이터 명령을 반복 입력하면 현재 행을 대상으로 동작: dd로 현재 행 제고, >>로 들여쓰기가 가능. gU는 두번의 입력이기 때문에 현재 행을 대상으로 실행하려면 `gUgU` 혹은 `gUU` (약식)을 사용하면 된다.

**조합의 힘으로 vim 확장하기**
vim의 기본 오퍼레이터/모션을 조합하면 매우 큰 경우의수 + 커스텀 모션, 오퍼레이터를 이용하면 더 크게 확장 가능하다.

**기존의 모션과 커스텀 오퍼레이터 사용하기**
표준 오퍼레이터로 부족하면 새로운 정의를 만드는 것도 가능
ex)[commentary vim plugin](https://github.com/tpope/vim-commentary)
commentary.vim 사용법 및 설치법: vundle에서 검색해서 설치 가능
이 플러그인으로는 `gc{motion}`으로 주석 사용 가능, 일반적인 모션과 함께 조합해서 사용 가능하다.
이렇게 custom operator를 만들기 위해선 `:h :map-operator`를 참고

**기존의 오퍼레이터와 커스텀 모션 사용**
빔에서는 새로운 모션, 텍스트 객체를 정의해서 확장 가능.
ex)[textobj-entire](https://github.com/kana/vim-textobj-entire)
사용법: ie, ae (전체 파일에 해당하는 텍스트 객체)가 추가됨. 
자신만의 모션을 만드는 방법은 `:h omap-info`를 확인


오퍼레이터대기 모드(Operator-Pending mode):`dw명령오퍼레이터 명령을 입력하면 d,w 사이에 오퍼레이터 대기 모드, 사용자가 직접 오퍼레이터, 모션을 제작해서 vim의 어휘를 늘릴 수 있도록



### 03. 끼워넣기 모드

#### Tip.13 끼워넣기 모드에서 바로 교정하기
insert mode에서 backspace는 커서 앞 문자 하나를 지움. 다음과 같음.
`<C-h>`: 앞에있는 글자 제거. (백스페이스와 동일)
`<C-w>`: 앞에 단어 하나 제거
`<C-u>`: 행의 시작부분까지 제거
=> 이 명령은 vim 명령행 뿐 아니라 bash에서도 사용 가능 (u로 다시 복구는 불가능한거같다.)

#### Tip.14 일반 모드로 돌아가기
vim의 normal, insert 모드를 빠르게 변환하는 것이 중요. 다음의 모든 방법을 사용해서 가능하다.
1. `<ESC>`: normal mode
2. `<C-[>`: 일반모드로 진입
3. `<C-o>`: 끼워넣기-일반(insert-normal) mode로 진입

**insert normal mode**
일반 모드의 특별판. 일반 모드의 명령 하나를 실행 할 수 있도록 일반 모드로 잠시 전환 후 명령 실행 직후에 끼워넣기 모드로 돌아옴. insert mode에서 `<C-o>`를 입력하면 진입 가능 (`:h i_CTRL-O` 참고)
ex) `zz`명령어: 현재 화면을 vim 화면의 중간에 오게 함. 이를 위해서 normal mode로 바로 진입 하지 않고 `<C-o>zz`를 입력하면 입력 중에 zz를 사용 가능하다. 

#### Tip.15 끼워넣기 모드를 벗어나지 않고 레지스터 붙여넣기
```
Practical Vim, by Drew Neil
Read Drew Neil's
```
다음 문장의 practical vim이라는 단어를 복사해서 문장 마지막에 붙이기
1. `yt,`: t{문자}(문자로 찾기)
2. `jA `: go to next line
3. `<C-r>0`: <C-r>{register num} (insert mode에서 0번으로 붙이기)

**<C-r>{register}를 문자 단위 레지스터에 사용**
만약 레지스터가 많은 분량의 내용을 포함하면 지연 발생: vim이 레지스터의 내용을 한글자씩 가져와서 추가
`<C-r><C-p>{register}`: 레지스터의 내용을 변경하지 않고 저장된 그대로 본문에 복사 (`:h i_CTRL-R_CTRL-P`참고)

#### Tip.16 즉석에서 계산하기
표현식 레지스터- 지금 위치에서 계산을 수행 후 바로 문서에 입력
```
Typing in Insert mode extends the line. But in Replace mode
the line length doesn't change.
```
다음에서 . 을 쉼표로, B를 b로 바꿈; `R`로 바꾸기 모드로 들어갈 수 있음.
1. `f.`: .을 find
2. `R, b<Esc>`: 변경
**선택치환 모드로 탭 문자 덮어쓰기**
탭문자 등은 1개의 문자/ tabstop에 지정된 만큼 열 차지 (`:h tabstop`참조)
`gR` 명령을 통해 탭 문자를 일반 문자로 처리 (공백당 1문장으로)
즉, 선택치환 모드에서는 실제 파일에 존재하는 문자를 바로 대체 하지 않고 화면에 표시되는 기준에 따라 => 될 수 있으면 선택치환을 사용해라
```
		test
```


## 04장 비주얼 모드 (visual mode)

#### Tip.20 비주얼 모드의 내부 들여다보기
비주얼 모드의 명령은 일반 모드와 거의 동일. 
> select mode (`:h Select-mode`)
> `<C-g>`를 눌러 visual mode와 전환 가능. 비주얼 모드에서 c를 눌러 선택 영역을 바꾸는것과 동일
> 바로 문자를 수정 가능하다. 그래도 최대한 사용 안하는게 좋음

#### Tip.21 비주얼 영역 선택 정의하기
visual mode에는 세가지 서브모드
1. character-wise: 개별 단어/구문 단위
3. line-wise: 행 단위
4. block-wisd: 블록 단위로 사용

비주얼 모드 활성화**
1. `v`: character-wise: 개별 단어/구문 단위
2. `V`: line-wise: 행 단위
3. `<C-v>`: block-wisd: 블록 단위로 사용
4. `gv`: 마지막 선택 영역 선택하기

**비주얼 모드간 전환**
`v/ V / <C-v>` 각각 모드로 전환 / 각 모드에서 일반모드로 전환
`o`: 선택 영역 중 반대쪽 끝으로 이동

**선택영역의 끝 전환**
시작 위치를 잘못 정했을 떄 `o`를 눌러 영역 재지정 가능.

#### Tip.22 행 범위 비주얼 모드 반복하기
v-mode에서도 . 명령을 이용해서 선택 영역 변경 가능
```
def fib(n):
    a, b = 0, 1
    while a < n:
print a,
a, b = b, a+b
fib(42)
```
다음 코드에서 indent 맞추기. 가장 빔스러운 방법은?
1. `Vj`로 두 줄 선택
2. `>.`로 두차례 실행
2>를 사용하는 것보다 눈에 직접 보이고 u를 사용 가능하기 때문에 점 명령을 사용하는 것이 좋다!

#### Tip.23 가능하면 v-mode보다는 operator 명령 사용
v-mode는 약점도 존재: .명령에서 다른 방식으로 동작 가능.
다음 링크 목록을 대문자로 바꿔보자
```
<a href="#">one</a>
<a href="#">two</a>
<a href="#">three</a>
```
- visual-mode로 사용해서 바꾸기
1. `vit`: visuallyselect inside the tag (태그 안 선택! wow, 이런명령어가 있다니... ㄷㄷ)
2. `U`: 대문자 전환
3. `j.`을 반복하여 적용하면 3번째 three -> THRee가 됨. 3블록만 되기 때문
- operator command 사용
1. `gUit`: gU명령(소문자>대문자)을 `gu{motions}`형식으로 사용. it는 'in tag'의 의미
2. `j.`을 반복: 3번째도 THREE로 잘 바뀜
> v-mode에서 점 반복 작업을 하면 고려할 사항이 많아서 오퍼레이터를 사용하는것이 좋다. 

#### Tip.24 탭으로 된 데이터를 비주얼 블록 모드로 편집하기.
문서를 열 단위로 조작하기 위해서 v-block mode 사용.
```
Chapter            Page
Normal mode          15
Insert mode          31
Visual mode          44
```
위 내용을 표 형식으로 바꾸려고 한다. (결과물은 다음)
```
Chapter      |  page
--------------------
normal mode  |    15
insert mode  |    31
visual mode  |    44
```
1. (커서를 공백에 놓고)`<C-v>3j`: 블록지정
2. `x...`: 원하는 만큼 줄 줄이기
3. `gv`: 이전 블록
4. `r|`: 블록을 pipeline으로 변경
5. `yyp`: 첫 줄 복사
6. `Vr-`: 줄 블록설정하여 ---------로 모두 변경

#### Tip.25 문서 열 변경
```
li.one   a{ background-image: url('/images/sprite.png'); }
li.two   a{ background-image: url('/images/sprite.png'); }
li.three a{ background-image: url('/images/sprite.png'); }
```
이 css에서 모든 /images를 /components로 변경하려고 한다.
1. `f/k`: images의 i로 커서 이동 (내 방법이므로 더 효율적인 방법이 있을듯)
2. `<C-v>jje`: 모든 images 선택 
3. `c`: 끼워넣기 모드
4. `components<Esc>`: 첫 줄만 바뀌다가 Esc를 입력하면 모두 변경됨

#### Tip.26 비주얼 블록을 쪼개서 본문 넣기
```
var foo = 1
var bar = 'a'
var foobar = foo + bar
```
위 js 코드에서 모든 줄의 뒤에 ;를 넣으려고 한다. 점 명령 말고 비쥬얼 블록으로도 사용 가능하다. 
1. `f1`: 1로 커서를 옯김
2. `<C-v>jj$`: 세 줄 모두 끝까지 블록 지정. $, ^등을 사용 가능하다.
3. `A;<Esc>`: 끼워넣기 모드로 변경 후 세미콜론 지정 후 일반모드
> I를 입력하여 맨 첫줄로도 동작. 하지만 i, a 명령은 다른 기능으로 동작.

