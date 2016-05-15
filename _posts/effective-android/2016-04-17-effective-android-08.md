---
layout: post
title: "Effective android 08) Use static nested over inner class"
modified:
categories: effective-android
excerpt:
tags: [android, effective-android]
image:
  feature:
date: 2016-04-17
---

### 8. Use static nested over inner class
Inner class is an element of the class

   It increases the time & space complexity
   Might causes the memory leaks because the inner class always need enclosing class as well
   Use static class for the view holder in the adapter
   Always use static for the nested classes if you are not using any member of the enclosing class

### 8. Use static nested over inner class
내부 클래스는 그 클래스의 요소이다.

시간이 지나고 복잡성이 높아질수록, 메모리 릭이 발생할 가능성이 높아진다. 왜냐하면 내부 클래스는 자신이 정의되어있는 클래스가 항상 필요하기 때문인다. 
Adapter 에서 View holder 를 위해서는 static 클래스를 사용해라. 자신을 정의하고있는 클래스의 멤버변수들을 사용하지 않은다면 static 클래스를 이용하여 내부 클래스를 정의해라.


<br><br>

#### 고찰
static 을 이용해서 정적 클래스로 정의할경우 static 변수와 같이 해당 클래스의 instance 가 없이 사용할수 있다. 그러니 static 을 이용하는게 해당 클래스의 멤버변수들을 사용하지 않는다면 이득 이다는 얘기인듯 하다. static 키워드에 대한 이야기 인듯 하다.

[출처] [http://orhanobut.github.io/effective-android/](http://orhanobut.github.io/effective-android/)