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

<br>
<br>
<br>
<br>
#### onReceive
브로드캐스트리시버에서 onReceive 에서 받아서 처리를 할경우 이 함수는 UI 스레드에서 처리되기 때문에, 시간이 오래 걸리는 작업을 하면 안된다. 따라서 별도의 쓰레드를 뺴서 처리를 하거나 IntentService 를 이용하여 처리해야 한다.

<br>
<br>
<br>
<br>
#### IntentService
서비스는 기본적으로 UI 쓰레드에서 동작을 한다. 하지만 IntentService 는 별도의 작업쓰레드에서 처리가 되며, onHandleIntent 에서 처리가 된다. 빠른 시간에 여러번 요청이 오면 병렬처리가 되는게 아니라 스택처럼 직렬로 처리가 된다.
노티피케이션이나 알람받았을때 처리하는 동작을 하면 좋을듯 하다.

<!-- #####  -->

{% highlight java %}
{% endhighlight %}
