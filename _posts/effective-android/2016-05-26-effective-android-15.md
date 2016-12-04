---
layout: post
title: "Effective android 15) Consider law of demeter"
modified:
categories: effective-android
excerpt:
tags: [android, effective-android]
image:
  feature:
date: 2016-05-26
---
Effective Android 번역및 회고 글입니다


### 15. Consider law of demeter
- Each unit should have only limited knowledge about other units: only units "closely" related to the current unit.
- Each unit should only talk to its friends; don't talk to strangers.
- Only talk to your immediate friends

<br>

### 15. Consider law of demeter
- 각 단위는 오직 현재 단위와 가장 관련된 다른 단위에 대해서 제한된 지식을 가져야 한다.
- 각 단위는 오직 그것의 친구들에게만 이야기 해야하고, 모르는 사람에게 얘기 하지 마라.
- 오직 당신의 친구들에게 이야기 해라

<br><br>

##### 고찰
law of demter 을 찾오보니 `데메테르의 법칙` 이라고 한다.

어떤 클라스의 멤버 ( 메소드 또는 속성 ) 는 반드시 다음과 같은 객체들의 멤버들만을 실행시켜야 한다:
- 해당 메소드 또는 속성이 선언된 객체
- 메소드의 파라미터로 보내진 객체
- 메소드 또는 속성이 직접 초기화시킨 객체
- 호출을 위한 메소드 또는 속성으로서 같은 클라스 안에서 선언된 객체
- 전역 객체

말이 어려운데, 모듈간의 의존도를 최대한 낮추는 것이라고 한다. 예로써 메소드 체이닝을 최대한 하지 않는다고 한다. 어떠한 객체의 멤버 함수를 실행할때, 그것을 가지고있는 객체의 메소드를 직접 호출 하지 않는다는 것이다.

사실 이게 크게 어떠한 의미가 있는지 와닿지가 않는다. 굳이 이 법칙을 지켜야 하는건가 ? 라는 생각도 든다. 이법칙을 지켰을때와 안지켰을때 상황이 어떻게 달라질수있는지 실제 코드에서 발견하게 되면 다시 정리를 해봐야 겠다.


[출처] [http://orhanobut.github.io/effective-android/](http://orhanobut.github.io/effective-android/)         
