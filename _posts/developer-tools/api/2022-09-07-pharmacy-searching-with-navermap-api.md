---
title:  "Node.js & 네이버지도로 내 근처 약국들 찾기 1"
categories: [developer-tools, api, Node.js]
tags: [api, nodejs]
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
# 1 &nbsp; 폴더 만들기, 네이버api 가입, 공공데이터 포털
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
<br> 

![Desktop View](/assets/img/error/npm/1.PNG)

<br>

이렇게 나옵니다 근데 당황하진 맙시다! 왜냐하면 우린 고칠 수 있으니까!
### [오류를 고치기 위해 이걸 클릭](https://joonhwan2.github.io/posts/npm-init-error/)

<br>
<br>

자 오류를 해결했으면 이 경로에서 C:\nodejs_pharmacy> 이어서 입력합시다
```console
npm init
```
이거 입력하면
어떤 항목들이 나오는데 쭉 엔터하다가 마지막에\
`is this OK? (yes)` 라는 문장이 나오는데 여기서 y입력을 하고 엔터누르면 `package.json`이 생성됩니다.\
저 파일을 들어가보면 버젼은 어떻고 메인으로 index.js 파일을 써라 등 기본정보가 나와있습니다.

이제 이어서 
```console
npm install express
```
입력 후 잘 설치가되었다면 현재 경로에 `폴더 node_modules` 그리고 `package-lock.json`이 생성되어 있을 것입니다.
<br>
<br>
<br>
<br>
<br>

이제 `index.js` 파일을 만들어줍시다.

```javascript
let express = require("express");                           // 1
let app = express ();                                       // 2
let port = process.env.PORT || 80;                // 포트 80 변수 선언
app.use(express.static("public_html"));                     // 3


app.listen(port,function(){                                  //4
    console.log("HTML 서버 시작됨")
})
```

1) &nbsp; express모듈을 선언   +   어떤 모듈을 쓸건지 require로 지정   =    이제 이 구문을 통해 express모듈이 사용 가능한 상태가 됨
2) &nbsp; app 이라는 변수 하나를 더 선언하여 express객체를 할당하자     이제 이 구문을 통해 app이라는 변수는 express 모듈을 가르키게 됨
3) &nbsp; express의 use 메소드를 선언하고 express.static이라고 괄호 사이에 입력한 후  public_html 로 지정하겠다\
&nbsp; 이제 public_html 폴더 아래에 있는 모든 파일들은 app.use 즉 express 모듈의 웹서버가 구동되게함
4) express 서비스가 작동될 포트 지정 보통 80번 많이씀 그리고 포트 열렸는지 확인해주기위해 콘솔을 적음\
app.listen에 port를 사용해주고 이건 사용되기 이전에 선언되야 하니 위에 포트 80변수를 선언해주자

<br>
★ 사실 nodejs heroku를 작동시킬때 `process.env.PORT || 80;` 이 포트환경설정 문구가 있으면 헤로쿠에서도 작동이 잘된다.\
console.log 추가후 잘 작동되는지 확인해봅시다
```javascript
node index.js
```
입력합시다, 잘 나온다면 아래와 같은 사진처럼 나올겁니다.

![Desktop View](/assets/img/api/naver-map-api-pharmacy/1.PNG)

<br>
<br>

이를 통해 정상적으로 app.listen이 진행되어 80번 포트가 열려있고 html서버가 express모듈을 통해 실행되면서\
80번 포트를 통해 제공되는걸 알 수 있습니다.

<br>
<br>
<br>

app.use에 적었던 것과 같이 이제 같은 경로에 `public_html 폴더` 만들어줍시다, 그 폴더 안에 있는 내용이 웹서버로 서비스됨.\
vscode에서 ctr + n 눌러 새로운 파일을 생성후 ctr + s 눌러서 public_html 안에 index.html로 저장해주고 index.html에 `ㅎㅇ루` 입력한 뒤\
주소창에 localhost 쳐봅시다.

<br>
<br>
<br>
<br>

![Desktop View](/assets/img/api/naver-map-api-pharmacy/2.PNG)

<br>

위의 화면처럼 안나오고 혹시 아래 화면과 같이 나온다면
<br>
<br>
<br>
<br>
<br>
<br>

![Desktop View](/assets/img/api/naver-map-api-pharmacy/3.PNG)

<br>
<br>

또잉?   &nbsp;&nbsp;이유는 아래사진을 보면 나옵니다

