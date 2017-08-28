---
layout: post
title:  "git branch 관련 명령어"
categories: tools
---
<h2>Branch</h2>

<p><strong>브랜치, checkout 명령어들</strong></p>

<p>```
git banch exp_name # exp(experimental), feature etc.. 이름은 붙이기 나름</p>

<p>git branch</p>

<p>git branch branch_name</p>

<p>git branch -b branch_name</p>

<p>git branch -D branch_name # 강제 삭제 (병합되지 않은 브랜치도)</p>

<p>git checkout branch_name # 브랜치 전환</p>

<p>git checkout -b branch_name # 브랜치 생성 후 전환
```</p>

<p><strong>브랜치 로그 명령어</strong></p>

<p>```
git log --branches --decorate # 각종 브랜치들의 로그 확인</p>

<p>gti log --branches --decorate --graph # 그래프로 확인 가능, </p>

<p>git log --branches --decorate --graph --oneline # 한눈에 볼 수 있게 한줄로 </p>

<p>git log master..exp # master와 exp의 브랜치 로그 비교. 자신에게 맞는 브랜치 이름을 master, exp 대신 적어주면 됨</p>

<p>git diff master..exp # master, exp의 브랜치 코드 비교
```</p>

<p><strong>브랜치 병합(merge)</strong></p>

<p>우선 A에다가 B의 내용을 병합하고 싶다면 (master에 exp의 내용을 합치고 싶다면)
1. A 브랜치에 checkout 한 후에 (브랜치 전환)</p>

<ol>
<li><code>git merge B</code> 명령어를 입력하면 된다.</li>
</ol>

<p>다음은 master에 exp를 병합하는 (exp -> master) 코드이다.
<code>
git checkout master
git merge exp
</code></p>

<p><strong>conflict (충돌)</strong></p>

<blockquote>
  <p>만약 브랜치끼리 같은 내용을 수정하면 충돌이 발생한다.</p>
</blockquote>

<p><code>&lt;&lt;&lt;&lt;&lt;&lt;&lt; HEAD</code> 부터 <code>=======</code> 사이의 구간이 현재 체크 아웃된 파일의 내용이고
<code>=======</code> 부터 <code>&gt;&gt;&gt;&gt;&gt;&gt;&gt; exp</code> 사이의 구간이 병합하려는 대상인 exp 브랜치의 코드 내용입니다. <br />
이 정보를 참고로해서 두개의 코드를 병합한 후에 특수기호들을 제거해주시면 됩니다. 
작업이 끝나면 파일을 저장.
충돌 작업을 끝냈다는 것을 깃에게 알려줍니다. </p>

<p><strong>stash</strong></p>

<blockquote>
  <p>브랜치의 내용이 안 끝났는데 다른 브랜치로 checkout 해서 작업해야 하는 상황에서
commit을 안하면 checkout이 안되는데 이 때 작업 내용을 숨겨놓는 것</p>
</blockquote>

<p>staging 된 상태에서 다른 브랜치로 checkout을 하면 그 브랜치에도 스테이징 된 상태가 적용되는 것을 볼 수 있다. 그러면 새로운 작업을 하기 힘들다. 이런 상황을 해결하기 위해 stash가 존재한다고 한다.</p>

<p>(<code>git stash --help</code>로 다양한 명령어를 볼 수 있다.)</p>

<p>우선 stash의 기본 개념은 현재 staging 된 작업 (<code>git add</code>로 추가된 작업)을 어딘가에 숨기는 것이다. <code>git stash</code> 명령어로 숨길 수 있다.
<code>
git stash # 지금 하고 있는 작업 (add 명령어로 staged된 작업)을 스태시해줌
</code></p>

<p>그러면 <code>git status</code> 상태에 아무 것도 없이 바뀌는 것을 볼 수 있다. 그 이후에 다른 branch로 전환해서 작업을 할 수 있다.
그리고 다시 stash 한 브랜치로 돌아와서 작업을 재개하려면
<code>
git stash apply
</code>
라는 명령어로 작업을 재개할 수 있다. 이게 기본이다: stash -> apply</p>

<p>stash가 여러 개 있으면 
<code>
git stash list
</code>
로 현재 있는 stash들을 확인할 수 있다.
스태시는 순차적으로 아래에서 위로 스태시가 쌓이고, apply 할 때도 최신 순으로 불러온다 (tap을 눌러서 선택적으로 부를 수 있다.)</p>

<p>git stash list에서 볼 수 있는 스태시들은 delete 명령어를 사용하기 전까지는 지워지지 않는다. 그래서 apply하고 지우는 작업을 반복해주어야 하는데 이걸 방지해 주는 명령어가 pop이다. </p>

<p><code>
git stash pop # 최신 스태시를 불러오고 스태시 리스트에서 삭제
</code></p>

