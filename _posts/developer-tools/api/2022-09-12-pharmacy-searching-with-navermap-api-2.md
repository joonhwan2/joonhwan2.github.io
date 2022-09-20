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

<br>
<br>
<br>
<br>

오 시도, 시군에 콘솔로그 해준 내용이 잘 나오군요!

<br>
<br>
<br>

공공데이터포털 -> 마이페이지, 잠깐 사진을 봅시다, 공백을 허용하지않군요.

<br>
<br>
<br>
<br>

![Desktop View](/assets/img/api/naver-map-api-pharmacy/19.png)

<br>
<br>
<br>

이제 `경상북도 xx시`, `경상남도 xx시`로 나누어지는 경우를 생각해봅시다. 공백을 제거하기 위해\
index.html의 시도, 시구군 함수 수정합시다, 일단 2개 주석처리

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
                        //console.log(items[0].addrdetail.sido); // 시도값
                        //console.log(items[0].addrdetail.sigugun); // 시구군
                        let sido_arr = items[0].addrdetail.sido.split(" ");
                        let gubun_arr = items[0].addrdetail.sigugun.split(" ");

                        let sido = "";
                        let gubun = "";
                        if(sido_arr.length ==1) {
                            sido = sido_arr[0];
                            gugun = gubun_arr[0];
                        }
                        else if(sido_arr.length > 1) {
                            sido = sido_arr[0];
                            gugun = gubun_arr[1];
                        }
                        console.log(sido);
                        console.log(gugun);
                    });
