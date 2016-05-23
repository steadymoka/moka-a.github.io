---
layout: post
title: "Effective android 14) Do not use magic strings/numbers"
modified:
categories: effective-android
excerpt:
tags: [android, effective-android]
image:
  feature:
date: 2016-05-23
---

### 14. Do not use magic strings/numbers
Our aim should be more readability, in order to achive that we need to make everything clear for ourselves

- Always use constants to explain your magic string/numbers
- empty string is not magic
- 0 is not magic if you use it to check size

<br> 

### 14. Do not use magic strings/numbers
모든것들을 그 쓰임에 명확하게 만들어야 하기 때문에, 우리의 초점은 가독성을 높이는 거다.

- 매직 string/numbers 를 설명하기 위해서는 항상 constants 를 사용해라
- 빈 문자열은 magic 이 아니다.
- size 확인을 위해 0을 이용하면, 0 은 magic 아니다

<br><br>

##### 고찰
magic string/numbers 는 무언가를 최적화하기 위한 값을 말한다. 예를들어 알파값을 10으로 했을 때 가장 투명한게 아름답다 싶으면 10이 magic number이다.

그런데 제목은 Do not use .. 라면 사용하지 말라는 말인데, 내용은 constant / ( static final ) 을 이용하라고 한다. 그러면 사용하라는 말인지 아닌지.. 헤깔린다. 빈문자열은 magic 이 아니고 사이즈 확인을 위한 0 은 magic 이 아니다.. 라고 한다.

그래서 gdg 사람들과 토론을 한결과, `jihwan`님께서 위키에서 찾아주셨다. (나는 네이버 영어사전 찾아만 본 ..) `magic number 란 값인데 정확한 의미를 내포하지 않은것` 이다.

예를들어 `String userName` 은 변수명인데, `int userName = student + 100` 이렇게 쓸경우에는 100이 magic number 라 할수 있다. 작동은 하지만 100에는 아무런 뜻이 내포되어 있지 않기 때문이다.

따라서 constants 로 상수지만 의미를 내포하도록 써라는 말이고, 비거나 0은 magic number 가 아니라는 말인듯 하다.

[출처] [http://orhanobut.github.io/effective-android/](http://orhanobut.github.io/effective-android/)         