<br>
<br>
<br>

![Desktop View](/assets/img/api/naver-map-api-pharmacy/4.PNG)

<br>
<br>
<br>
<br>

그건 `node index.js`가 꺼져있기 때문에 다시 틀어주면 됩니다!\
편한 방법으로 vscode 터미널에서 화살표 방향 윗키 누르면 그 명령어 다시 불러옵니다.\
이제 그러고 다시 주소창에 localhost  친곳에서 강력한 새로고침 (ctr + shirt + r) 누르면 홈페이에 `ㅎㅇ루` 나올겁니다.

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

### 1-2 &nbsp; 네이버 지도 api 불러오기

<br>
<br>

ncloud.com -> 회원가입 --> 로그인 --> 우측상단 콘솔
검색 --> naver api --> 이렇게 나온다 aiㆍnaver api --> application
--> application 등록 --> 약국찾기인 경우 Maps 선택

> 다 선택해도 상관없는데 저의 경우는 나머지는 체크 해제하고 이거 3개만 체크하였습니다.
> * Web dynamic Map   (웹페이지에 지도를 띄우기 위함)
> * geocoding   (위도 경로 좌표를 주소로 변환)
> * Reverse Geocoding  (주소를 위도 경로로 변환)

![Desktop View](/assets/img/api/naver-map-api-pharmacy/5.PNG)

<br>

인증정보🔑 옆에있는 `수정` 클릭하면 아래와 같은 사진이 보일겁니다.
<br>

★참고 `인증정보🔑` 이거 나중에 필요합니다. 클릭하여 안에 들어가보면\
client id, client secret 값이 나오는데 네이버 지도를 띄울때 사용됩니다.

<br>
<br>
<br>

![Desktop View](/assets/img/api/naver-map-api-pharmacy/6.PNG)

<br>
<br>
<br>
<br>
<br>
<br>

URL란에 추가 -> 등록 &nbsp;&nbsp;&nbsp;&nbsp; http://localhost 

<br>
<br>
<br>
<br>
<br>

이제 지도를 불러옵시다, 2가지방법중 아무거나
1. 검색 -> 네이버 지도 예제
2. https://navermaps.github.io/maps.js/docs/tutorial-digest.example.html

<br>

`지도 기본 예제` 클릭후 아래로 내리면 이런 코드가 보일겁니다
```javascript
//지도를 삽입할 HTML 요소 또는 HTML 요소의 id를 지정합니다.
var mapDiv = document.getElementById('map'); // 'map'으로 선언해도 동일

//옵션 없이 지도 객체를 생성하면 서울 시청을 중심으로 하는 16 레벨의 지도가 생성됩니다.
var map = new naver.maps.Map(mapDiv);
```
html요소의 id를 넣어주면 그 id를 기반으로 네이버 지도 api가 작동한다는 내용이다.

<br>
<br>
<br>

이제 index.html로 가서 내용을 수정합시다.<br>
head태그 - 디자인적인 부분은 아니지만 상단에 정의되야할 부분을 미리 정의해줌&nbsp;&nbsp;&nbsp;
ex) 제목, CSS부분, script(자바스크립트 부분) 등\
<br>
body태그 - 실제 본문

```html
<html>
    <head>
        <script type ="text/javascript" src="주소"></script>
    </head>
        <body>

        </body>
</html>
```

<br>
<br>

자 이제 주소를 찾기 위해 아까 열었던 `지도 기본 예제`로 갑시다 
<br>
<br>
<br>
<br>
<br>


![Desktop View](/assets/img/api/naver-map-api-pharmacy/7.PNG)

<br>
<br>

지도 기본 예제의 흰공간 --> 우클릭으로 페이지소스 보기 --> ctr f 로 script 검색 후 open api 내용 한줄 쭉 복사후\
기존 <script type 부분을 없앤 후 index.html에 붙여넣기합시다

![Desktop View](/assets/img/api/naver-map-api-pharmacy/8.PNG)

<br>
<br>
그럼 아래사진처럼 됩니다

![Desktop View](/assets/img/api/naver-map-api-pharmacy/9.PNG)

id는 아까 `인증정보🔑` 이거 있던곳에서 저걸 클릭하여 아이디 복붙해서 바꿔주면되고\
submodule은 geocoder만 남겨놓으면 됩니다 geocoder는 `주소 ->좌표`, `좌표 -> 주소` 를 바꿔주는 기능을 하기때문에 꼭 필요합니다\
여기까지 코드는 이렇게 됩니다.

