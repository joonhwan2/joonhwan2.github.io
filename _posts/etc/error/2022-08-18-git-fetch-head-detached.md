---
title:  "[head detached at] git push가 되지않음, head detached at"
categories: [etc, error] 
tags: [git error]
toc: true
toc_sticky: true
date: 2022-08-18
---

<br>

> ## '공대 아로마'님의 블로그를 참고하여 작성했습니다.    
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>


<br>
<br>

> # 🚨문제 발생 &nbsp;
> `git fetch origin/main` 을 한 후\
> `git push origin main`후 오류가 뜸\
> `git status`을 하였더니 사진과 같은 오류
>
> * 사진 참고

<br>
<br>


![Desktop View](/assets/img/git-error/head-detached-at/1.PNG)

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>


> # 🔑내가 해결한 방법 
#### (저는 git 연습할 겸 빠른 해결법을 사용하지 않았습니다)

<br>
<br>

```bash
git branch
```
깃 브랜치 보유상태 확인 명령어

![Desktop View](/assets/img/git-error/head-detached-at/3.PNG)

<br>
<br>
<br>
<br>
<br>

```bash
git branch joonhwan              (`준환` 브랜치 만들기)  `준환` 브랜치가 없는 경우 가능 
git checkout joonhwan            (`준환`을 주요 브랜치로 쓰겠다)
```

> ### 참고 ㅎ
> * git checkout -b joonhwan   
> 준환을 주요 브랜치로 만들어 쓰겠다\
> (준환 브랜치를 만들지 않은경우 가능)

![Desktop View](/assets/img/git-error/head-detached-at/4.PNG)



이어서

<br>

```bash
git branch -D candy              ( candy 브랜치 제거)
git branch -D water              ( water 브랜치 제거)
git branch -D main               ( main 브랜치 제거)
git branch -m joonhwan main      (브랜치 joonhwan -> main)
git push origin main
```
# 끝


# [준환과 함께 깃 명령어 알아보러 가기🤓](https://joonhwan2.github.io/posts/git-add/)

<br>
<br>
<br>
<br>
<br>
<br>

## 보시고 미흡한 부분이 있다면 피드백은 언제나 환영합니다!

<br>
<br>
아래 사진을 클릭하면 `제 취미공간`으로 이어집니다 ㅎㅎ 여기에서 메모 및 일상을 기록하는데 놀러오실 분들은 언제나 환영합니다!

<br>

# 링크로 이동하려면 사진을 클릭

[![어서오셔 ㅎ](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQk-zPB4TCuWRNJVIF0aWgniDPNJgUTdXmILg&usqp=CAU)](https://discord.gg/zkzk5xtm)

<br>
<br>
<br>
<br>
<br>

# 참고

'공대 아로마' &nbsp;&nbsp; [git push가 안 될 때 / detached HEAD 문제 해결 방법](https://aroma-dev.tistory.com/4)

<br>
