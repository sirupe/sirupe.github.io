---
layout: post
title:  "[Mac] Intelli J 단축키 변경하기"
categories: IntelliJ
img: use-post/intellij.jpg
toc: true
---
<br/><br/>
강력한 추천을 받아 Intelli J 를 사용하기로 했습니다. (mac 에는 Intelli J 지! 라며..)

이전까지는 이클립스를 사용해오던 터라 단축키가 매우 헷갈리기도 했고,

맥북을 사용중인지라 이미 바꾼 맥북 키설정과도 충돌에 문제가 없도록

Intelli J 의 단축키 변경이 반드시 필요했어요.

<br/><br/>


### 1. 단축키 변경을 위한 메뉴
* * *

`IntelliJ IDEA` > `Preferences...` > `Keymap`

mac 의 최상단에 항상 나오는 메뉴 중 위 메뉴를 따라 선택하면

![menu]({{ site.url }}/upload/images/2017-10-08-intellij-keymap-change/menu.jpg)

<br/>

아래의 `Preferences` 창이 떠요.

`Preferences` 메뉴 중 `Keymap` 메뉴가 단축키를 변경할 수 있는 메뉴에요 !

주로 봐야 할 부분은 빨간 테두리 영역이지요 :D

![keymap]({{ site.url }}/upload/images/2017-10-08-intellij-keymap-change/keymap.jpg)

<br/><br/>

### 2. 단축키를 변경할 작업 찾기
* * *

변경하고 싶은 동작의 keyword 를 영문으로 알고 있다면 검색해주세요.

![keymap]({{ site.url }}/upload/images/2017-10-08-intellij-keymap-change/search-word.jpg)

<br/>

적용하고 싶은 단축키가 어떤 동작에 매핑되어있는지 찾고 싶다면

검색바 우측의 파란 돋보기 모양의 아이콘을 클릭하세요.

클릭하면 아래의 사진처럼 `Find Shortcut` 창이 뜨는데요,

`Find Shortcut` 창에서 단축키를 마구마구 입력해보면

해당 단축키가 매핑되어 있는 동작이 찾아집니다!

![keymap]({{ site.url }}/upload/images/2017-10-08-intellij-keymap-change/find-shotcut.jpg)

<br/><br/>

### 3. 단축키 변경하기
* * *

단축키를 변경하고 싶다면 위의 방법으로 찾은, 변경하고 싶은 동작 명을 우클릭하세요.

![keymap]({{ site.url }}/upload/images/2017-10-08-intellij-keymap-change/right-click.jpg)

우클릭하면 각종 메뉴들이 나옵니다.

```
Add Keyboard Shortcut
> 키보드 단축키를 등록합니다.

Add Mouse Shortcut
> 마우스 단축키를 등록합니다.

Add Abbreviation
> Search Everywhere (더블 shift) 화면에서 제일 먼저 검색될 검색어를 입력합니다. (Top hit 에 표시됩니다.)

Remove ...
> 등록된 단축키, 검색어 등을 삭제합니다.
```

<br/>

등록하고자 하는 메뉴를 선택하여 동작시킬 단축키를 입력합니다.

만약 등록하고자 하는 단축키가 이미 다른 동작에 매핑되어 있으면 경고메세지가 뜨게 됩니다.

![keymap]({{ site.url }}/upload/images/2017-10-08-intellij-keymap-change/keyboard-shortcut.jpg)

<br/>

경고메세지를 살펴보면 어떤 동작에 해당 단축키가 등록되어 있는지 보여요 :D

어쨌든 OK 를 눌러보면 .. 아래와 같은 창이 뜨는 것을 볼 수 있어요.

![keymap]({{ site.url }}/upload/images/2017-10-08-intellij-keymap-change/warning.jpg)

```
[Leave] 등록하고자 하는 단축키를 등록하고 기존의 단축키도 그대로 놔둡니다.
[Cancel] 단축키 등록작업을 취소합니다.
[Remove] 기존에 등록되어 있던 단축키를 삭제하고 현재 등록하고자 하는 단축키를 등록합니다.
```

원하는 작업의 버튼을 클릭해주시면 단축키 변경 작업은 완료됩니다.

<br/><br/>

### 4. 단축키 삭제하기
* * *

기존에 등록되어 있던 단축키를 미처 확인하지 못했다거나

다시 등록하기가 귀찮다거나 하면 `Leave` 를 선택하게 될텐데요,

그렇게 되면 하나의 단축키에 두 가지의 작업이 매핑되어 버려요. :(

![keymap]({{ site.url }}/upload/images/2017-10-08-intellij-keymap-change/overlap-search.jpg)

다시 키매핑으로 동작을 찾아보면 두가지가 나오는 것을 확인할 수 있습니다.

<br/>

기존의 동작이 쓸데가 없다면 해당 단축키를 `Remove` 해주시면 됩니다 :D

![keymap]({{ site.url }}/upload/images/2017-10-08-intellij-keymap-change/remove.jpg)

<br/><br/>

### #잠깐.. Add Abbreviation ??
* * *

단축키를 등록하는 메뉴 중 키보드와 마우스를 제외한 `Add Abbreviation` 이 있었던 것을 기억하실거에요.

익숙하지 않은 영어에.. 뜻을 찾아봐도 '축약형' 이라는 뜻만 나오고..

해서 무슨 기능인지 찾아봤습니다 :D

먼저 일단 `Add Abbreviation` 을 등록해봅니다.

![keymap]({{ site.url }}/upload/images/2017-10-08-intellij-keymap-change/abbreviation.jpg)

<br/>

등록한 글자가 단축키가 표시되는 부분에 표시되는군요 !

이렇게 등록된 `Abbreviation` 은 키워드를 입력해서 찾는 검색창에 검색하여도 검색이 됩니다.

하지만 여기서 검색하려고 만든 기능은 아니에요 :D

<br/>

Intelli J 를 좀 사용해보셨다면 쉬프트키를 두번 누르면 (Double Shift)

`Search Everywhere` 창이 뜨는 것을 본 적이 있으실거에요.

찾고자 하는 파일명이나 기능명을 입력하면 굳이 찾아가지 않아도 바로 찾을 수 있어요.

여기에 아까 임의로 등록했던 'para' 를 검색해보면 ~

![keymap]({{ site.url }}/upload/images/2017-10-08-intellij-keymap-change/search-everywhere.jpg)

`Top Hit` 부분에 제일 상단에 해당 메뉴가 조회되는 것을 볼 수 있어요 ~

<br/>

아예 안쓰는 것도 아니고.. 단축키를 등록해놓기도 애매하고..

가끔 찾아갈 때마다 경로는 너무 헷갈리는 메뉴 등을

찾기 편한 단어로 등록해놓으면 좋을 것 같아요 :D