```html
<html>
    <head>
        <script type="text/javascript" src="https://openapi.map.naver.com/openapi/v3/maps.js?ncpClientId=보안을 위해 제 아이디 입력 안함&amp;submodules=geocoder"></script>
    </head>
        <body>

        </body>
</html>
```
이제 편의성을 위해 jquery cdn을 사용하겠습니다.\
구글검색 -> jquery cdn

<br>
<br>
<br>


![Desktop View](/assets/img/api/naver-map-api-pharmacy/10.PNG)

<br>
<br>
<br>
<br>
<br>
<br>

2022.09.08 기준으로 제일 최신버젼인 3.x를 사용합시다 그리고 `minified` 클릭하면 <script> 코드들이 나오는데 싹 다 복사하여 <script type -- 바로 아래에 붙여줍시다 하게된다면 코드는 이렇게 됩니다
                                                                                            
 ```html
 <html>
    <head>
        <script type="text/javascript" src="https://openapi.map.naver.com/openapi/v3/maps.js?ncpClientId=보안을 위해 제 아이디 입력 안함&amp;submodules=geocoder"></script>
        <script src="https://code.jquery.com/jquery-3.6.1.min.js" integrity="sha256-o88AwQnZB+VDvE9tvIXrMQaPlFFSUTR+nldQm1LuPXQ="crossorigin="anonymous"></script>
    </head>
        <body>

        </body>
</html>                                                                                           
 ```      
 <br>
 <br>
 <br>
 <br>
 <br>
 <br>
 
이제 `naver map api` 그리고 그 아래줄에 `제이쿼리`도 활성화 됩니다. 이제 디자인적 요소를 넣기위해 body태그 안에 div객체를 만듭시다,\
이 div객체가 `네이버지도`가 들어올 곳입니다.

* 우선 div 객체의 id에 map으로 설정하겠습니다 스타일은 폭 100% 너비 80%
* 그위에 div객체 하나 더 생성해 `약국지도💊` 입력 후 스타일 지정해줍시다 `상단여백 20픽셀`, `하단여백 10픽셀`, `글자 굵게 보이기`
* body 태그 안에 script 구문 추가 document가 ready상태일때 function(기능)을 하기위해 코드를 넣어야하는데\
그건 아까 `지도 기본 예제`에 있던 코드 복사하면 된다.

<br>
<br>
<br>

```html
<head>     
    <script type="text/javascript" src="https://openapi.map.naver.com/openapi/v3/maps.js?ncpClientId=보안을 위해 제 아이디 입력안함&amp;submodules=geocoder"></script>
    <script src="https://code.jquery.com/jquery-3.6.1.min.js" integrity="sha256-o88AwQnZB+VDvE9tvIXrMQaPlFFSUTR+nldQm1LuPXQ=" crossorigin="anonymous"></script>
</head>    
    <body>
        <div style="margin-top: 20px; margin-bottom: 10px; font-weight: bold;">
            약국 지도💊
        </div>
        <div id="map" style="width:100%; height:80%">

        </div>
    </body>
        <script>
            $(document).ready(function(){
                //지도를 삽입할 HTML 요소 또는 HTML 요소의 id를 지정합니다.
                var mapDiv = document.getElementById('map'); // 'map'으로 선언해도 동일

                //옵션 없이 지도 객체를 생성하면 서울 시청을 중심으로 하는 16 레벨의 지도가 생성됩니다.
                var map = new naver.maps.Map(mapDiv);
            });
        </script>
```

<br>
<br>
<br>

이제 다시 그 localhost 홈페이지로 가서 ctr + shift + r (강력 새로고침)하면 아래처럼 지도가 빰!

<br>
<br>
<br>
<br>

![Desktop View](/assets/img/api/naver-map-api-pharmacy/11.PNG)

<br>
<br>
<br>

<del>문제가 하나 있는데 접속한 사람의 위치에 따른 지도가 떠야하는 것이다</del>, 위도와 경도정보를 등록합시다
<br>
<br>
1) 이제 function을 하나 만들어봅시다 이름은 getLocation으로, 사용할 객체는 navigator.geolocation (gps 관련 객체)

