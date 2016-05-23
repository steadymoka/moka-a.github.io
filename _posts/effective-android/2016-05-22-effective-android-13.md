---
layout: post
title: "Effective android 13) Lower the modifier"
modified:
categories: effective-android
excerpt:
tags: [android, effective-android]
image:
  feature:
date: 2016-05-22
---

### 13. Lower the modifier
- Lower the modifier and don't expose the data
- Increased readabiliy
- Try to localize as much as possible
- Always start with private

<br> 

### 13. Lower the modifier
- 수식자를 낮춰라, 데이터를 노출시키지 마라
- 가독성을 늘려라
- 가능한한 localize 를 해라
- 항상 private 로 시작해라

<br><br>

##### 고찰
수식자를 낮춰라 <br>
수식자는 private / public / protected 를 의미 하는듯 하다. 접근 제어자를 뜻한다 <br>
최대한 private 으로 설정하여 접근을 제한하여, 가독성을 줄이라는 말인듯 하다. <br>

OOP 개념의 캠슐화를 뜻하는듯 한데, 사실 개념은 알겠지만 실무에서 private 으로 하나 public 으로 하나 별반 차이가 없어 보이는건 기분탓인가 ..  좀더 공부가 필요한듯 하다. 

<br> 

### 그래서 캡슐화란
캡슐화를 단순히 private 만을 이용해 데이터를 감추는 것으로 생각을 많이 한다. ( 아마도? ) 하지만, 이렇게만 생각한다면 실제로 코딩할때, 왜 ?? 왜 감추지 ?? 감추는거나 안감추는거나 별반 다를거 없는데 ?? 라는 생각을 하게 될수도 있다. ( 나만 그런가 ? .. ) 

아무튼 캡슐화에 대해 제대로 알 필요가 있다. 그래서 캡슐화는 내부 구조를 숨기고 외부에 노출된 함수를 이용해 최대한 밖에서 사용할때 별 생각 없이(?) 사용할수 있도록 하는것이다. 모듈화 시키는 것이라고 생각이 든다. 

그러니까 그 코드를 사용하는 쪽에서는 내부 코드가 어떻게 변경되든 영향을 최대한 덜 주도록 구현하는것이 캡슐화 시키는 목적이라고 보면 되겠다. 따라서 내부 데이터를 private 으로 숨기고 사용하는 함수 네이밍을 잘해 어떤역활인지 잘 알수있도록 구현 하는 방향인것 같다.

[출처] [http://orhanobut.github.io/effective-android/](http://orhanobut.github.io/effective-android/)         