---
title:  "[error: npm init]"
categories: [etc, error] 
tags: [npm]
toc: true
toc_sticky: true
date: 2022-09-07
---

<br>
<br>
<br>
<br>

> # 🚨문제 발생 &nbsp;
> * 사진 참고

<br>
<br>

```console
npm init
```
입력했더니

![Desktop View](/assets/img/error/npm/1.PNG)

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



---
# 참고
---
 '지옥의불구덩이' &nbsp;&nbsp;&nbsp;&nbsp;   [VSCode 오류 : 이 시스템에서 스크립트를 실행할 수 없으므로 ...](https://hellcoding.tistory.com/entry/VSCode-%EC%98%A4%EB%A5%98-%EC%9D%B4-%EC%8B%9C%EC%8A%A4%ED%85%9C%EC%97%90%EC%84%9C-%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EB%A5%BC-%EC%8B%A4%ED%96%89%ED%95%A0-%EC%88%98-%EC%97%86%EC%9C%BC%EB%AF%80%EB%A1%9C)
