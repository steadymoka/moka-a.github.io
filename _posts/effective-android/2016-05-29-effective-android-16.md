---
layout: post
title: "Effective android 16) Do not cross max line width"
modified:
categories: effective-android
excerpt:
tags: [android, effective-android]
image:
  feature:
date: 2016-05-29
---
Effective Android 번역및 회고 글입니다


### 16. Do not cross max line width
- For android I use 120 as default. I think it is better
- Use group formatting when the line is long
- Remember to localize if you want to shorten the line. Don't be afraid of that

<br>

### 16. Do not cross max line width
- 안드로이드에서는 120자를 기본으로 사용하는것이 좋다.
- 라인이 길때는 그룹 포맷팅을 사용해라
- 라인을 좀더 짧게 만들고 싶으면 localize 를 기억하고, 그것을 하는것을 두려워 하지마라

<br><br>

##### 고찰
첫번째 말의 120 이라는 숫자가 무엇인지 사실 감이 오지 않는다 ..<br>
제목이 최대 라인의 가로길이름 넘지 말라는것으로 보아 한줄 코드의 갯수 인것 같은데, 120자이면 너무 길기도 한것 같다.

그리고 그룹포멧팅이라는것도 무잇인지 .. 확실지는 않지만, 안드로이드스튜디오에서 지원해주는 자동포맷팅 기능이 정말 유용하다. 여럿이서 협업을 할경우에도 서로의 코드 포맷이 일치될수 있고, 보기에도 깔끔하다.

localize 도 사실 무엇인지 정확한 의미를 모르겠다. 이것은 로컬변수로 만들어서 사용하라는 말인것 같다. 이것에 대해서는 어떤 로직의 결과를 이용하는 경우가 두번이상일경우에는 로컬 변수로 위에서 빼놓은후 이것을 사용하는것이 로직을 두번타는것 보다는 낫기때문에 로컬 변수로 빼는것이 낫다는 경험이 있다.

effective-android 여기 글이 적어도 1년전 글이다 보니 여기글을 꼭 따를 필요는 없는것 같다.

[출처] [http://orhanobut.github.io/effective-android/](http://orhanobut.github.io/effective-android/)         
