---
layout: post
title: "Effective android 11) Use final where applicable"
modified:
categories: effective-android
excerpt:
tags: [android, effective-android]
image:
  feature:
date: 2016-05-17
---

### 11. Use final where applicable
- final declared fields cannot be reassigned
- final declared methods cannot be overriden
- final declared classes cannot be inherinted

- Ask yourself why you should not use final for the variable declaration
- Ask yourself why you should use final for method and class declaration
- For readability, I prefer to not use final for paramaters and local variables

<br> 

### 11. Use final where applicable
- final 로 정의된 필드는 다시 정의 될수 없다.
- final 로 정의된 함수는 override 될수 없다.
- final 로 정의된 클래스는 상속 받을수 없다.

- 변수 선언시에 왜 final 로 하지 않았는지 의문을 가져라 
- 함수 선언시와 클래스 선언시에 왜 final 로 정의하는지 의문을 가져라 
- 가독성을 위해서는, 파라미터와 로컬 변수 선언시에는 final 로 전언하지 않는것이 좋다. 

<br><br>

#### * 고찰

``` java
final FileInputStream in;
if(test)
  in = new FileInputStream("foo.txt");
else
  System.out.println("test failed");
in.read(); // Compiler error because variable 'in' might be unassigned
}
```
변수 in 에는 test 의 값에 따라 할당이 될수도 있고 안될수도 있다. 이런경우에 final 을 안붙일 경우 in.read() 에서 test 가 false 인 경우 Null Point Exception 이 날수가 있는데, final 키워드로 in 변수를 잡아주면, compile 단에서 in 이 초기화가 안되었기 때문에 error 가 나게되어 run time 에 에러날수있는 것을 피할수 있다. <br> 

이 외에도 다양하게 크레쉬가 날수있는 부분에서 사전에 컴파일단에서 에러를 방지 할수 있게 된다.
`성능상`에는 이점이 없지만, 쓰는 습관을 들이는것이 좋을것 같다 !
<br>
<br>
참고자료<br>
[http://stackoverflow.com/questions/137868/using-final-modifier-whenever-applicable-in-java](http://stackoverflow.com/questions/137868/using-final-modifier-whenever-applicable-in-java)

[출처] [http://orhanobut.github.io/effective-android/](http://orhanobut.github.io/effective-android/)         


