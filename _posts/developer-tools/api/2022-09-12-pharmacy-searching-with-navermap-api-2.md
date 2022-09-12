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

 '소스놀이터' &nbsp;&nbsp;&nbsp;&nbsp;   [Node.Js로 네이버 약국 지도 만들기 #3 (LAST) (data.go.kr 오픈 API)](https://www.youtube.com/watch?v=XC8vBN_WhYs) &nbsp;&nbsp;&nbsp;&nbsp; 30:12 ~