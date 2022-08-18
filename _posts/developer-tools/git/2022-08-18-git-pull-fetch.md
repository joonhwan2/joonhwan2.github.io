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

---
> * # git fetch
---
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

자 원격저장소에서는 바뀌었지만 로컬저장소(내 노트북)에서는 어떨까?\
바로 확인해봅시다.

<br>
<br>

![Desktop View](/assets/img/git/fetch-pull/7.PNG)

<br>

오! git fetch는 원격저장소만 수정하고 내 로컬저장소는 수정하지 않군요.\
여기까지가 git fetch이고 이어서 git pull 합니다 ㅎ

<br>
<br>
<br>
<br>

---
> ####  ❗ 잠깐 혹시 `git push` 하다가 오류나서 `git status` 쳤는데 이런 오류가 떳다면 여기를 참고해주세요 ㅎ
---
![Desktop View](/assets/img/git-error/head-detached-at/1.PNG)

### [HEAD detached at 오류 해결하러가기!](https://joonhwan2.github.io/posts/git-fetch-head-detached/)


<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

---
> * # git pull
---

![Desktop View](/assets/img/git/fetch-pull/8.PNG)

<br>
<br>

> 이번에 깃허브로 들어가서 이렇게 고쳐봤습니다.

<br>

```bash
git pull origin main
git push 여기까지  
```

![Desktop View](/assets/img/git/fetch-pull/9.PNG)

<br>
<br>
<br>

자 아까랑 똑같은 방법으로 깃허브 -> action -> 해당되는 숫자 클릭을하면 변화를 보실 수 있을겁니다.\
그 후 노트북의 그 파일을 열어보면 

<br>
<br>
<br>

![Desktop View](/assets/img/git/fetch-pull/10.PNG)

<br>
<br>

git pull은 제 노트북의 자료도 동일화 시키네요!\
자 이로서 `git fetch`와 `git pull`차이를 알아보았습니다. 자료들이 도움이 됬길 바랍니다 ㅎㅎ

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
# 참고 &nbsp;
---
'공부하는 공작새' &nbsp;&nbsp;&nbsp;&nbsp;   [git fetch pull 사용법, fetch pull 차이점 알아보기](https://chaeyoung2.tistory.com/43)
