---
layout: post
title: "Effective android 07) Aim for Loosely Coupling"
modified:
categories: effective-android
excerpt:
tags: [android, effective-android]
image:
  feature:
date: 2016-03-21
---

### 7. Aim for Loosely Coupling
 Each component should have less depend on other components
Use EventBus to communicate from activity to fragment. Thus you don't have to rely specific fragment
 Use EventBus to deliver network responses
Avoid casting as much as possible. ie: In fragment, if you call MainActivity, you will be tightly coupled with the main activity class. Avoid doing this. Use abstractions such as interface

### 7. Aim for Loosely Coupling
각 컴포넌트는 다른 컴포넌트들과 의존관계가 적어야 한다. 액티비티와 프레그먼트와 소통을 위해 이벤트 버스를 사용해라. 그러면 특정 프레그먼트에 의존 하지 않아도 된다.
 네트워크 응답을 전달하기 위해 이벤트 버스를 이용해라
 가능한 캐스팅을 피해라. 예를 들어 프레그먼트에서, MainActivity 를 부르면 main activity 와 단단히 묶이게 된다. 이것을 피해라. 
  인터페이스로 추상화 해라.

<br><br>

#### 고찰
네트워크의 응답을 전달할때 이벤트 버스를 이용하라고 하는데 .. 보통 전달할때 프레그먼트 객체에 바로 setter 함수같은걸로 만들어서 하지는 않는다. 하지만 프로젝트가 크고, 화면의 이벤트가 많을경우 이벤트 버스로 계속 이벤트 던지면 코드 가독성도 많이 떨어지고 프로젝트가 점점 복잡해지는것 같다. 아직 제대로 알아보지는 않았지만, redux 나 flux 같은 이벤트 의 흐름이 아닌 데이터의 흐름으로 구현하는게 좋을것 같다.


[출처] [http://orhanobut.github.io/effective-android/](http://orhanobut.github.io/effective-android/)