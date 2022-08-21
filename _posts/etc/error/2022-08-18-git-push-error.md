---
title:  "[error: failed to push some refs to] 깃 푸시 에러"
categories: [etc, error] 
tags: [git error]
toc: true
toc_sticky: true
date: 2022-08-18
---

<br>
<br>
<br>
<br>

> # 🚨문제 발생 &nbsp;
> * 사진 참고

<br>
<br>

```bash
git push -u origin main
```
입력했더니

![Desktop View](/assets/img/git-error/git-push/1.PNG)

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

```bash
git pull origin main
```
을 해보았더니

> fatal: Couldn't find remote ref main.  Unexpected end of commands stream

<br>



![Desktop View](/assets/img/git-error/git-push/4.PNG)

<br>
<br>
<br>

```bash
$git fetch origin
```
맨 밑줄에 unrelated histories가 있다 그러면 병합해주자!

<br>

![Desktop View](/assets/img/git-error/git-push/5.PNG)

<br>
<br>
<br>

```bash
git pull origin --allow-unrelated--histories
```

![Desktop View](/assets/img/git-error/git-push/6.PNG)

<br>
그 후 git push로 해결 완료!

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
