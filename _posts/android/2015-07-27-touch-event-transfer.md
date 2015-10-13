---
layout: post
title: "안드로이드 이벤트 전달"
modified:
categories: android
excerpt:
tags: [android, event, 이벤트]
image:
  feature:
date: 2015-07-27T19:39:55-04:00
---
### 터치이벤트의 종류
다운 이벤트, 이동 이벤트, 업 이벤트  -> 순서대로 동작하며, 이동이벤트는 생략될수 있다. <br>

디바이스의 터치 정보는 시스템 서비스인 윈도우 매니저에 전달이 되고, 위도우 매니저는 화면에 떠있는 앱에 최종 전달한다.
이까지의 과정은 궂이 알필요 없다. <br>

앱에서 터치 이벤트는 최초 액티비티를 통해 최초 전달되며, 액티비티의 두함수를 재정의하여 컨트롤 할수있다. 정의하지 않으면 화면에 배치된 각종 뷰에 순차적으로 전달 된다.

{% highlight java %}
dispatchTouchEvent 
onTouchEvent 
{% endhighlight %}

화면터치하면 먼저 dispatchTouchEvent 가 호출되고, 이어서 onTouchEvent 가 호출이 된다

이벤트의 정보에는 getX( 이벤트가 발생한 x축 위치 ), getY, getAction, getDownTime( down 이벤트가 발생한 시간 millis ), 등이 표시된다.

이벤트는 가장 최산단의 뷰( 액티비티 )부터 아례로 계층구조로 전달된다.

