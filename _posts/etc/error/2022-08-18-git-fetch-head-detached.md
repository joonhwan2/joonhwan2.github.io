---
title:  "[head detached at] 깃 푸시 에러"
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

ㅇㄴ

```bash
git pull origin main
```
을 해보았더니

> fatal: Couldn't find remote ref main.  Unexpected end of commands stream

<br>

```bash
git pull --rebase origin main
```

