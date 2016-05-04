---
layout: post
title: "Effective android 06) High Cohesion"
modified:
categories: effective-android
excerpt:
tags: [android, effective-android]
image:
  feature:
date: 2016-02-17
---

### 6. High Cohesion
All related elements should be together
Do not create a different constant class for the bundle/sharedpreferences keys. Instead put it to related class. Use static factory method.

Class should contain only members related to its responsibility
Ask yourself, if the method/variables should be in this class, if the answer is no, then put it into the correct class or create a class for that Remember to put common methods to the utility classes


### 6. High Cohesion
모든 요소들은 연관되어 있다. 
bundle/프리퍼런스 키값들을 다른 constant 클래스에 만들지 마라.
대신에 그것과 연관된 클래스에 static 변수 또는 함수를 이용하여 넣어라.

클래스는 자신과 연관된 멤버들만 포함해야 한다. 만약 함수/변수등이 그 클래스에 꼭 있어야 될것들이 아니라면 맞는 클래스 또는 util 클래스를 만들어서 거기에 넣어라.

<br><br>

#### 고찰
어떠한 클래스가 있을때, 해당 클래스와 관련된것만 멤버 변수와, 함수로 정의하라는 것인듯 하다.
그리고 KEY 값들 같은경우에는 다른 클래스에 모두 빼서 일괄적으로 적어놓기도 했었는데, 위 글에 반하는 것 같아 고민을 좀 해봐야 겠다. 같은 기능의 키값인데 여러 클래스에 있으면 중복되는 느낌이 있어 한곳에 몰아 넣었기 때문에, 뭐가 딱 맞다고 할수는 없는듯 하다.

[출처] [http://orhanobut.github.io/effective-android/](http://orhanobut.github.io/effective-android/)