---
title:  "Node.js & 네이버지도로 내 근처 약국들 찾기 2"
categories: [developer-tools, api, Node.js]
tags: [api, nodejs]
toc: true
toc_sticky: true
date: 2022-09-12
---

<br>
<br>
<br>


이제 index.html로 갑시다

```html
                $(document).ready(async function(){
                    let XY = await getLocation();

                    alert("위도" + XY.lat);
                    alert("경도" + XY.lng);
```
이 부분을 보면 document.ready 함수에서 문서가 document로 로드된 후 ready상태일 때\
현재 위치를 가리키고 그걸 alert 경고창으로 띄우는 걸 나타냅니다 일단 `alert 2개 주석`처리합시다.

그 후 index.js의 이 경로로 접속되게 합시다 `/pharmach_list`\
이걸 위해 index.html로 돌아와 ajax를 사용합니다 즉 jquery

```html
                    //지도를 삽입할 HTML 요소 또는 HTML 요소의 id를 지정합니다.
                    var mapDiv = document.getElementById('map'); // 'map'으로 선언해도 동일

                    //옵션 없이 지도 객체를 생성하면 서울 시청을 중심으로 하는 16 레벨의 지도가 생성됩니다.
                    var map = new naver.maps.Map(mapDiv);
                    $.ajax({
                        url: "/pharmach_list",
                        type: "GET",
                        cache: "false",
                        dataType: "json",
                        data: {"Q0": "서울특별시", "Q1": "강남구", "QT": "", "QN": "", "ORD": "", "pageNo": "1", "numOfRows": "1000"},
                        success: function(data) {
                            console.log(data);
                        },
                        error: function(request, status, error) {

                        }

                    });
```
1. url은 경로
2. type에서 GET방식은 주석처리한 주소의 ?뒤의 값들을 눈에 보이게 하나하나 입력해줌, 그래서\
`/pharmach_list`를 정의할 때 app.get을 사용했음
4. 데이터 값은 index.js에서 찾으면됨
5. 에러는 3가지 변수를 받음


<br>
<br>
<br>

index.js 로 가서 수정합시다

```javascript
                params: {
                    "serviceKey": "mL6hpE93V2cGEHnZNYbp2kbpZIm2IFyc9rhdh2wIaUseyjghN%2FlJSV7tSchmbL47mZsX8gNcLVtGpsTxQkstdA%3D%3D",
                    "Q0":req.query.Q0,
                    "Q1":req.query.Q1,
                    "QT":req.query.QT,
                    "QN":req.query.QN,
                    "ORD":req.query.ORD,
                    "pageNo":req.query.pageNo,
                    "numOfRows":req.query.numOfRows
                }
```

<br>
<br>

이렇게 하면 index.html로 전달한 값이 nodejs로 갔다가 nodejs에서 다시 nodejs에서 api를 호출하여\
원하는 값이 변환됩니다. index.html을 보면 나오지만 data변수의 값을 콘솔에 출력한 것을 통해\
디버깅을 가능하게하여 매우 유용합니다.

<br>
<br>
<br>
<br>
<br>
<br>

## 여기까지 요약
<br>

우선 api 변수가 실행되고 나면 .then으로 들어와 Header에다가 cors를 잡는 오류를 넣었습니다,\
그런데! response변수가 api내에서 실행되었는데 실제로 response를 return하는 부분은 바깥에 있기에
* return response를 알맞은 위치인 위쪽으로 보내줍시다.
* response 선언한 부분 역시 api 선언문 안에 넣어줍시다
* try catch문 역시 axios를 쓰는 위치로 바꿔줍시다

```javascript
app.get("/pharmach_list", (req, res) => {

        let api = async() => {
            let response = null
            
            try {
            response = await axios.get("http://apis.data.go.kr/B552657/ErmctInsttInfoInqireService/getParmacyListInfoInqire", {
                params: {
                    "serviceKey": "mL6hpE93V2cGEHnZNYbp2kbpZIm2IFyc9rhdh2wIaUseyjghN%2FlJSV7tSchmbL47mZsX8gNcLVtGpsTxQkstdA%3D%3D",
                    "Q0":req.query.Q0,
                    "Q1":req.query.Q1,
                    "QT":req.query.QT,
                    "QN":req.query.QN,
                    "ORD":req.query.ORD,
                    "pageNo":req.query.pageNo,
                    "numOfRows":req.query.numOfRows
                }
            })
        }






        catch(e) {
            console.log(e);
        }





        

            return response;
        }
        api().then((response) =>  {
            res.setHeader("Access-Control-Allow-Origin", "*")
            res.json(response.data.response.body);
        });
});
```

<br>
<br>

그리고 req.query.Q0 &nbsp;&nbsp; req.query.Q1 이런것들은 프로그램 변수에 할당된 것들이라\
바꿔줘야 할 것이 있습니다.\
공공데이터포털 -> 마이페이지\
여기서 인증키가 (인코딩, 디코딩) 방식이 있습니다.\
url은 인코딩방식, 저희의 경우엔 디코딩 방식을 써야합니다. 이제 디코딩 키를 복사하여\
index.js에서 serviceKey 바꿔줍시다

