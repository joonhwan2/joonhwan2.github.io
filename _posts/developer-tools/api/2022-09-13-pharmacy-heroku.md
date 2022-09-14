---
title:  "Node.js & 약국지도찾기, heroku 배포하기"
categories: [developer-tools, api, Node.js]
tags: [api, nodejs, heroku]
toc: true
toc_sticky: true
date: 2022-09-13
---

<br>
<br>
<br>

'소스놀이터'님의 유튜브영상을 보고 이렇게 좋은 작업물을 완성할 수 있었습니다.\
양질의 자료를 제공해주신 '소스놀이터'님께 다시한번 감사의 말씀을 드립니다.   


---
# 1 &nbsp; Heroku 회원가입
---

<br>

![Desktop View](/assets/img/api/Heroku/1.png)

<br>

create app 클릭

<br>

![Desktop View](/assets/img/api/Heroku/2.png)


<br>
<br>

![Desktop View](/assets/img/api/Heroku/3.png)

<br>
<br>

heroku cli 클릭하면 이렇게 됩니다.

<br>

![Desktop View](/assets/img/api/Heroku/4.png)

<br>
<br>
<br>
<br>

이제 Node.js 클릭 -> view as single page -> 보이는 지침서 참고

<br>
<br>

이제 cmd를 새로 켜서 따라 입력합시다
```console
heroku login
```

![Desktop View](/assets/img/api/Heroku/5.png)

<br>
<br>
<br>

자 사진에 보면 Opening browser to https://cli~~ 나오는데 이거 주소창에\
https 주소 복붙해서 한번 쳐주고 진행합시다 그러면 로그인 됩니다.

<br>
<br>

이제 
```console
node --version
npm --version
git --version
```
확인합시다

<br>

저는 `노드  v16.17.0`, `npm  8.18.0`, 'git  2.37.1.windows.1' 이렇군요.

<br>
<br>
<br>
<br>

![Desktop View](/assets/img/api/Heroku/6.png)


<br>

해당되는 앱이름 클릭 -> setting -> Heroku git URL 복사

# 잠깐!

로컬디스크C에 폴더 하나 `HTML_SERVER_TEST` 만듭시다\
그리고 windows + R을 눌러 CMD 입력해준후 이 경로로 옵시다\
`cd ..` 입력하면 경로 하나씩 줄어듭니다\

```console
C:\HTML_SERVER_TEST> 
```

<br>
<br>
<br>
<br>

저 경로에서 

```console
git clone 해당URL
```
입력합시다

자 저의 경우는 그러면 이 경로로 빈 폴더가 생길겁니다.
`C:\HTML_SERVER_TEST\joonhwan-test`

<br>

이제 저 안에 `index.js`, `public_html 폴더`, `package.json` 복붙해줍시다

그리고 package.json 수정해줍시다.

```javascript
{
  "name": "nodejs_pharmacy",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "axios": "^0.27.2",
    "express": "^4.18.1"
  }
}
```

-----------  engines와 node 버젼 추가------------>

```javascript
{
  "name": "nodejs_pharmacy",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "engines": {
    "node": "16.x"
  },
  "scripts": {
    "start": "node index.js",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "axios": "^0.27.2",
    "express": "^4.18.1"
  }
}
```

<br>
<br>
<br>
<br>

```javascript
let port = process.env.PORT || 80;
```

그리고 index.js에서 80을 5000으로 바꿔줍시다\
그 후 깃클론으로 폴더가 생성된 경로에서 아래 명령어 입력합시다\
저의 경우는 C:\HTML_SERVER_TEST\joonhwan-test 이곳에서 실행합니다
```console
npm install
```
이제 `node index.js` 이어서 입력후 주소창에 http://localhost:5000/ 쳐봅시다.
저는 잘 나오는 군요!\
참! 이거 휴대폰으로 지도 보면 확대할수록 글자 크기가 작아져서 해줘야하는게 있습니다

<br>

index.html로 갑시다  거기서 <head> </head>\
이 사이에 이 문장 추가하면 됩니다
```
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```
이 문장을 추가해주면 휴대폰에서도 노트북에서 보는것과 같은 크기로 볼 수 있습니다.\
만약 이걸 추가 안할 시 휴대폰에서 확대할수록 크기는 더욱 작아집니다.

<br>
<br>
<br>

이제 입력합시다.
```console
git add .
git commit -m "아무거나 입력"
git push origin master
```

![Desktop View](/assets/img/api/Heroku/7.png)

<br>
<br>

이 주소를 잘 확인했다면 console.ncloud.com -> 마이페이지 -> 아래사진을 봐주세요

<br>
<br>

![Desktop View](/assets/img/api/Heroku/8.png)
![Desktop View](/assets/img/api/Heroku/9.png)

저 곳에 아까 그 주소 추가해줍시다. 그리고 테스트를 해보니 오잉?

<br>
<br>

![Desktop View](/assets/img/api/Heroku/10.png)

<br>
<br>
<br>

F12를 눌러봅시다

![Desktop View](/assets/img/api/Heroku/11.png)

<br>
<br>
<br>

아아 저거 제 아이디가 중복사용되고있었군요 바꿔주어야겠습니다. 실은 미리 하나 만들어놓고 테스트용으로 하나 더 만들어주었는데\
그 테스트용 아이디를 입력하지 않아서 생긴 문제입니다.

<br>
<br>

수정을 완료하였으니 이제 저는 이미 배포한 앱을 지우고 새로 넣겠습니다

```console
heroku apps:destroy --app joonhwan-test    // 그 후 자기 앱이름을 한번 더 입력하면 됩니다
git status    // 혹시 했는데 업데이트할 것이 아무것도 없다면 폴더를 바탕화면에 한번 옮겼다가 다시 재위치시켜보십시오
```
아무것도 없으므로 폴더를 지우고 새로 만들었습니다. 그리고 cmd 껏다 켜주겠습니다

```console
heroku login
git clone URL
```
`cd joonhwan-test` 입력해주고 그안에 자료들 다시 옮겨주고
```console
git status
git add .
git commit -m "아무거나"
git push origin master
```
![Desktop View](/assets/img/api/Heroku/12.png)

잘 나오군요! 이것으로 배포까지 끝내겠습니다 ㅎ\
https://pharmacy1234.herokuapp.com




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

[![어서오셔 ㅎ](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQk-zPB4TCuWRNJVIF0aWgniDPNJgUTdXmILg&usqp=CAU)](https://discord.com/channels/976352361142452234/976352361142452239)


---
# 참고
---
 '소스놀이터' &nbsp;&nbsp;&nbsp;&nbsp;   [무료 Node Js 호스팅 (5분만에 무료 홈페이지 소스 올리기)](https://www.youtube.com/watch?v=hqrKiKac84o)

 <br>
