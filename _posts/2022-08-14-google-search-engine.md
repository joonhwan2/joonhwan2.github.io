---
title:  "깃허브 블로그 구글검색 노출시키기"
excerpt: "4"

categories:
  - github blog 
tags:
  - [blog, jekyll, github, git]

toc: true
toc_sticky: true
 
date: 2022-08-14
---
<br>

---
google search console 들어가기
---
※`eona1301`님의 블로그를 참고하여 작성합니다.
<br>
<br>

[google search console 바로가기](https://search.google.com/search-console/about)

<br>

이 곳을 들어간 후 <u>시작<u/>을 누릅시다.

<br>
그러면 이 화면으로 바뀝니다.
  
<br>
<br>

![Desktop View](/assets/img/2022-08-14k/1.PNG)  
<br>
  
이제 여기 주소를 입력해주시면 됩니다. 저의 경우는 https://joonhwan2.github.io
<br>
그리고 `계속`을 누르면
<br>
<br>
  
![Desktop View](/assets/img/2022-08-14k/2.PNG)
<br>
  
저거 클릭해서 깃허브 블로그 폴더와 같은 경로에 저장합시다
  
<br>
<br>
  
![Desktop View](/assets/img/2022-08-14k/3.PNG)
  
<br>
<br>
<br>
  
이제 `sitemap.xml`을 열면
<br>
<br>
<br>
  
```
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.sitemaps.org/schemas/sitemap/0.9 http://www.sitemaps.org/schemas/sitemap/0.9/sitemap.xsd" xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
<url>
<loc>http://localhost:4000/posts/text-and-typography</loc>
<lastmod>2019-08-08T12:33:00+09:00</lastmod>
</url>
  
<url>
<loc>http://localhost:4000/posts/write-a-new-post</loc>
<lastmod>2019-08-08T15:10:00+09:00</lastmod>
</url>
  
<url>
<loc>http://localhost:4000/posts/getting-started</loc>
<lastmod>2019-08-09T21:55:00+09:00</lastmod>  

~~~~~~
~~~  
```
<br>
<br>

이런식으로 나오는데 https://localhost:4000 이라고 적힌 부분을 모두 교체해줍니다. 한 20개 조금 넘습니다.\
저의 경우는 https://joonhwan2.github.io 로 하였습니다.

<br>
<br>
<br>

이제 `robot.txt`를 열읍시다.\
(robot.txt 역할은 사이트 페이지를 검색 결과에 포함시킬지 여부를 검색 엔진에 알립니다.)  
```
User-agent: *
allow: /

Sitemap: https://joonhwan2.github.io/sitemap.xml
```
내용이 disallow 라고 되있는 분들도 계실텐데 저처럼 바꿔주시면 됩니다.
밑에 주소적고 꼭 `/sitemap.xml`도 붙여야합니다 ㅎ

<br>
다 되셨다면 이제 git add~push 를 통해 깃허브에 업로드 해주시면 됩니다.  
<br>
  
이제 google search console 홈페이지로 돌아와서 왼쪽에 보면 sitemaps 라고 있는데
  
<br> 
<br>
<br>  
  
![Desktop View](/assets/img/2022-08-14k/4.PNG)
  
<br>
<br>
<br>
  
이 곳에서 `sitemap.xml` 을 입력합니다. 그리고 제출을 합시다

<br>  
<br>

![Desktop View](/assets/img/2022-08-14k/5.PNG)
  
<br>
  
이제 이런 문구가 뜨면 성공입니다. 구글에 반영되는건 짧으면 1~2일 길면 몇일 더 걸린다하니\
일단 테스트는 후에 해볼 예정입니다 ㅎ 
  
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
 'eona1301' &nbsp;&nbsp;&nbsp;&nbsp;   [[Github Blog] 검색창 노출시키기](https://velog.io/@eona1301/Github-Blog-%EA%B2%80%EC%83%89%EC%B0%BD-%EB%85%B8%EC%B6%9C%EC%8B%9C%ED%82%A4%EA%B8%B0)