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

## 1  &nbsp; `windowspowershell`을 입력하여 관리자로 실행합시다
<img src="/assets/img/error/npm/2.PNG" width="500" height="500">

<br>
<br>
<br>
<br>
<br>



<br>
<br>
<br>

## 2  &nbsp; 권한상태 확인

<br>

<img src="/assets/img/error/npm/3.PNG" width="700" height="600">

```console
 get-ExecutionPolicy
 ```
 이걸 입력해보면 Restricted 라고 나올겁니다

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

## 3  &nbsp; 권한변경 및 변경이 잘 되었는지 확인

<br>

![Desktop View](/assets/img/error/npm/4.PNG)

<br>
<br>
<br>

```
- 권한 상태값 참고

 Restricted : default설정값으로, 스크립트 파일을 실행할 수 없습니다.

 AllSigned : 신뢰할 수 있는(서명된) 스크립트 파일만 실행할 수 있습니다.

 RemoteSigned : 로컬에서 본인이 생성한 스크립트와, 신뢰할 수 있는(서명된) 스크립트 파일 실행할 수 있습니다.

 Unrestricted : 모든 스크립트 실행가능

 ByPass : 경고/차단 없이 모든 것을 실행가능하도록함

 Undefined : 권한을 설정하지 않겠음
```

<br>
<br>
<br>

```console
Set-ExecutionPolicy RemoteSigned
```
입력후 y 입력하고 엔터 

```console
 get-ExecutionPolicy
 ```
그리고 `RemoteSigned`로 변경이 잘 되었는지 확인해봅시다\
다 되었다면 vscode나 cmd에서 npm init 입력하였을때 잘 작동할겁니다!

<br>
<br>
<br>
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

[![어서오셔 ㅎ](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQk-zPB4TCuWRNJVIF0aWgniDPNJgUTdXmILg&usqp=CAU)](https://discord.com/channels/976352361142452234/976352361142452239)



---
# 참고
---
 '지옥의불구덩이' &nbsp;&nbsp;&nbsp;&nbsp;   [VSCode 오류 : 이 시스템에서 스크립트를 실행할 수 없으므로 ...](https://hellcoding.tistory.com/entry/VSCode-%EC%98%A4%EB%A5%98-%EC%9D%B4-%EC%8B%9C%EC%8A%A4%ED%85%9C%EC%97%90%EC%84%9C-%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EB%A5%BC-%EC%8B%A4%ED%96%89%ED%95%A0-%EC%88%98-%EC%97%86%EC%9C%BC%EB%AF%80%EB%A1%9C)