```

이렇게되면 2개의 요소를 가진 배열이 되어, 한줄에 있던 `경상남도 xx시`\
-->\
`경상남도`\
`xx시`

<br>

그리고 새롭게 편집될 변수도 미리 정의해줍시다.\
만약 시도가 1이라는 것은 `서울특별시` 처럼 한큐에 끝날때 시도와 구군의 첫번째 요소가됨,\
만약 그렇지 않고 시도 어레이가 1보다 클때, 즉 공백이 하나 있을때 ex) `경상남도 xx시`
그때 시도 어래이는 0, 그러나 구군 어레이는 첫번째 인자인 1을 적어주게 되면\
시도와 구군이 공백이 제거된 상태로 만들어짐\
이제 값이 제대로 잘 적혀있는지 콘솔로 시도, 구군을 확인해봅시다

<br>
<br>

![Desktop View](/assets/img/api/naver-map-api-pharmacy/20.png)

<br>
<br>

오 잘 나오군요!\
이제 index.html로 가서 Q0에는 sido변수를, Q1에는 gugun변수를 넣어줍시다\
```javascript
                    $.ajax({
                        url: "/pharmach_list",
                        type: "GET",   // GET을 통해 밑에 주석처리한 api url 부분 ?뒤부터 눈에 보이게끔 값들을 하나하나 입력해줌
                        cache: false,  //cache는 쓰지 않을거라 false
                        dataType: "json",    //dataType은 json으로 받겠다
                        data: {"Q0": sido, "Q1": gugun, "QT": "", "QN": "", "ORD": "", "pageNo": "1", "numOfRows": "1000"},
``` 
그리고 이부분 역시 비동기이기 때문에 ajax구문 위치를 옮깁시다.
```javascript
                        console.log(sido);
                        console.log(gugun);
```

<br>
<br>
<br>

이곳 아래에 배치합시다, 여기까지의 index.html 코드는 이렇게 됩니다

<br>
<br>
<br>

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
                        //console.log(items[0].addrdetail.sido); // 시도값
                        //console.log(items[0].addrdetail.sigugun); // 시구군
                        let sido_arr = items[0].addrdetail.sido.split(" ");
                        let gubun_arr = items[0].addrdetail.sigugun.split(" ");

                        let sido = "";
                        let gubun = "";
                        if(sido_arr.length ==1) {
                            sido = sido_arr[0];
                            gugun = gubun_arr[0];
                        }
                        else if(sido_arr.length > 1) {
                            sido = sido_arr[0];
                            gugun = gubun_arr[1];
                        }
                        console.log(sido);
                        console.log(gugun);

                        $.ajax({
                            url: "/pharmach_list",
                            type: "GET",   // GET을 통해 밑에 주석처리한 api url 부분 ?뒤부터 눈에 보이게끔 값들을 하나하나 입력해줌
                            cache: false,  //cache는 쓰지 않을거라 false
                            dataType: "json",    //dataType은 json으로 받겠다
                            data: {"Q0": sido, "Q1": gugun, "QT": "", "QN": "", "ORD": "", "pageNo": "1", "numOfRows": "1000"},   
                            success: function(data) {
                                console.log(data);
    
                                var mapOptions = {
                                    center: new naver.maps.LatLng(XY.lat, XY.lng),
                                    zoom: 14
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
```

<br>
<br>
<br>

그리고 강력 새로고침을 해보면 빠밤

<br>
<br>
<br>

![Desktop View](/assets/img/api/naver-map-api-pharmacy/21.png)

<br>
<br>
<br>

## 마지막으로 약국이 지도에 보이게 하는 작업만 남았습니다.\

https://navermaps.github.io/maps.js/docs/tutorial-digest.example.html\
--> 중간에 `정보창` 있는 곳에 `정보 창 옵션 사용하기` 클릭\
푸른 아이콘 클릭시 그곳의 정보를 보여주는 역할 수행합니다.

<br>

잠깐 localhost에서 F12 -> console -> items 클릭\
그러면 아래와 같은 사진들이 보일 것입니다
<br>

![Desktop View](/assets/img/api/naver-map-api-pharmacy/22.png)

<br>
<br>

총 81개의 각 배열들이 있고 제가 있는 곳은 아마 큰 번화가가 아니라 약국갯수가 이런데 다른 번화가가면 몇백개 나올 수도 있습니다.\
복수형으로 만들어주어야하기 때문에 하나하나 엑세스하기 위해 반복문인 forEach 받읍시다, 첫번째인자로 it, 두번째 인자로 index\
이 it변수에는 item 배열에 있는 81개가 각각 it변수에 담깁니다.\
function 영역은 81개 전체 배열갯수만큼 반복해서 실행되는 영역

<br>

```javascript
var map =
```
이 곳 아래에 적어줍시다

이제 각 dutyName dutyAddr dutyTel1 dutyTime으로 변수 받읍시다\
그리고 https://navermaps.github.io/maps.js/docs/tutorial-infowindow-options.example.html\
이곳에서 marker부터 끝까지 복사하여 그 아래에 붙여넣기, 그리고 marker는 let 변수로 선언해줍시다.\
이제 index.html을 보면 마커가 표시될 위치(position)이 있는데 position이 cityhall이라는 변수로 되있음\
예제를 보면 cityhall이 있는 내용이 있습니다 그 부분 복사하여 dutyTime 아래에 넣읍시다. 근데 우리는 약국을 하니까\
let cityhall --> let pharmacy_location 바꿔줍시다 그리고 position도 바꿔주고 아래에\

```javascript
var contentString = [
---
---
---
]
```

--->

```javascript
                                    var contentString = [
                                            '<div class="iw_inner">',
                                            '   <h3>'+dutyName+'</h3>',
                                            '   <p>'+dutyAddr+'<br />', 
                                            '       '+dutyTel1+'<br />',
                                            '       '+dutyTime,
                                            '   </p>',
                                            '</div>'
                                        ].join('');
```
참 이곳에 img src 한줄 지웁시다 우린 사진이 필요없으니

<br>
<br>
<br>

그리고 localhost에서 F12를 누릅시다

<br>
<br>

![Desktop View](/assets/img/api/naver-map-api-pharmacy/23.png)

<br>
<br>

사진에 보면 행으로 번호가 0~16으로 있는데 이중에 아무거나 누르면 자세한 정보가 나오는데 그 안에 이 2개가 있을겁니다
`wgs84Lat` `wgs84Lon` 그거를 37.xxx  126.xxx 대신에 앞에 it.을 붙여 합쳐서 각각 고칩시다

<br>
<br>
<br>

여기까지의 index.html 코드는 이렇습니다.

```html
<html>
    <head>     
        <script type="text/javascript" src="https://openapi.map.naver.com/openapi/v3/maps.js?ncpClientId=b3cui5r9yl&amp;submodules=geocoder"></script>
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

                    await naver.maps.Service.reverseGeocode({
                        location: new naver.maps.LatLng(XY.lat, XY.lng)
                    }, function(status, response){
                        let result = response.result;
                        let items = result.items;
                        //console.log(items[0].addrdetail.sido); // 시도값
                        //console.log(items[0].addrdetail.sigugun); // 시구군
                        let sido_arr = items[0].addrdetail.sido.split(" ");
                        let gubun_arr = items[0].addrdetail.sigugun.split(" ");

                        let sido = "";
                        let gubun = "";
                        if(sido_arr.length ==1) {
                            sido = sido_arr[0];
                            gugun = gubun_arr[0];
                        }
                        else if(sido_arr.length > 1) {
                            sido = sido_arr[0];
                            gugun = gubun_arr[1];
                        }
                        console.log(sido);
                        console.log(gugun);

                        $.ajax({
                            url: "/pharmach_list",
                            type: "GET",   // GET을 통해 밑에 주석처리한 api url 부분 ?뒤부터 눈에 보이게끔 값들을 하나하나 입력해줌
                            cache: false,  //cache는 쓰지 않을거라 false
                            dataType: "json",    //dataType은 json으로 받겠다
                            data: {"Q0": sido, "Q1": gugun, "QT": "", "QN": "", "ORD": "", "pageNo": "1", "numOfRows": "1000"},   
                            success: function(data) {
                                console.log(data);
    
                                var mapOptions = {
                                    center: new naver.maps.LatLng(XY.lat, XY.lng),
                                    zoom: 14
                                }
    
                                                            //지도를 삽입할 HTML 요소 또는 HTML 요소의 id를 지정합니다.
                                var mapDiv = document.getElementById('map'); // 'map'으로 선언해도 동일
    
                                //옵션 없이 지도 객체를 생성하면 서울 시청을 중심으로 하는 16 레벨의 지도가 생성됩니다.
                                var map = new naver.maps.Map(mapDiv, mapOptions);
                                
                                data.items.item.forEach(function(it, index){
                                    let dutyName = it.dutyName; //약국명
                                    let dutyAddr = it.dutyAddr; //주소
                                    let dutyTel1 = it.dutyTel1; //전번

                                    let dutyTime = ""; //업무시간

                                    let pharmacy_location = new naver.maps.LatLng(it.wgs84Lat, it.wgs84Lon)


                                    
                                    let marker = new naver.maps.Marker({
                                        map: map,
                                        position: pharmacy_location
                                    });
                                
                                    var contentString = [
                                            '<div class="iw_inner">',
                                            '   <h3>'+dutyName+'</h3>',
                                            '   <p>'+dutyAddr+'<br />', 
                                            '       '+dutyTel1+'<br />',
                                            '       '+dutyTime,
                                            '   </p>',
                                            '</div>'
                                        ].join('');
                                        
                                        var infowindow = new naver.maps.InfoWindow({
                                            content: contentString,
                                            maxWidth: 140,
                                            backgroundColor: "#eee",
                                            borderColor: "#2db400",
                                            borderWidth: 5,
                                            anchorSize: new naver.maps.Size(30, 30),
                                            anchorSkew: true,
                                            anchorColor: "#eee",
                                            pixelOffset: new naver.maps.Point(20, -20)
                                        });
                                        
                                naver.maps.Event.addListener(marker, "click", function(e) {
                                    if (infowindow.getMap()) {
                                        infowindow.close();
                                    } else {
                                        infowindow.open(map, marker);
                                    }
                                });






                                }); 

                            },
                            error: function(request, status, error) {
    
                            }
    
                        });
                        
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

그리고 localhost에서 강력 새로고침하여 확인해보면  

<br>

![Desktop View](/assets/img/api/naver-map-api-pharmacy/24.png)

<br>

약국은 잘 뜨는데 보이는 폭이 좁아서 글자가 좀 잘리는 것 같군요.\
폭 maxWidth 140 -> 440으로 고칩시다 그리고 요일별 영업시간 번호 등 추가해줍시다.

<br>

오 이제 잘나옵니다! 

<br>
<br>
<br>

![Desktop View](/assets/img/api/naver-map-api-pharmacy/25.png)

<br>
<br>
<br>
<br>
<br>
<br>
<br>

여기까지의 `index.html`와 `index.js` 코드를 공개할테니 잘 안되시는 분들은 비교해보시고 수정해보세요ㅎ

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

                    await naver.maps.Service.reverseGeocode({
                        location: new naver.maps.LatLng(XY.lat, XY.lng)
                    }, function(status, response){
                        let result = response.result;
                        let items = result.items;
                        //console.log(items[0].addrdetail.sido); // 시도값
                        //console.log(items[0].addrdetail.sigugun); // 시구군
                        let sido_arr = items[0].addrdetail.sido.split(" ");
                        let gubun_arr = items[0].addrdetail.sigugun.split(" ");

                        let sido = "";
                        let gubun = "";
                        if(sido_arr.length ==1) {
                            sido = sido_arr[0];
                            gugun = gubun_arr[0];
                        }
                        else if(sido_arr.length > 1) {
                            sido = sido_arr[0];
                            gugun = gubun_arr[1];
                        }
                        console.log(sido);
                        console.log(gugun);

                        $.ajax({
                            url: "/pharmach_list",
                            type: "GET",   // GET을 통해 밑에 주석처리한 api url 부분 ?뒤부터 눈에 보이게끔 값들을 하나하나 입력해줌
                            cache: false,  //cache는 쓰지 않을거라 false
                            dataType: "json",    //dataType은 json으로 받겠다
                            data: {"Q0": sido, "Q1": gugun, "QT": "", "QN": "", "ORD": "", "pageNo": "1", "numOfRows": "1000"},   
                            success: function(data) {
                                console.log(data);
    
                                var mapOptions = {
                                    center: new naver.maps.LatLng(XY.lat, XY.lng),
                                    zoom: 20
                                }
    
                                                            //지도를 삽입할 HTML 요소 또는 HTML 요소의 id를 지정합니다.
                                var mapDiv = document.getElementById('map'); // 'map'으로 선언해도 동일
    
                                //옵션 없이 지도 객체를 생성하면 서울 시청을 중심으로 하는 16 레벨의 지도가 생성됩니다.
                                var map = new naver.maps.Map(mapDiv, mapOptions);
                                
                                data.items.item.forEach(function(it, index){
                                    let dutyName = it.dutyName; //약국명
                                    let dutyAddr = it.dutyAddr; //주소
                                    let dutyTel1 = it.dutyTel1; //전번

                                    let dutyTime = ""; //업무시간

                                    
                                    if(it.dutyTime1s && it.dutyTime1c) {
                                        dutyTime += "월요일: " + it.dutyTime1s + " ~ " + it.dutyTime1c + "<br>";
                                    }
    
                                    if(it.dutyTime2s && it.dutyTime2c) {
                                        dutyTime += "화요일: " + it.dutyTime2s + " ~ " + it.dutyTime2c + "<br>";
                                    }
    
                                    if(it.dutyTime3s && it.dutyTime3c) {
                                        dutyTime += "수요일: " + it.dutyTime3s + " ~ " + it.dutyTime3c + "<br>";
                                    }
    
                                    if(it.dutyTime4s && it.dutyTime4c) {
                                        dutyTime += "목요일: " + it.dutyTime4s + " ~ " + it.dutyTime4c + "<br>";
                                    }
    
                                    if(it.dutyTime5s && it.dutyTime5c) {
                                        dutyTime += "금요일: " + it.dutyTime5s + " ~ " + it.dutyTime5c + "<br>";
                                    }
    
                                    if(it.dutyTime6s && it.dutyTime6c) {
                                        dutyTime += "토요일: " + it.dutyTime6s + " ~ " + it.dutyTime6c + "<br>";
                                    }
    
                                    if(it.dutyTime7s && it.dutyTime7c) {
                                        dutyTime += "일요일: " + it.dutyTime7s + " ~ " + it.dutyTime7c + "<br>";
                                    }
    
                                    if(it.dutyTime8s && it.dutyTime8c) {
                                        dutyTime += "공휴일"; + it.dutyTime8s + " ~ " + it.dutyTime8c + "<br>";
                                    }
                                    


                                    let pharmacy_location = new naver.maps.LatLng(it.wgs84Lat, it.wgs84Lon)


                                    
                                    let marker = new naver.maps.Marker({
                                        map: map,
                                        position: pharmacy_location
                                    });
                                
                                    var contentString = [
                                            '<div class="iw_inner">',
                                            '   <h3>'+dutyName+'</h3>',
                                            '   <p>'+dutyAddr+'<br />', 
                                            '       '+dutyTel1+'<br />',
                                            '       '+dutyTime,
                                            '   </p>',
                                            '</div>'
                                        ].join('');
                                        
                                        var infowindow = new naver.maps.InfoWindow({
                                            content: contentString,
                                            maxWidth: 440,
                                            backgroundColor: "#eee",
                                            borderColor: "#2db400",
                                            borderWidth: 5,
                                            anchorSize: new naver.maps.Size(30, 30),
                                            anchorSkew: true,
                                            anchorColor: "#eee",
                                            pixelOffset: new naver.maps.Point(20, -20)
                                        });
                                        
                                naver.maps.Event.addListener(marker, "click", function(e) {
                                    if (infowindow.getMap()) {
                                        infowindow.close();
                                    } else {
                                        infowindow.open(map, marker);
                                    }
                                });






                                }); 

                            },
                            error: function(request, status, error) {
    
                            }
    
                        });
                        
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

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

## index.js

<br>
<br>

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

```

<br>
<br>
<br>
<br>
<br>

2022 09 19일 갑자기 내 위치를 나타내는 커스텀 마커가 추가하고싶어졌다. 그리하여 코드를 변형시켰다\
`index.html` 로 갑시다.

<br>
<br>

```javascript
var mapOptions = {
  center: new naver.maps.LatLng(XY.lat, XY.lng),
zoom: 14,

}
var map = new naver.maps.Map(mapDiv, mapOptions);
```

<br>

이문장 바로 아래에 이 문장들을 추가합시다

```javascript
                                var position = new naver.maps.LatLng(XY.lat, XY.lng);

                                var map = new naver.maps.Map('map', {
                                    center: position,
                                    zoom: 14
                                });

                                var markerOptions = {
                                    position: position,
                                    map: map,
                                    icon: {
                                        content: [
                                        '<img src="/img/sugar.jpg" style="position:absolute; top:2px; left:2px; width:32px; height:32px; object-fit:cover; object-position:center center; border-radius:50%; z-index:6">',
                                        '<img src="/map-pin-afterUser.svg" style="position;absolute; top:0; left:0; z-index:5">',
                                        ].join(''),

                                        anchor: new naver.maps.Point(25, 26)
                                    }
                                };
                                var marker = new naver.maps.Marker(markerOptions);                            




                            var circle = new naver.maps.Circle({
                                map: map,
                                center: new naver.maps.LatLng(XY.lat, XY.lng),
                                radius: 50,
                                fillColor: 'red',
                                fillOpacity: 0.2
                            });
```

<br>

그 후 저의 경우에는 public 폴더를 만들어 그안에 img 폴더, 그안에 `sugar.jpg`를 저장하였습니다.\
자 이상태로 바로 node index.js 실행하면 오류나서 커스텀 마커 안나옵니다

<br>

무슨 말이냐면 `index.js`에 코드에 static으로 img 디렉터리를 지정하거나\
리소스에 접근했을때 보내주는 코드가 없기 때문입니다.\
이제 `index.js`로 가서 문장 하나 추가해주고 포트번호 80 -> 5000 변경해줍시다

```javascript
app.use(express.static('public'));
```

<br>
그러면 사진처럼 빰!

![Desktop View](/assets/img/api/naver-map-api-pharmacy/26.png)

<br>
<br>
<br>
<br>
<br>
<br>
<br>

자 여기까지의 파일트리와 `index.js`와 `index.html` 코드는 이렇습니다.

<br>
<br>
<br>

![Desktop View](/assets/img/api/naver-map-api-pharmacy/27.png)

<br>
<br>
<br>
<br>

### index.js

<br>

```javascript
let express = require("express");
// express모듈을 선언   +   어떤 모듈을 쓸건지 require로 지정   =    이제 이 구문을 통해 express모듈이 사용 가능한 상태가 됨

let axios = require("axios");
// axios 모듈을 사용하겠다

let app = express ();
// app 이라는 변수 하나를 더 선언하여 express객체를 할당하자     이제 이 구문을 통해 app이라는 변수는 express 모듈을 가르키게 됨

let port = process.env.PORT || 5000;


app.use(express.static('public'));
// 커스텀 마커를 사용하기 위해 public 폴더 안 파일과 localhost:5000 연동되게 해줌

app.use(express.static("public_html"));
// express의 use 메소드를 선언하고 express.static이라고 괄호 사이에 입력한 후  public_html 로 지정하겠다
// 이제 public_html 폴더 아래에 있는 모든 파일들은 app.use 즉 express 모듈의 웹서버가 구동되게함

app.listen(port,function(){
    console.log("HTML 서버 시작됨")
})
// express 서비스가 작동될 포트 지정 보통 80번 많이씀 그리고 포트 열렸는지 확인해주기위해 콘솔을 적음
// app.listen에 port를 사용해주고 대신 위에 포트 80변수를 선언해주자 이건 사용되기 앞서 그 이전에 설정되야함
// 이제 위로 가서 포트 80 변수를 선언해주자  process.env.PORT는 환경설정 내용이다



//현재 app.use를 사용해 웹서버를 열은 상황이다 근데
//한페이지만 이름을 pharmach_list따로 열어보겠다 
//이 경우는 app.get으로 가능하다
//그리고 여기 접속했을때 어떤 데이터를 보낼지 결정가능

/////////

////////


app.get ("/pharmach_list", (req, res) =>  {
        let api = async() => {
            let response = null;

            try {
            response = await axios.get("http://apis.data.go.kr/B552657/ErmctInsttInfoInqireService/getParmacyListInfoInqire", {
                params: {
                        "serviceKey": "mL6hpE93V2cGEHnZNYbp2kbpZIm2IFyc9rhdh2wIaUseyjghN/lJSV7tSchmbL47mZsX8gNcLVtGpsTxQkstdA==",
                        "Q0": req.query.Q0,
                        "Q1": req.query.Q1,
                        "QT": req.query.QT,
                        "QN": req.query.QN,
                        "ORD": req.query.ORD,
                        "pageNo": req.query.pageNo,
                        "numOfRows": req.query.numOfRows    //  1000개의 데이터를 1장에 한번에 받겠다 
                    }
                })
            }








            catch(e) {
                console.log(e);
            }






            
            return response;
        }


        api().then((response) => {
            res.setHeader("Access-Control-Allow-Origin", "*");    // * = all,   access control 근원(origin)으로 가능한 것들은 전부 허용해줘라    이렇게 세팅시 cors 로 인한 문제 해결 가능
            res.json(response.data.response.body); // data.response.body가 위에 보이는 약국데이터다, 이를 pharmach_list 경로로 접근하는 컴퓨터에게 데이터 제공
        });  
});
```

<br>
<br>
<br>
<br>
<br>
<br>

### index.html

<br>

```html
<html>
    <head>     <!-- script type는 자바스크립트, src를 넣어 소스는 다른 곳에 있다고 지정 ex) 네이버 open api에 있는 정보-->
        <script type="text/javascript" src="https://openapi.map.naver.com/openapi/v3/maps.js?ncpClientId=xeulgqnc95&amp;submodules=geocoder"></script>
        <script src="https://code.jquery.com/jquery-3.6.1.min.js" integrity="sha256-o88AwQnZB+VDvE9tvIXrMQaPlFFSUTR+nldQm1LuPXQ=" crossorigin="anonymous"></script>
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
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

                // reverseGeocode는 비동기형이라 await 추가
                await naver.maps.Service.reverseGeocode({
                    location: new naver.maps.LatLng(XY.lat, XY.lng)
                },function(status, response){
                    let result = response.result;
                    let items = result.items;
                    //console.log(items[0].addrdetail.sido);
                    //console.log(items[0].addrdetail.sigugun);
                    let sido_arr = items[0].addrdetail.sido.split(" ");
                    let gubun_arr = items[0].addrdetail.sigugun.split(" ");

                    let sido = "";
                    let gubun = "";
                    if(sido_arr.length ==1) {
                        sido = sido_arr[0];
                        gugun = gubun_arr[0]
                    }


                    else if(sido_arr.length > 1) {
                        sido = sido_arr[0];
                        gugun = sido_arr[1]
                    }
                    console.log(sido);
                    console.log(gugun);


                    $.ajax({
                        url: "/pharmach_list",
                        type: "GET",   // GET을 통해 밑에 주석처리한 api url 부분 ?뒤부터 눈에 보이게끔 값들을 하나하나 입력해줌
                        cache: false,  //cache는 쓰지 않을거라 false
                        dataType: "json",    //dataType은 json으로 받겠다
                        data: {"Q0": sido, "Q1": gugun, "QT": "", "QN": "", "ORD": "", "pageNo": "1", "numOfRows": "1000"},   
                        success: function(data) {
                            console.log(data);
    
    
                            //지도를 삽입할 HTML 요소 또는 HTML 요소의 id를 지정합니다.
                            var mapDiv = document.getElementById('map'); // 'map'으로 선언해도 동일
    
                            //옵션 없이 지도 객체를 생성하면 서울 시청을 중심으로 하는 16 레벨의 지도가 생성됩니다.
                            var mapOptions = {
                                center: new naver.maps.LatLng(XY.lat, XY.lng),
                                zoom: 14
                            }
                            var map = new naver.maps.Map(mapDiv, mapOptions);

//////////////////////////////////////////////////////////////////////////////////////////
                                var position = new naver.maps.LatLng(XY.lat, XY.lng);

                                var map = new naver.maps.Map('map', {
                                    center: position,
                                    zoom: 14
                                });

                                var markerOptions = {
                                    position: position,
                                    map: map,
                                    icon: {
                                        content: [
                                        '<img src="/img/sugar.jpg" style="position:absolute; top:2px; left:2px; width:32px; height:32px; object-fit:cover; object-position:center center; border-radius:50%; z-index:6">',
                                        '<img src="/map-pin-afterUser.svg" style="position;absolute; top:0; left:0; z-index:5">',
                                        ].join(''),

                                        anchor: new naver.maps.Point(25, 26)
                                    }
                                };
                                var marker = new naver.maps.Marker(markerOptions);                            




                            var circle = new naver.maps.Circle({
                                map: map,
                                center: new naver.maps.LatLng(XY.lat, XY.lng),
                                radius: 50,
                                fillColor: 'red',
                                fillOpacity: 0.2
                            });
                            
 //////////////////////////////////////////////////////////////////////////////
                            
                            data.items.item.forEach(function(it, index){
                                let dutyName = it.dutyName;
                                let dutyAddr = it.dutyAddr;
                                let dutyTel1 = it.dutyTel1;

                                let dutyTime = "";
                                if(it.dutyTime1s && it.dutyTime1c) {
                                    dutyTime += "월요일: " + it.dutyTime1s + " ~ " + it.dutyTime1c + "<br>";
                                }

                                if(it.dutyTime2s && it.dutyTime2c) {
                                    dutyTime += "화요일: " + it.dutyTime2s + " ~ " + it.dutyTime2c + "<br>";
                                }

                                if(it.dutyTime3s && it.dutyTime3c) {
                                    dutyTime += "수요일: " + it.dutyTime3s + " ~ " + it.dutyTime3c + "<br>";
                                }

                                if(it.dutyTime4s && it.dutyTime4c) {
                                    dutyTime += "목요일: " + it.dutyTime4s + " ~ " + it.dutyTime4c + "<br>";
                                }

                                if(it.dutyTime5s && it.dutyTime5c) {
                                    dutyTime += "금요일: " + it.dutyTime5s + " ~ " + it.dutyTime5c + "<br>";
                                }

                                if(it.dutyTime6s && it.dutyTime6c) {
                                    dutyTime += "토요일: " + it.dutyTime6s + " ~ " + it.dutyTime6c + "<br>";
                                }

                                if(it.dutyTime7s && it.dutyTime7c) {
                                    dutyTime += "일요일: " + it.dutyTime7s + " ~ " + it.dutyTime7c + "<br>";
                                }

                                if(it.dutyTime8s && it.dutyTime8c) {
                                    dutyTime += "공휴일"; + it.dutyTime8s + " ~ " + it.dutyTime8c + "<br>";
                                }


                                let pharmacy_location = new naver.maps.LatLng(it.wgs84Lat, it.wgs84Lon)

                                    let marker = new naver.maps.Marker({
                                        map: map,
                                        position: pharmacy_location
                                    });
                                
                                    var contentString = [
                                            '<div class="iw_inner">',
                                            '   <h3>'+dutyName+'</h3>',
                                            '   <p>'+dutyAddr+'<br />', 
                                            '       '+dutyTel1+'<br />',
                                            '       '+dutyTime,
                                            '   </p>',
                                            '</div>'
                                        ].join('');
                                    
                                    var infowindow = new naver.maps.InfoWindow({
                                        content: contentString,
                                        maxWidth: 440,
                                        backgroundColor: "#eee",
                                        borderColor: "#2db400",
                                        borderWidth: 5,
                                        anchorSize: new naver.maps.Size(30, 30),
                                        anchorSkew: true,
                                        anchorColor: "#eee",
                                        pixelOffset: new naver.maps.Point(20, -20)
                                    });
                                    
                                    naver.maps.Event.addListener(marker, "click", function(e) {
                                        if (infowindow.getMap()) {
                                            infowindow.close();
                                        } else {
                                            infowindow.open(map, marker);
                                        }
                                    });












                                    
                                    });
    
                        },
                        error: function(request, status, error) {
                            
                        }
                    });


                });


            });

            // geolocation은 gps와 관련된 객체, 이 객체가 존재하면 getLocation 실행함
            // getCurrentPosition 사용시 현재 위치를 알 수 있다, position 이라는 객체를 통해 현 위치 파악
            async function getLocation() {
                let XY = new Object();
                if(navigator.geolocation) {

                    
                    let promise = new Promise((resolve, rejected) => {
                        navigator.geolocation.getCurrentPosition((position) => {
                            resolve(position);
                        });
                    });

                    let position = await promise;

                    //위도 position.coords.latitude 
                    //경도 position.coords.longitude 
                    XY.lat = position.coords.latitude
                    XY.lng = position.coords.longitude
  
                }
                return XY;     
            }        
        </script>    
</html>
```

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

 <br>
 <br>
 <br>
 <br>
 <br>
 <br>

---
# 도와주신 분들
---
`유튜브` - 소스놀이터
