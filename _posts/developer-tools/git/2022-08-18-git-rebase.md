---
title:  "[git] git rebase 정리"
categories: [developer-tools, git]
tags: [git]
toc: true
toc_sticky: true
date: 2022-08-18
---

<br>
<br>
<br>

---
## git rebase 
---

## 잠깐 ❗

> 어딘가에 push로 내보낸 작업은  git rebase 하면안됨 ---> 왜냐?\
> 하는 순간 가지치기처럼 내용이 병합되면서 원하고자 하는 내용과 달라짐.\
> 로컬저장소(내 노트북)에서 push 하기 전 작업은 rebase 해도 무방하다

<br>
<br>
<br>
<br>
<br>
<br>

### 🎲 예시 상황
> 난 작업을 하던 중 `water`, `sun` 브랜치로 커밋이 얽혀있는 것을 발견해 일원화를 하려고 한다.

![Desktop View](/assets/img/git/rebase/1.PNG)

<br>
<br>

`water` 브랜치

<br>
<br>
<br>
<br>
<br>
<br>
<br>


아래 사진을 클릭하면 `제 취미공간`으로 이어집니다 ㅎㅎ 여기에서 메모 및 일상을 기록하는데 놀러오실 분들은 언제나 환영합니다!

<br>

# 링크로 이동하려면 사진을 클릭

[![어서오셔 ㅎ](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQk-zPB4TCuWRNJVIF0aWgniDPNJgUTdXmILg&usqp=CAU)](https://discord.gg/zkzk5xtm)


---
# 참고 &nbsp;
---
'git 공식홈페이지' &nbsp;&nbsp;&nbsp;&nbsp;   [3.6 Git 브랜치 - Rebase 하기](https://git-scm.com/book/ko/v2/Git-%EB%B8%8C%EB%9E%9C%EC%B9%98-Rebase-%ED%95%98%EA%B8%B0)