그 후
```javascript
node index.js
```
입력 후 localhost 들어가서 `F12` 누른 후 아래사진과 같이 순서대로 누르면 리스트가 쫙 나옵니다

![Desktop View](/assets/img/api/naver-map-api-pharmacy/15.png)

<br>
<br>
<br>
<br>

자 이렇게 나오는 이유는 index.html을 보면 `/pharmach_list` 이 url이 제대로 실행되어 콘솔의 data결과물이\
console.log로 찍힌 것을 알 수 있습니다.

<br>
<br>
<br>

![Desktop View](/assets/img/api/naver-map-api-pharmacy/16.png)

<br>
<br>
<br>

여기서 코드를 조금 수정해봅시다
<br>
약국 데이터를 받고난 후 지도가 뜨도록 수정해봅시다

```javascript
                    //지도를 삽입할 HTML 요소 또는 HTML 요소의 id를 지정합니다.
                    var mapDiv = document.getElementById('map'); // 'map'으로 선언해도 동일

                    //옵션 없이 지도 객체를 생성하면 서울 시청을 중심으로 하는 16 레벨의 지도가 생성됩니다.
                    var map = new naver.maps.Map(mapDiv);
```
<br>

이 코드를 ajax 코드 밑의 console.log(data); 아래에 옮겨줍시다\
<br>
그 후 https://navermaps.github.io/maps.js/docs/tutorial-digest.example.html\
이 주소로 와서 '지도 이동하기'를 찾읍시다\
코드를 자세히 보면 위도경도값을 LatLng객체를 생성해 그것을 웹에 설정해주면\
panTo라는 명령으로 이동하고, 그리고 처음지도를 실행할 때 LatLng를 center라는 옵션을 주게되면\
해당 설정된 경도, 위도로 웹이 이동됨.\
그리고 그 곳에 이 코드를 복사하여 mapOptions변수를 만들고 아래에 붙입시다
```javascript
    center: new naver.maps.LatLng(37.5666805, 126.9784147),
    zoom: 9
```
<br>
그 후 아까 자리 옮겼던 2번째 변수의 끝에 mapOptions인자를 하나 더 넣은 것으로 수정함으로써\
옵션에 있는 내용의 위도와 경도 값이 들어감. 그리고 숫자가 37.xxxxx 126.xxxx 인데 그 자리를\
`XY.lat` `XY.lng`로 바꿉시다.

<br>
<br>
<br>

이제 localhost로 가보면 이렇게 나옵니다!

<br>
<br>

![Desktop View](/assets/img/api/naver-map-api-pharmacy/17.png)

<br>

여기까지의 `index.js` `index.html`코드를 보여드릴테니 잘 안된다면 무엇이 틀린건지 비교해보시길 바랍니다.

