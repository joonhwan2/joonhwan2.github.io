---
title:  "[git] git pull fetch 정리"
categories: [developer-tools, git]
tags: [git]
toc: true
toc_sticky: true
date: 2022-08-18
---

<br>

> ## '공부하는 공작새'님의 블로그를 참고하여 작성했습니다.    
<br>
<br>
<br>
<br>
<br>


# git fetch

원격저장소(깃허브 인터넷 상)에 있는 변경사항들을 로컬저장소(내 노트북)에 가져오기 전에 변경내용을 확인하고 싶은경우에 사용\
만약 제가 공동으로 작업하는 파일을 github에 올리고 잤어요.\
근데 다음날 누군가 제 파일에 수정을 했는지 안했는지 확인하고 싶을때!\
로컬디렉토리(내 노트북)로 변경한 내용을 가져오지 않고 변경한 내역들만 확인\
그 명령어가 fetch 입니다.

<br>
<br>
<br>
실험을 위해 파일을 하나 만들어봅시다!


```bash
echo "hiya it's me Joonhwan!" > yes.txt
```
그 후 git add ~ push 를 통해 원격저장소에 올려줍시다, 이제 깃허브로 가보면
<br>
<br>
<br>
<br>

![Desktop View](/assets/img/git/fetch-pull/1.PNG)

<br>
<br>
<br>

이제 여기서 직접 수정해줍시다

<br>
<br>
<br>

![Desktop View](/assets/img/git/fetch-pull/2.PNG)

<br>
<br>
<br>

 완료 후 맨 아래에 내려가서 `commit changes` 클릭
 
 <br>
 <br>
 
 ```bash
 git fetch 원격저장소 이름
 ```
 저는 origin 이라서     `git fetch origin` 으로 했습니다

<br>
<br>
<br>

![Desktop View](/assets/img/git/fetch-pull/3.PNG)

<br>
<br>
<br>
<br>

그 후 
```bash
git fetch -r
```

해당 명령어를 치면 fetch 확인 가능한 브랜치 내역들이 나옵니다\
자 저는 `origin/gh-pages`  `origin/main` 2가지가 있는데 origin/main 을 선택하겠습니다.

<br>
<br>

```bash
git checkout origin/main
```

<br>
<br>
<br>

![Desktop View](/assets/img/git/fetch-pull/4.PNG)

<br>
<br>
<br>

빨간 네모를 잘 봐주시고 이제 깃허브로 갑시다


<br>
<br>
<br>

![Desktop View](/assets/img/git/fetch-pull/5.PNG)

<br>
<br>
<br>
action을 눌러 저는 해당되는 숫자를 찾아 클릭해보니 
<br>
<br>
<br>

![Desktop View](/assets/img/git/fetch-pull/6.PNG)

<br>
<br>
<br>

### 


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
'Outsider's Dev Story' &nbsp;&nbsp;&nbsp;&nbsp;   [git이 추적하지 않는 untracked files 한꺼번에 삭제하기](https://blog.outsider.ne.kr/1164)

<br>

'noyo0123.log' &nbsp;&nbsp;&nbsp;&nbsp; [git 명령어 정리, 에러정리](https://velog.io/@noyo0123/git-%EB%AA%85%EB%A0%B9%EC%96%B4-%EC%A0%95%EB%A6%AC-%EC%97%90%EB%9F%AC%EC%A0%95%EB%A6%AC-znk1zz2k5e)

<br>

'gmlwjd9405' &nbsp;&nbsp;&nbsp;&nbsp;[[Git] git add 취소하기, git commit 취소하기, git push 취소하기](https://gmlwjd9405.github.io/2018/05/25/git-add-cancle.html)
