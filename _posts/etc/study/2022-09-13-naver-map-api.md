---
title:  "[네이버지도로 약국찾기 작업하면서 배운 것]"
categories: [etc, study] 
tags: [study]
toc: true
toc_sticky: true
date: 2022-09-13
---

<br>
<br>
<br>
<br>

> # ✏️배운 것들 &nbsp;
> * 동기와 비동기 차이
> * 구문 해석능력
> * 괄호와 대괄호 차이
> * 변수의 의미

<br>
<br>
<br>

## 1 동기

```javascript
function getLocation() {
                let XY = new Object();
                if(navigator.geolocation) {
                    navigator.geolocation.getCurrentPosition(function(position){
                        position.coords.latitude //위도
                        position.coords.longitude //경도
                        XY.lat = position.coords.latitude
                        XY.lng = position.coords.longitude
                        alert("위도" + XY.lat);
                        alert("경도" + XY.lng);
                    });  
                }
                return XY;     
            }
```
if부터 순차적으로 구문이 실행되고 나면 XY에는 위도와 경도가 있고\
그 값을 return 해줘서 반환해줬기 때문에 getLocation 함수가 실행되고\
나면 그 return된 값이 XY에 담겨서 제대로 출력 되겠다라고 생각하게 됨

<br>
<br>
<br>
<br>
<br>

## 2 비동기

```javascript
function getLocation() {
                let XY = new Object();
                if(navigator.geolocation) {
                    navigator.geolocation.getCurrentPosition(function(position){
                        position.coords.latitude //위도
                        position.coords.longitude //경도
                        XY.lat = position.coords.latitude
                        XY.lng = position.coords.longitude
                        alert("위도" + XY.lat);
                        alert("경도" + XY.lng);
                    });  
                }
                return XY;     
            }
```
원래라면 이 if문이 실행이 다되고 나서 그 다음에 return XY로 오는데\
실제로 안그렇고 if문 안으로 들어왔을때 한 navigator.geolocation쯤\
왔을때 또 다른 프로세스가 return으로 와서 전체 if문이 실행되기 전에\
return으로 실행되어져 XY에는 값이 정해지지않은게 return됨 이게\
비동기식이다 

<br>
<br>
<br>
<br>
<br>
<br>
<br>

## 3 구문해석

```javascript
               $.ajax({
                    url: "",
                    type: "",
                    cache: "",
                    dataType: "",
                    data: "",
                    sucess: function(data) {

                    }
                });
```
여기서 위의 dataType와 data는 밑의 succes랑 아무 연관이 없다\
밑의 (data)는 대괄호 안의 항목만 연동이됨


<br>
<br>
<br>
<br>
<br>
<br>
<br>

## 4 괄호와 대괄호 차이

<br>
<br>

인자가 단수일때는 그냥 괄호 하나()  혹은 \
\
복수일때는 대괄호로 ({\
})


<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

## 5 변수의 의미

```javascript
	var mapOptions = {
           		center: new naver.maps.LatLng(37.5666805, 126.9784147),
                            zoom: 9
                        }
```
의미  :   mapOptions라는 변수안에 저 내용들이 담긴 것


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
