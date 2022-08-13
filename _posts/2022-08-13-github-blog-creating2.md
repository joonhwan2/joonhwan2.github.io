---
title:  "깃허브 블로그 만드는 법 2"
excerpt: "3"

categories:
  - github blog 
tags:
  - [blog, jekyll, github, git]

toc: true
toc_sticky: true
 
date: 2022-08-13
---
<br>

---
# 2-1 jekyll 그리고 Ruby 설치
---
<br>

[지킬테마 다운로드](https://github.com/topics/jekyll-theme)\
<br>

저는 jekyll-chirpy-theme 을 사용하였습니다. [👉저가 사용한 테마를 다운받으시려면 이곳을 클릭 해주세요](https://github.com/cotes2020?tab=repositories)\
제가 사용한 테마를 다운받으시려는 분들은 들어가면 지금부터 저 화면이 나올겁니다.

<br>

![Desktop View](/assets/img/2022-08-13/0.PNG)

<br>
빨간색을 클릭하면
<br>

![Desktop View](/assets/img/2022-08-13/01.PNG)
<br>

code를 눌러 downloadzip을 통해 일단 찾기 쉽게 저장해두는 것을 권장합니다. 저는 바탕화면에 저장 후 진행하였습니다.

<br>
<br>
<br>
<br>

이제 루비를 다운로드 해보겠습니다.&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[💎루비 다운로드 하기](https://rubyinstaller.org/downloads/)\
들어가면 이제 바로 아래와 같은 화면이 나올 겁니다
<br>
<br>


![Desktop View](/assets/img/2022-08-13/1.PNG)

<br>
<br>

무조건 **with devkit**과 있는 것을 다운로드 해주셔야하며, 저의 경우에는 2.7을 다운받았습니다\
맨 처음에 3.1을 받았는데 오류가생겨 재설치하였습니다.

<br>
<br>
<br>
<br>


![Desktop View](/assets/img/2022-08-13/2.PNG)

<br>
<br>
<br>

<u>저거 체크 안하면 조금 피곤해집니다, 꼭 체크 까먹지마시길 바랍니다 ㅎ<u/>

<br>
<br>
<br>

![Desktop View](/assets/img/2022-08-13/3.PNG)

<br>
<br>

이제 진행되다보면 이 화면이 뜰텐데 엔터 한번씩 눌러주고 저의 경우에는 혹시나 몰라서 1,2,3 을 각각 순차적으로 한번씩 더 눌러주었습니다.
1,2,3을 각각 눌렀을때 이런 문구가 뜨면 성공

<br>
<br>

![Desktop View](/assets/img/2022-08-13/4.PNG)\
그 후 엔터를 누르면 자동으로 꺼집니다.

<br>
<br>
자 이제 바탕화면에서 windows를 눌러 ruby 검색후 ruby cmd 클릭

<br>
<br>

![Desktop View](/assets/img/2022-08-13/5.PNG)

<br>
루비가 잘 설치되었나 확인하기위해

```bash
$ruby -v
```
입력해보자 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; (혹시나 모르시는 분들을 위해, %는 빼고 입력입니다!)
  
<br>

![Desktop View](/assets/img/2022-08-13/6.PNG)
  
<br>
이렇게 뜨면 설치가 잘 되었다는 뜻이다.
  
<br>

이어서
```bash
$gem install jekyll bundler
$gem install webrick
$cd 원하는 경로로 이동
```

<br>
  
저의 경우에는 cd desktop을 입력하여 C:\Users\rhwns\Desktop> 이상태로 만든 후
```bash
jekyll new 폴더명 아무거나
```
저는 yes로 했습니다.
<br>
<br>
  
![Desktop View](/assets/img/2022-08-13/7.PNG)

<br>
<br>

자 이렇게 폴더가 생성된 것을 확인할 수 있습니다.
  
<br>

```bash
$cd 폴더경로로 이동
$bundle install 
$bundle exec jekyll serve
```
이렇게 해보고 오류가 떠서 안된다면

<br>

```bash
$bundle update
$bundle add webrick
$bundle exec jekyll serve
```
<br>

그래도 안되면 gem update --system을 콘솔창에 입력한 뒤 처음부터 전부 싹 다 해봅시다.
```bash
$gem update --system
$gem install jekyll bundler
$gem install webrick
$bundle install
$bundle update
$bundle add webrick
$bundle exec jekyll serve
```
<br>
<br>

성공한다면 이런창이 뜹니다
  
<br>
<br>
  
![Desktop View](/assets/img/2022-08-13/8.PNG)

<br>
serer address 부분에 저거를 복사하여 주소창에 입력해보면

<br>
<br>

![Desktop View](/assets/img/2022-08-13/9.PNG)
  
<br>
<br>

자 이제 거의 다와갑니다!&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 조금만 더 힘 냅시다.  
아까 다운로드 해놓았던 테마안의 파일 전부다 jekyll new ? 로 생성한 그 폴더에 전부다 복사하여 붙여넣습니다\
덮어쓰기O
<br>
<br>
  
---
## 지울 것 2-2
---
   
`.travis.yml`\
`_posts` 폴더 안의 파일들만\
`.github`폴더 안에서, `workflows`폴더를 제외한 나머지\
`.github`폴더 > `workflows`폴더에서 ci.yml 그리고  `issue-pr-interceptor.yml` 그리고 `commitlint.yml` 이 3개 삭제\
`gemfile.lock`\
 `asset`폴더의 404.html 건드리지말고!, 바깥에 있는 404.html 제거
<br>
<br>
  
※수정할 것\
<br>  
`.github>workflows>pages-deploy.yml.hook` 여기서 `.hook`을 지우고\
파일명을 `.github>workflows>pages-deploy.yml`으로 바꿔줍시다\
<br>
(옵션)그리고 깔지 않고싶은 것이 있다면 gitignore 파일을 열어 적고 수정합시다  
<br>

그 후 해당 파일을 열어 branch 부분을 수정합시다.
<br>

```yml
name: 'Automatic build'
 on:
 push:
     branches:
     - master          # 이 부분  master -> main 
     paths-ignore:
     - .gitignore
     - README.md
     - LICENSE  
```

<br>

보통 여기서 완료하고 되는 분들도 있겠지만 저의 경우엔 되지않았습니다. 이 상태에서 로컬 서버를 열기 위한 명령어를 입력 했더니
```bash
$bundle exec jekyll serve
```
<br>
<br>
  
![Desktop View](/assets/img/2022-08-13/10.PNG)

<br>
이 사단이 납니다.
<br>

---
### 🧐자 어떻게 해결하느냐?
---
<br>
우선 vs코드로 들어가서 확장(extension)에서 category:formatters yaml 검색을 하고 다운로드

<br>
<br>

![Desktop View](/assets/img/2022-08-13/11.PNG)

<br>
<br>

`gemfile`을 열어 수정을 하다보면 안보이던 빨간밑줄(오류)가 생겨나기 시작할 것이다.\
이걸 다운로드 받지않은게 문제였다, 그로 인해 시간이 정말 오래걸렸다.

<br>
  
### 이건 제가 쓴 방법입니다.

<br>
  
<br>
  
`gemfile`에서 첫번째 문장인 `# frozen_string_literal: true` 이것을 없애고 3번째 문장에 있던 거를 1번째로 올려준다
`gempec`도 지운다
<br>
<br>
  
```gemfile
gem "webrick", "~> 1.7"
```

<br>
이제 이 문장 밑에 이 문구를 복붙합시다, 그리고 저장
<br>
<br>
  
```  
gem "jekyll"
# This is the default theme for new Jekyll sites. You may change this to anything you like.
gem "jekyll-theme-chirpy"

gem 'jekyll-sitemap'
# If you want to use GitHub Pages, remove the "gem "jekyll"" above and
# uncomment the line below. To upgrade, run `bundle update github-pages`.
# gem "github-pages", group: :jekyll_plugins
# If you have any plugins, put them here!
group :jekyll_plugins do
  gem "jekyll-feed", "~> 0.12"
end
```
<br>
<br>
  
이제 이걸 치면
```bash
$bundle exec jekyll serve 
```
또 로컬 주소가 생성이될텐데 아까 그거 복사해서 주소에 붙여넣으면 짜잔

<br>
<br>

![Desktop View](/assets/img/2022-08-13/12.PNG)
  
<br>
<br>

다음글 이어서 마지막 수정 및 추가편 작성합니다ㅎ\
여기까지 완료하는데 많은 도움을 주셨던 https://github.com/nyannyacha 선생님 감사드립니다!

<br>
 
<br>

아래 사진을 클릭하면 제 취미공간입니다 ㅎㅎ 여기에서 메모 및 일상을 기록하는데 놀러오실 분들은 언제나 환영합니다!

<br>

# 링크로 이동하려면 사진을 클릭

[![어서오셔 ㅎ](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQk-zPB4TCuWRNJVIF0aWgniDPNJgUTdXmILg&usqp=CAU)](https://discord.gg/zkzk5xtm)


---
# 참고
---
 'dodev' &nbsp;&nbsp;&nbsp;&nbsp;   [초보자를 위한 GitHub Blog 만들기 - 3](https://wlqmffl0102.github.io/posts/Making-Git-blogs-for-beginners-3/)

<br>
https://chirpy.cotes.page/posts/getting-started/
