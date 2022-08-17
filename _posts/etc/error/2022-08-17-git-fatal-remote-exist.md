---
title:  "[fatal: remote origin already exists] 깃 푸시 오류"
categories: [etc, error] 
tags: [git error]
toc: true
toc_sticky: true
date: 2022-08-17
---

<br>
<br>
<br>
<br>

> # 🚨문제 발생

```
fatal: remote origin already exists
```

<br>
<br>
연결오류라고 하니 자 이제 기존 연결을 끊고 재연결을 해봅시다.
<br>
<br>
<br>
<br>

> # 🗝내가 해결한 방법

```
git remote rm origin     (기존 연결 끊기)
git remote add origin 깃 리포 주소 입력
git remote -v          (잘 연결되었나 확인)
git push origin main
```



<br> 


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