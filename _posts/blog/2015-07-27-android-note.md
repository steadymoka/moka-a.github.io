---
layout: post
title: "안드로이드 노트"
modified:
categories: blog
excerpt:
tags: [android, actionbar]
image:
  feature:
date: 2015-07-27T19:39:55-04:00
---

#### 액션바
롤리팝이되면 툴바로 변경이 되었다

- public abstract void setDisplayHomeAsUpEnabled (boolean showHomeAsUp)<br> 
액션바 아이콘을 네비게이터로 표시한다.
- Android 4.0 ( API Level 14 ) 부터는 application icon 이 기본으로 action item 으로 작동하지 않는다. 기본으로 작동하게 하려면 setHomeButtonEnabled( true ) 를 호출해주어야 한다.

#### 리사이클러뷰
{% highlight java %}
android:clipToPadding:"false"
{% endhighlight %}
- 리사이클러뷰로 padding 을 줄때, 위 속성을 이용하여 해더나 푸터에 패딩을 주면 좋다.

#### 프레그먼트 라이프사이클
Fragment 클래스 객체 생성후 setter 함수를 부르면 onCreateView 가 불리기전에 setter 가 먼저 불린다.
객체를 생성해도 onCreateView 는 불리지 않는다. 화면으로 create 될때 함수가 불린다.

#### 뷰의 touchEvent
특별한 기능을 넣기위해 뷰를 상속 받아 onTouchEvent 를 오버라이딩 할때,
event 의 getX()와 getY() 는 해당 뷰의 x,y 값을 리턴해주고, getRawX()와 getRawY() 는 화면에서 절대좌표를 반환해준다.

#### Animnation
{% highlight java %}
// 전체 속성
@android:anim/accelerate_interpolator // 애니메이션을 점점 빠르게 
@android:startOffset // 애니메이션을 시작하기 전 대기
@android:repeatCount // 반복 횟수 입니다 -1은 무한반복
@android:repeatMode (restart, reverse) // 반복 모드 restart는 애니메이션을 처음부터 반복, reverse는 애니메이션을 반시계방향으로 다시 실행
@android:interpolator // 애니메이션 효과가 지속되는 동안 빠르게, 또는 느리게 효과가 진행
@android:fillAfter // 애니메이션 효과가 끝난뒤에 상태를 유지할지 결정, 기본은 false
{% endhighlight java %}

 - translate<br> fromXDelta : "숫자" - 픽셀돤위의 뷰위치, "%" - 뷰너비의 퍼센트 연산, "%p" - 부모뷰의 대한 퍼센트 
 - scale
 - alpha
 - rotation

animation 구현할때는 ViewHelper 를 쓰는게 답인것 같다. ViewHelper 가 어떻게 동작하는지 내부를 확실하게 알아 볼 필요가 있다. ViewHelper 는 짱짱이었다 ...
