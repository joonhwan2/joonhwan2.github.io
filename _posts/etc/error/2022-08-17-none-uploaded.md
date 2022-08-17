---
title:  "[git error no commits updated] 파일 업로드가 안됨"
categories: [etc, error] 
tags: [git error]
toc: true
toc_sticky: true
date: 2022-08-17
---

<br>
<br>
<br>

## ❗  주의사항
> 저의 개인적인 경험으로 해결됬으므로 꼭 100% 정답이라고 할 수는 없습니다. 

<br>
<br>
<br>
<br>
<br>
<br>

> # 🚨문제 발생
> * 로컬 저장소에서 git push했는데 원격저장소에 업로드가 되지않는다!!.

<br>
<br>
<br>
<br>
<br>

> # 🗝내가 해결한 방법 
```bash
git branch
```
이걸 통해 아래와 같이 확인
```
*master    main
```

```bash
git branch -D main    (main 브랜치 제거)
git branch -m master main
git add ~ push
```
깃허브 홈페이지에 들어가보니 결과는 성공! 파일이 업로드가 되었다!



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