---
layout: post
title: "안드로이드) 노트3 - 2015.11.02"
modified: 2015-11-19T23:39:55-04:00
categories: android
excerpt:
tags: [android]
image:
  feature:
date: 2015-11-2T23:39:55-04:00
---

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

<br>
<br>
<br>
<br>

#### static 변수를 final 로 할당하는 이유
정적변수로 어떤 값을 할당해놓는 경우가 있다. 여기서 final 로 안해주고 그냥 정적변수로 해주면은 만약 프로세스가 죽으면서 메모리가 날라가게되면 이 정적변수들의 값다 다 null 로 되버릴수가 있다. 따라서 final 로 저장을 해줘야 저런경우에서도 다시 로드가 되더라도, 예외가 발생하지 않고 제대로 동작할수가 있다. 

<br>
<br>
<br>
<br>

#### ScrollView 에서 제일밑에까지 스크롤이 안된다.
스크롤뷰의 하위에 Linear 또는 Relative 를 두고, 이 뷰들의 margin 을 주면 위, 왼쪽, 오른쪽은 잘 먹히는데 marginBottom 은 제대로 먹지를 않는다. 그래서 스크롤이 원하는대까지 밑으로 내려가지 않는 버그가 있다. 구글링결과 안드로이드 버그인걸로 보이고 magin 대신에 padding 을 주어야 될것 같다. 하지만 내가 만든뷰에서는 해당 자식뷰의 배경스타일이 있어 margin 을 padding 으로 바꿀수가 없다. 그래서 약간의 꼼수로 제일밑에 그냥 더미 view 로 height 만있는 투명한 뷰를 만들어 margin 의 역활을 할수 있도록 하였다. 

<br>

<!-- #####  -->

{% highlight java %}
{% endhighlight %}
