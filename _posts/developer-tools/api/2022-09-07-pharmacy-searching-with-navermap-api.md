---
title:  "[api] Node.js & 공공데이터포털(전국약국조회) & 네이버 지도 api를 이용해 내 위치기반 약국들 찾기"
categories: [api, Node.js]
tags: [api]
toc: true
toc_sticky: true
date: 2022-09-07
---

<br>
<br>
<br>

'소스놀이터'님의 유튜브영상을 보고 이렇게 좋은 작업물을 완성할 수 있었습니다.\
양질의 자료를 제공해주신 '소스놀이터'님께 다시한번 감사의 말씀을 드립니다.   
- [x] vscode 설치
- [x] nodejs 설치완료
- [x] nodejs 환경변수 설정

<br>
이 3가지가 완료되어있다는 가정하에 시작하겠습니다.
<br>




<br>
<br>
<br>

---
# 1 &nbsp; 폴더 만들기 및 공공데이터포털, 네이버api 가입
---

### 1-1 &nbsp; 로컬디스크C에  미리 `nodejs_pharmacy 폴더` 만들어 놓기

<br>
미리 로컬디스크C에 nodejs_pharmacy 폴더를 만들어 놓은 후\
이제 cmd로 들어갑시다, C:\Users\rhwns> 이경로가 나와서   `cd ..` 을 2번 입력해줍시다  그러면 경로 하나씩 지워지면서\
C:/> 이쪽으로 경로가 바뀝니다 이제 여기서 cd nodejs_pharmacy 입력 ㄱㄱ
<br>
<br>

이제 cmd나 vscode를 열어 C:\nodejs_pharmacy> 이 경로에서 아래 문장 입력
```console
npm init
```

![Desktop View](/assets/img/error/npm/npm init error.PNG)

> h1은 = 3개<br>
> h2는 - 3개<br>
> `#` 는 6개까지 가능
```
이건 H1
===
이건H2
---
```

이건 H1
===
이건H2
---
![Desktop View](/assets/img/md/2.PNG)
<br>
<br>
<br>
<br>

```
# 이거 H1
## 이거 H2
### 이거 H3
#### 이거 H4
##### 이거 H5
###### 이거 H6
```

# 이거 H1
## 이거 H2
### 이거 H3
#### 이거 H4
##### 이거 H5
###### 이거 H6

---

<br>
<br>

---
# 2 줄 긋기
---



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



아래 사진을 클릭하면 `제 취미공간`으로 이어집니다 ㅎㅎ 여기에서 메모 및 일상을 기록하는데 놀러오실 분들은 언제나 환영합니다!

<br>

# 링크로 이동하려면 사진을 클릭

[![어서오셔 ㅎ](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQk-zPB4TCuWRNJVIF0aWgniDPNJgUTdXmILg&usqp=CAU)](https://discord.gg/zkzk5xtm)


---
# 참고
---
 '소스놀이터' &nbsp;&nbsp;&nbsp;&nbsp;   [Node Js로 네이버 약국 지도 만들기 #1 (공공데이터포털 오픈 API 활용)](https://www.youtube.com/watch?v=-sKd4NYS2sA&t=548s)