---
layout: post
title: "안드로이드) 노트3 - 2015.11.02"
modified:
categories: android
excerpt:
tags: [android]
image:
  feature:
date: 2015-11-2T23:39:55-04:00
---
<br>
<br>

#### Activity Life Cycle 다시보기 
OnCreate() - 최초생성될때 호출, UI 구성 ( setContentView() )<br>
OnStart() - 거의 사용x<br>
OnReStart() - 거의 사용x<br>
OnResume() - 실행상태 전에 호출 보장 <br>
OnPause() - 종료시점의 호출 보장( 종료시 꼭 해야할 처리 )<br>
OnStop() - 거의 사용x<br>
OnDestroy() - 거의 사용x<br>

<!-- #####  -->

{% highlight java %}
{% endhighlight %}
