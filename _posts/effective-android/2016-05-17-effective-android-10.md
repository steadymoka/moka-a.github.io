---
layout: post
title: "Effective android 10) Use lazy load"
modified:
categories: effective-android
excerpt:
tags: [android, effective-android]
image:
  feature:
date: 2016-05-17
---

### 10. Use lazy load
- Instantiate the json parser when needed
- Instantiate the network adapter when needed
- Instantiate fragments when they are called in the viewpager
- Instantiate lists/maps when you need it.

<br> 

### 10. Use lazy load
- json 파싱은 필요할때 해라
- 네트워크 통신은 필요 할때 해라
- 뷰페이저에서 프레그먼트가 불릴때 초기화 해라
- lists/map 은 그것이 필요할때 초기화 해라 


<br><br>

#### * 고찰
lazy load <br> 
필요할때 그때 그때 하라는 말인듯 하다. <br> 
사실 무슨 의미인지 크게 와닿지 않는다 .. <br> 
<br> 
인터넷을 찾아보니 signton 패턴을 이용할때 <br> 

``` java
private Signleton instance;  // instance = new Signleton 하지 않는다.

public Signleton getInstance() {

  if ( null == instance )
    instance = new Signleton;
  return instance;
}
```
이렇게 instance 를 바로 객체 생성하지 않고 필요할때 객체를 생성 할수 있도록 한다. <br> 
<br>
<br>
참고자료<br>
[http://www.javaworld.com/article/2077568/learn-java/java-tip-67--lazy-instantiation.html](http://www.javaworld.com/article/2077568/learn-java/java-tip-67--lazy-instantiation.html)

[출처] [http://orhanobut.github.io/effective-android/](http://orhanobut.github.io/effective-android/)         