2) navigator.geolocation.getCurrentPosition 이걸 사용하면 현재 위치를 알 수 있습니다, 그 인자로\
function을 적고 position 이라는 객체를 통해 현 위치를 알 수 있습니다.

3) getLocation은 위도, 경도 2개를 반환해야하기 때문에 미리 객체를 만들어줍시다\
let XY로 XY변수를 Object형으로 선언해줍시다.\
그리고 XY.lat, XY.lng 옵션에 각각 위도, 경도값 저장해줍시다.

4) 아래엔 XY를 return해줍시다 = 현재위치의 XY좌표(위도, 경도) 반환
<br>
★참고 `getCurrentPosition` 이게 비동기형이라 이대로 하면 문제가 됨 즉 동기형으로 바꾸어주어야 한다

5) 일단 잘 작동되는지 확인하기위해 getLocation을 실행하고 거기서 return된 값을 XY로 받아 alert로 경도,위도값 띄워봅시다

```javascript
</body>
        <script>
            $(document).ready(function(){
                let XY = getLocation();                 // 5
                alert("위도" + XY.lat);
                alert("경도" + XY.lng);
                //지도를 삽입할 HTML 요소 또는 HTML 요소의 id를 지정합니다.
                var mapDiv = document.getElementById('map'); // 'map'으로 선언해도 동일

                //옵션 없이 지도 객체를 생성하면 서울 시청을 중심으로 하는 16 레벨의 지도가 생성됩니다.
                var map = new naver.maps.Map(mapDiv);
            });
            
            function getLocation() {                            // 1
                let XY = new Object();           // 3-1
                if(navigator.geolocation) {                      // 2
                    navigator.geolocation.getCurrentPosition(function(position){
                        //위도 position.coords.latitude
                        //경도 position.coords.longitude
                        XY.lat = position.coords.latitude;           // 3-2
                        XY.lng = position.coords.longitude;
                    });
                }
                return XY;              // 4
            }
        </script>
```

<br>
<br>
<br>

자 이상태로 바로 localhost 보면 안 나올수도 있기때문에 vscode 종료하고 localhost홈페이지도 끄고 둘다 다시 재실행시킵시다.\
그러면 아래와 같은 사진과 함께 이 2문구가 나올겁니다, `위도 undefined`   `경도 undefined`
<br>

![Desktop View](/assets/img/api/naver-map-api-pharmacy/12.png)
지도가 잘 나오지않는데 그 이유는 그 이유는 아까 말씀드린대로 `getCurrentPosition`가 비동기형이기때문입니다\
이제 동기형으로 바꿔주기위해 코드를 수정합시다

기존 alert 2줄 위치를 XY.lat, XY.lng 아래로 옮긴 후 확인해보면 위도, 경도와 함께 지도가 잘 나옵니다


```javascript
        <script>
            $(document).ready(function(){
                let XY = getLocation();

                //지도를 삽입할 HTML 요소 또는 HTML 요소의 id를 지정합니다.
                var mapDiv = document.getElementById('map'); // 'map'으로 선언해도 동일

                //옵션 없이 지도 객체를 생성하면 서울 시청을 중심으로 하는 16 레벨의 지도가 생성됩니다.
                var map = new naver.maps.Map(mapDiv);
            });
            
            function getLocation() {
                let XY = new Object();
                if(navigator.geolocation) {
                    navigator.geolocation.getCurrentPosition(function(position){
                        //위도 position.coords.latitude
                        //경도 position.coords.longitude
                        XY.lat = position.coords.latitude;
                        XY.lng = position.coords.longitude;
                        alert("위도" + XY.lat);
                        alert("경도" + XY.lng);
                    });
                }
                return XY;
            }
        </script>
```
자, `function getLocation() {`  &nbsp;&nbsp;&nbsp;&nbsp; 여기부터는 비동기입니다 getLocation이 비동기이기 때문에,\
무슨 말이냐면 if 구문안의 식이 진행되다가 밑의 alert까지 읽기 전에 XY.lat에서 값을 return 할 수도 있다는 얘기입니다.
<br>

ex) 휴대폰으로 햄버거세트를 10개 주문하여 주문프로그램에서 그걸 인지하고 그 주문정보를 나에게 다시 반환해주었는데\
정작 나의 주문량은 7개로 되어있음

<br>
<br>

### 비동기형 -> 동기형&nbsp;&nbsp;&nbsp;(전환)
1) `function getLocation() {` &nbsp;&nbsp; 앞에 async를 넣음으로써 이 function 안에 비동기구문이 있다고 표시