## index.js

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

        let api = async() => {
            let response = null
            
            try {
            response = await axios.get("http://apis.data.go.kr/B552657/ErmctInsttInfoInqireService/getParmacyListInfoInqire", {
                params: {
                    "serviceKey": "mL6hpE93V2cGEHnZNYbp2kbpZIm2IFyc9rhdh2wIaUseyjghN/lJSV7tSchmbL47mZsX8gNcLVtGpsTxQkstdA==",
                    "Q0":req.query.Q0,
                    "Q1":req.query.Q1,
                    "QT":req.query.QT,
                    "QN":req.query.QN,
                    "ORD":req.query.ORD,
                    "pageNo":req.query.pageNo,
                    "numOfRows":req.query.numOfRows
                }
            })
        }






        catch(e) {
            console.log(e);
        }







            return response;
        }
        api().then((response) =>  {
            res.setHeader("Access-Control-Allow-Origin", "*")
            res.json(response.data.response.body);
        });
});
//  https://apis.data.go.kr/B552657/ErmctInsttInfoInqireService/getParmacyListInfoInqire?serviceKey=mL6hpE93V2cGEHnZNYbp2kbpZIm2IFyc9rhdh2wIaUseyjghN%2FlJSV7tSchmbL47mZsX8gNcLVtGpsTxQkstdA%3D%3D&Q0=%EC%84%9C%EC%9A%B8%ED%8A%B9%EB%B3%84%EC%8B%9C&Q1=%EA%B0%95%EB%82%A8%EA%B5%AC&QT=1&QN=%EC%82%BC%EC%84%B1%EC%95%BD%EA%B5%AD&ORD=NAME&pageNo=1&numOfRows=10
```

<br>
<br>
<br>
<br>
<br>
<br>
<br>


## index.html

```html
<html>
    <head>     
        <script type="text/javascript" src="https://openapi.map.naver.com/openapi/v3/maps.js?ncpClientId=아이디 &amp;submodules=geocoder"></script>
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
                $(document).ready(async function(){
                    let XY = await getLocation();

                    //alert("위도" + XY.lat);
                    //alert("경도" + XY.lng);

                    $.ajax({
                        url: "/pharmach_list",
                        type: "GET",   // GET을 통해 밑에 주석처리한 api url 부분 ?뒤부터 눈에 보이게끔 값들을 하나하나 입력해줌
                        cache: false,  //cache는 쓰지 않을거라 false
                        dataType: "json",    //dataType은 json으로 받겠다
                        data: {"Q0": "서울특별시", "Q1": "강남구", "QT": "", "QN": "", "ORD": "", "pageNo": "1", "numOfRows": "1000"},   
                        success: function(data) {
                            console.log(data);

                            var mapOptions = {
                                center: new naver.maps.LatLng(XY.lat, XY.lng),
                                zoom: 9
                            }

                                                        //지도를 삽입할 HTML 요소 또는 HTML 요소의 id를 지정합니다.
                            var mapDiv = document.getElementById('map'); // 'map'으로 선언해도 동일

                            //옵션 없이 지도 객체를 생성하면 서울 시청을 중심으로 하는 16 레벨의 지도가 생성됩니다.
                            var map = new naver.maps.Map(mapDiv, mapOptions);

                        },
                        error: function(request, status, error) {

                        }

                    });
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
</html>
```
자, `index.html`의 zoom: 9 ---> 14로 바꾸어봅시다 그러면 더 가까운 위치로 보이게 될 것입니다.\
근데 아직 제일 중요한 제 위치 근처로 약국데이터들을 받아오지 못했습니다.\
현재 위도와 경도만 받아왔는데 이제 위도와 경도의 주소를 가지고오는 역할의 함수가 필요합니다.

<br>

https://navermaps.github.io/maps.js/docs/tutorial-digest.example.html --> 아래쪽에 보면 `주소와 좌표 검색하기 API` 클릭\
여길 보면 reverseGeocode api가 있는데 이를 사용시 위도, 경도 -> 주소로 변환 
<br>
  
이제 `index.html`로 돌아가서 ajax 위에 적어줍시다 naver reverseGeocode 불러주려면 submodule이 geocoder여야 가능하다\
참고로 reverseGeocode는 비동기형이다 그래서 await를 적어주고 인자를 적기위해 대괄호 추가하겠습니다.\
LatLng 객체를 만들어 주기 위해 함수를 넣고 첫번째 인자와 두번째 인자를 넣어줍시다\
그리고 두번째 인자를 function형태로 받습니다 그리고 받을 때 status라는 변수와 response라는 변수로 받겠습니다.\
그리고 response.result에는 결과값을 받겠습니다. 그리고 그 변수를 result로 선언해줍니다, 그리고 그 밑에 result.items라고 하면\
items 변수가 담깁니다. items변수도 선언하겠습니다 그리고 시도와 시구군 값이 제대로 있는지 콘솔로 찍어봅시다.\
완성된 코드는 아래와 같습니다.

```javascript
                $(document).ready(async function(){
                    let XY = await getLocation();

                    //alert("위도" + XY.lat);
                    //alert("경도" + XY.lng);

                    await naver.maps.Service.reverseGeocode({
                        location: new naver.maps.LatLng(XY.lat, XY.lng)
                    }, function(status, response){
                        let result = response.result;
                        let items = result.items;
                        console.log(items[0].addrdetail.sido); // 시도값
                        console.log(items[0].addrdetail.sigugun); // 시구군


                    });

                    $.ajax({
```
여기까지하고 localhost에서 F12 -> 

<br>
<br>

![Desktop View](/assets/img/api/naver-map-api-pharmacy/18.png)

오 시도, 시군에 콘솔로그 해준 내용이 잘 나오군요!
<br>
<br>
<br>
<br>
<br>
<br>
<br>

---
# 2 &nbsp; 공공데이터 포털
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
<br>
<br>



아래 사진을 클릭하면 `제 취미공간`으로 이어집니다 ㅎㅎ 여기에서 메모 및 일상을 기록하는데 놀러오실 분들은 언제나 환영합니다!

<br>

# 링크로 이동하려면 사진을 클릭

[![어서오셔 ㅎ](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQk-zPB4TCuWRNJVIF0aWgniDPNJgUTdXmILg&usqp=CAU)](https://discord.com/channels/976352361142452234/976352361142452239)


---
# 참고
---
'소스놀이터' &nbsp;&nbsp;&nbsp;&nbsp;   [Node.Js로 네이버 약국 지도 만들기 #2 (express & axios 모듈, 비동기 문제 해결)](https://www.youtube.com/watch?v=FKQxItpstIk&t=1245s) &nbsp;&nbsp;&nbsp;&nbsp; 30:12 ~

 <br>

 '소스놀이터' &nbsp;&nbsp;&nbsp;&nbsp;   [Node.Js로 네이버 약국 지도 만들기 #3 (LAST) (data.go.kr 오픈 API)](https://www.youtube.com/watch?v=XC8vBN_WhYs) &nbsp;&nbsp;&nbsp;&nbsp;