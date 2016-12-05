---
layout: post
title: "안드로이드 | 터치 이벤트 전달 분석"
modified:
categories: android
excerpt:
tags: [android, event, 이벤트]
image:
  feature:
date: 2015-09-27T19:39:55-04:00
---
### 터치이벤트의 종류
다운 이벤트, 이동 이벤트, 업 이벤트  -> 순서대로 동작하며, 이동이벤트는 생략될수 있다. <br>

디바이스의 터치 정보는 시스템 서비스인 윈도우 매니저에 전달이 되고, 위도우 매니저는 화면에 떠있는 앱에 최종 전달한다.
이까지의 과정은 궂이 알필요 없다. <br><br>


### 터치이벤트 함수
앱에서 터치 이벤트는 최초 액티비티를 통해 최초 전달되며, 액티비티의 두함수를 재정의하여 컨트롤 할수있다. 정의하지 않으면 화면에 배치된 각종 뷰에 순차적으로 전달 된다.

{% highlight java %}
dispatchTouchEvent
onTouchEvent
{% endhighlight %}

화면터치하면 먼저 dispatchTouchEvent 가 호출되고, 이어서 onTouchEvent 가 호출이 된다

이벤트의 정보에는 getX( 이벤트가 발생한 x축 위치 ), getY, getAction, getDownTime( down 이벤트가 발생한 시간 millis ), 등이 표시된다.

이벤트는 가장 최산단의 뷰( 액티비티 )부터 아례로 계층구조로 전달된다.<br><br>

### 터치이벤트의 전달과정
최상단의 뷰에서 가장먼저 dispatchTouchEvent 가 호출되고, 하위뷰 의 dispatchTouchEvent 가 호출된다.
제일 하위단의 자식뷰에서 dispatchTouchEvent 가 호출되면 onTouchEvent 를 호출하게 되고 이때 이벤트를 사용할거면 true 를 리턴시키면된다.
최하위의 onTouchEvent 에서 false 를 리턴하게 되면 dispatchTouchEvent 는 false 를 리턴하게되며 상위단으로 넘어간다. 이때 다시 onTouchEvent 를 호출하게 되고 위 내용을 반복하여 호출한다. onTouchEvent 에서 true 를 리턴하게 되면 dispatchTouchEvent 도 true 를 리턴하게 되고 이떄는 onTouchEvent 를 호출하지 않게된다.

이렇게 이벤트의 흐름이 있고, dispatchTouchEvent 와 onTouchEvent 가 두개있는 이유이다.

dispatchTouchEvent 는 단지 뷰를 탐색해 나가는 기능을 위한 함수이고, 이 이벤트를 받아서 처리를 하려면 onTouchEvent를 재정의하여 처리하도록 한다.


터치다운이벤트에서 이벤트를 처리할 뷰를 선택했다면 ACTION_MOVE 의 이벤트는 더 이하의 뷰까지 전달되지 않고 타겟의 뷰까지만 전달된다. ACTION_UP 도 마찬가지이다.<br><br>


### onIterceptTouchEvent 함수
onIterceptTouchEvent 함수는 자식 뷰로 전달되는 터치 이벤트를 부모 뷰그룹이 가로챌 수 있게 한다. 무작정 가로챈다면 쓸수 있는 자식뷰가 하나도 없을것이다..
따라서 자신에게 필요한 이벤트라 판단되면 가로챈다. 또한 이벤트를 가로채게 되면 자식뷰에서는 ACTION_CANCEL 이라는 이벤트를 onTouchEvent 를 통해 전달해준다.

보통 터치다운의 위치에서 20픽셀 이상 이동되면 이벤트를 가로채서 onIterceptTouchEvent 에 리턴값을 true 로 한다.<br><br>

### requestDisallowInterceptTouchEvent 함수
이 함수는 자식 뷰가 부모 뷰그룹에게 이벤트를 가로채지 않도록 요청한다. 매개변수로 true 를 넘겨주면된다. 단, 한번의 터치 프로세스에만 유효하다.