2) `getCurrentPosition`을 promise 객체를 통해 재정의
```javascript
let XY = new Object();
if(navigator.geolocation) {
```
이곳 바로 아래에 추가

```javascript
            let promise = new Promise((resolve, rejected) => {
                        navigator.geolocation.getCurrentPosition((position) =>{
                            
                        });
                    });
```
`=>`는 function으로 지정한 부분과 같은 역할을 수행, 그리고 `getCurrentPosition`이 실행될 때\
resolve를 적고 그 인자로 position을 적으면 `getCurrentPosition`을 Promise로 감싸고 또 성공했을 때\
position 객체를 반환하며 실패하면 rejected

3) 아래에 이어서 position 변수 생성하고 선언한 후 await 합시다\
Promise 변수는 getCurrentPosition을 가리키며 await(끝날때까지 기다림)해줍니다.\
비동기가 끝나고 나면 이 position 변수에 그 값이 들어와 동기형태를 띄게됨

4) `let position = await promise;`여기서 navigator.geolocation이 한줄없애고 맨 밑의 alert 2문장을 잠시 다른데 보관 그 후, \
실행해주면 getLocaiton function이 동기식으로 변형됨\
= 현재의 위치를 받은 후, 동기식으로 마지막에 XY좌표값이 return됨


5) 이 코드 아래에 아까 그 alert 두 문장 옮깁시다
```javascript
$(document).ready(function(){
  `let XY = getLocation();  
