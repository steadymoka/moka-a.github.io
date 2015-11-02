---
layout: post
title: "안드로이드 노트1 - 2015.07.28"
modified:
categories: android
excerpt:
tags: [android, actionbar]
image:
  feature:
date: 2015-10-20T19:39:55-04:00
---
<br>
<br>

#### 액션바
롤리팝이되면 툴바로 변경이 되었다

- public abstract void setDisplayHomeAsUpEnabled (boolean showHomeAsUp)<br> 
액션바 아이콘을 네비게이터로 표시한다.
- Android 4.0 ( API Level 14 ) 부터는 application icon 이 기본으로 action item 으로 작동하지 않는다. 기본으로 작동하게 하려면 setHomeButtonEnabled( true ) 를 호출해주어야 한다.
<br>
<br>
<br>

#### 리사이클러뷰
{% highlight java %}
android:clipToPadding:"false"
{% endhighlight %}
- 리사이클러뷰로 padding 을 줄때, 위 속성을 이용하여 해더나 푸터에 패딩을 주면 좋다.
<br>
<br>
<br>
<br>

#### 프레그먼트 라이프사이클
Fragment 클래스 객체 생성후 setter 함수를 부르면 onCreateView 가 불리기전에 setter 가 먼저 불린다.
객체를 생성해도 onCreateView 는 불리지 않는다. 화면으로 create 될때 함수가 불린다.
<br>
<br>
<br>

#### 뷰의 touchEvent
특별한 기능을 넣기위해 뷰를 상속 받아 onTouchEvent 를 오버라이딩 할때,
event 의 getX()와 getY() 는 해당 뷰의 x,y 값을 리턴해주고, getRawX()와 getRawY() 는 화면에서 절대좌표를 반환해준다.
<br>
<br>
<br>
<br>

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
<br>
<br>
<br>
<br>

#### View 이벤트
onTouchEvent 에서 false 를 리턴시키면 모든 이벤트 동작을 안한다.
requestDisallowInterceptTouchEvent 를 true 로 넣어주면 ACTION_CANCEL 이 발생하지 않는다.
<br>
<br>
<br>
<br>

#### Cannot call this method while RecyclerView is computing a layout or scrolling
리사이클러 뷰에 레이아웃 매니저를 이용하여 grid Count 를 변경 시켰다.

{% highlight java %}
layoutManager.setSpanCount( gridCount );
layoutManager.requestLayout();
{% endhighlight java %}

를 이용하여 그리드 카운트를 변경시켰는데, 다른 액티비티에 갔다가 와서 스크롤할때
<br>Cannot call this method while RecyclerView is computing a layout or scrolling<br>
에러가 발생한다 

해결해야 한다
구글 검색 결과 notifyDataSetChanged 호출을 Handler 를 이용하여 post 해주라고 하여 하니까 일단은 되는것 같다. flag 를 두어서 해결하는 방법도 있는듯 하다. >>>> 정확한 문제를 잘 모르겠다.

그리고 또 다음 에러가 발생한다. recyclerView 내부 에러인듯 하다
<br>Inconsistency detected. Invalid item position 14(offset:14).state:27<br>
이에러는 뷰가 리프레쉬 될때 스크롤을 할시 발생하는 버그이다. isRefreshing 이라는 플레그값을 두어 onTouch 에서 리프레쉬 중일때 이벤트를 막는걸로 임시방편을 하였다. 

내부 버그라 다음 릴리즈일때 고쳐진다는것 같은데, 정확한건 모르겠다. 

그리드 카운트를 바꾸는 좋은 방법이 있으면 알려줬으면 좋겠다.
<br>
<br>
<br>
<br>

#### EditTextView 의 입력다한후 다음 버튼 없애기 
{% highlight java %}
	android:imeOptions="actionDone"
{% endhighlight java %}

<br>
<br>
<br>
<br>

#### EditTextView 입력에 따라 실시간으로 갱신해야 할때
editText 에 addOnTextChange 를 호출하여 리스너를 등록한후 
afterText 를 구현한다.

setOnFocusChanged 를 이용하여 hasFocus 가 false 일때를 구현하면 포커스를 잃을때 행동을 할수 있다.

<br>
<br>
<br>
<br>
    
#### 이미지 resize 하기
{% highlight java %}
    Bitmap sizingBmp = Bitmap.createScaledBitmap(bitmap, (int) width, (int)height, true);
{% endhighlight java %}

<br>
<br>
<br>
<br>