```

6) 그리고 이제 이 `getLocation 호출하는 이 function코드`에도 async를 넣어주고,\
getLocation을 실행할때 await을 하나 더 넣어줍시다

```javascript
                $(document).ready(function(){
                    let XY = getLocation();
---
---                    
```

<br>

--->

<br>

```javascript
                $(document).ready(async function(){
                    let XY = await getLocation();
---
---
```

<br>
<br>
<br>

여기 까지의 코드는 이렇습니다
```javascript
            <script>
                $(document).ready(async function(){
                    let XY = await getLocation();

                    alert("위도" + XY.lat);
                    alert("경도" + XY.lng);

                    //지도를 삽입할 HTML 요소 또는 HTML 요소의 id를 지정합니다.
                    var mapDiv = document.getElementById('map'); // 'map'으로 선언해도 동일

                    //옵션 없이 지도 객체를 생성하면 서울 시청을 중심으로 하는 16 레벨의 지도가 생성됩니다.
                    var map = new naver.maps.Map(mapDiv);
                });
                
                async  function getLocation() {
                    let XY = new Object();
                    if(navigator.geolocation) {

                        let promise = new Promise((resolve, rejected) => {
                            navigator.geolocation.getCurrentPosition((position) =>{
                                resolve(position);
                            });
                        });

                        let position = await promise;
                        
                        //위도 position.coords.latitude
                        //경도 position.coords.longitude
                        XY.lat = position.coords.latitude;
                        XY.lng = position.coords.longitude;
                           
                    }
                    return XY;
                }
            </script>
```
이제 실행해보면 위도, 경도 값 및 네이버지도가 동기식으로 바뀌어 잘 나온다는 것을 알 수 있습니다.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

### 1-3 &nbsp; 공공데이터 사용

<br>

주소창에 [data.go.kr](https://www.data.go.kr/) --> 로그인 --> 검색 : `국립중앙의료원_전국 약국 정보 조회 서비스` --> 신청\

<br>
이제 마이페이지에서 약국서비스를 선택한 후 조금 내려오면 

![Desktop View](/assets/img/api/naver-map-api-pharmacy/13.png)

<br>
확인 -> 미리보기 -> 주소창 복사하여 일단 index.js에 아무곳에나 주석처리하여 보관
https://apis.data.go.kr/B552657/ErmctInsttInfoInqireService/getParmacyListInfoInqire?serviceKey=mL6hpE93V2cGEHnZNYbp2kbpZIm2IFyc9rhdh2wIaUseyjghN%2FlJSV7tSchmbL47mZsX8gNcLVtGpsTxQkstdA%3D%3D&Q0=%EC%84%9C%EC%9A%B8%ED%8A%B9%EB%B3%84%EC%8B%9C&Q1=%EA%B0%95%EB%82%A8%EA%B5%AC&QT=1&QN=%EC%82%BC%EC%84%B1%EC%95%BD%EA%B5%AD&ORD=NAME&pageNo=1&numOfRows=10

<br>
<br>

이제 index.js 로 가보면 app.use를 통해 웹 서버를 열은 것을 알 수 있는데,\
한 페이지만 따로 app.get을 사용하여 열어봅시다, 열기위해 사용할 페이지 이름은 pharmach_list\
이제 "/pharmach_list" 이 경로로 접속하는 사용자들에게 자료제공 가능

<br>
<br>
<br>

> * 참고
> * `res(response)` : node.js에서 클라이언트들에게 데이터를 보냄
> * `req(requests)` : 사용자로부터 전달받는 내용 

<br>
<br>
<br>

이제 아래에 `api`라는 변수를 선언허
* 참고 api.get 역시 비동기식이라 async 입혀줍시다\
api로 작동시킬 모듈은 `axios`, get메소드를 통해 웹상의 정보를 가져옴
<br>

아까 주석처리해놓은 주소를 https -> http로 바꾸어 ?앞까지만 복사하여 붙입시다\
?뒤의 것들은 params라는 변수로 받읍시다 콤마하고 한칸 띄워서 붙이면됩니다,\
그리고 params는 하나가 아니라 여러개니 대괄호 처리해줍시다

<br>

넣을 항목들
* `"serviceKey": ""` &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ""안에는 serviceKey= 여기 값부터 &QO앞까지만 복사하여 넣으면됩니다 그리고 콤마, 
* Q0 (2번째인자)    &nbsp;&nbsp;&nbsp; 이곳에 넣을 정보로는 공공데이터포털 -> 마이페이지 -> 약국목록정보 조회 확인 클릭
* Q1 &nbsp;&nbsp;&nbsp;&nbsp; Q0과 같은방법으로
* --
* --

그리고 없어도 되는 값은 넣지 않았습니다. &nbsp;&nbsp;&nbsp;&nbsp; 여기까지의 코드는 아래와 같습니다

```javascript
app.get("/pharmach_list", (req, res) => {
    let api = async() => {
        axios.get("http://apis.data.go.kr/B552657/ErmctInsttInfoInqireService/getParmacyListInfoInqire", {
            params: {
                "serviceKey": "mL6hpE93V2cGEHnZNYbp2kbpZIm2IFyc9rhdh2wIaUseyjghN%2FlJSV7tSchmbL47mZsX8gNcLVtGpsTxQkstdA%3D%3D",
                "Q0":"서울특별시",
                "Q1":"강남구",
                "QT":"",
                "QN":"",
                "ORD":"",
                "pageNo":"1",             //페이지 한개에만 보겠다는 뜻
                "numOfRows":"1000",       // 보일 목록건수 1000개
            }
        })
    }
});
```
근데 axios.get 역시 비동기형태이기 때문에 동기형으로 바꾸어줍시다\
awiat를 적어주고 그 결과값을 response라는 변수로 받읍시다, 그걸 위해 그 이전에 response 변수 선언해줍시다\
초기값 null로 설정후 나중에 아래와같은 데이터로 받게됩니다. 그리고 마지막을`return response`로\
response 객체를 반환합니다. 그럼 이제 pharmach_list로 들어왔을 때\
이 api결과`(serviceKey, return response 등)`가 출력됨

```javascript
app.get("/pharmach_list", (req, res) => {
    let response = null
    let api = async() => {
        response = await axios.get("http://apis.data.go.kr/B552657/ErmctInsttInfoInqireService/getParmacyListInfoInqire", {
            params: {
---
---
---
    return response;
});
```
그리고 위에 axios 모듈을 추가해줍시다.
```javascript
let axios = require("axios");
```

<br>
<br>
<br>
<br>
<br>
<br>

잠시 점검! index.js의 여기까지의 코드는 이렇습니다
```javascript
let express = require("express");  
let axios = require("axios");

let app = express ();                                       
let port = process.env.PORT || 80;                
app.use(express.static("public_html"));                     


app.listen(port,function(){                                  
    console.log("HTML 서버 시작됨")
})

app.get("/pharmach_list", (req, res) => {
    let response = null
    let api = async() => {
        response = await axios.get("http://apis.data.go.kr/B552657/ErmctInsttInfoInqireService/getParmacyListInfoInqire", {
            params: {
                "serviceKey": "mL6hpE93V2cGEHnZNYbp2kbpZIm2IFyc9rhdh2wIaUseyjghN%2FlJSV7tSchmbL47mZsX8gNcLVtGpsTxQkstdA%3D%3D",
                "Q0":"서울특별시",
                "Q1":"강남구",
                "QT":"",
                "QN":"",
                "ORD":"",
                "pageNo":"1",
                "numOfRows":"1000",
            }
        })
    }
    return response;
});

```
이제 axios모듈을 쓰겠다고 정의해주었으니
```console
npm install axios
```
해줍시다.

<br>
<br>
<br>
<br>

이어서
```console
node index.js
```
입력하게 되면 오잉??

<br>
<br>
<br>
<br>

![Desktop View](/assets/img/api/naver-map-api-pharmacy/14.png)

<br>
<br>
<br>

### 🚨 문제 발생 
* 에러이벤트를 핸들링하지 않았다
* 80번 포트가 이미 사용중이다

<br>
<br>
<br>
<br>
<br>

### 🔑해결방법
* axios모듈을 쓰면 try catch문으로 에러 핸들링을 해줍시다

우선 try catch문을 만들어 try문 안에 api구문을 다 넣어주었고,\
이 try문에서 에러 발생시 그 에러가 catch로 오게됨, 그 예외상황이 오면 콘솔로그로 에러상황을 찍음\
그리고 nodejs로 api내용을 가져오는 이유는 cors보안문제를 해결하기 위함입니다.\
`.then`이라고 작성해주면 이 api기능이 실행될때 오류를 잡을 수 있습니다. 약국정보 있는게 response고,\
res는 이 api를 사용하는 객체에게 전달해주는 값입니다, 이제 이 api를 호출하는 대상에게\
데이터를 남기기 위해 두번째 res변수로 `res.setHeader`를 지정합니다. 여기에 보면 `*`이 있는데 *=all\
앞의 allow부터 쭉 읽어보면 가능한 것들은 다 허가해줘라는 뜻입니다.

<br>

이제 이 api에 접속한 대상에게 api데이터를 전달하기 위해 response.body 입력,\
이렇게 되면 약국데이터를 저 경로로 접속한 컴퓨터에게 제공해줍니다.

코드는 이렇게 변형되었습니다

```javascript
app.get("/pharmach_list", (req, res) => {
    let response = null
    try {
        let api = async() => {
            response = await axios.get("http://apis.data.go.kr/B552657/ErmctInsttInfoInqireService/getParmacyListInfoInqire", {
                params: {
                    "serviceKey": "mL6hpE93V2cGEHnZNYbp2kbpZIm2IFyc9rhdh2wIaUseyjghN%2FlJSV7tSchmbL47mZsX8gNcLVtGpsTxQkstdA%3D%3D",
                    "Q0":"서울특별시",
                    "Q1":"강남구",
                    "QT":"",
                    "QN":"",
                    "ORD":"",
                    "pageNo":"1",
                    "numOfRows":"1000",
                }
            })
        }
        api().then((response) =>  {
            res.setHeader("Access-Control-Allow-Origin", "*")
            res.json(response.data.response.body);
        });
    }
    catch(e) {
        console.log(e);
    }

    return response;
});
```

<br>
<br>
<br>
<br>
<br>

* 80번포트가 이미 사용중 에러\
앞서 실행중인 메모리가 콘솔에 남아있기 때문에 vscode를 껏다키면 해결됩니다.

<br>
<br>

------------------- 한것을 잠깐 요약해보면 -------------------\
`index.js`\
express 모듈 -  웹서버 구현\
axios 모듈 - 약국 api 불러오기

(이 2가지는 벡엔드 역할을 해줍니다.)
<br>
<br>

`index.html`\
웹 브라우저에서 작동하는 페이지

# 이어서 2부에서

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
 '소스놀이터' &nbsp;&nbsp;&nbsp;&nbsp;   [Node Js로 네이버 약국 지도 만들기 #1 (공공데이터포털 오픈 API 활용)](https://www.youtube.com/watch?v=-sKd4NYS2sA&t=548s)

 <br>

 '소스놀이터' &nbsp;&nbsp;&nbsp;&nbsp;   [Node.Js로 네이버 약국 지도 만들기 #2 (express & axios 모듈, 비동기 문제 해결)](https://www.youtube.com/watch?v=FKQxItpstIk&t=1245s) &nbsp;&nbsp;&nbsp;&nbsp; 30:12 까지