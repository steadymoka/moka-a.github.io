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
- public abstract void setDisplayHomeAsUpEnabled (boolean showHomeAsUp)<br> 
액션바 아이콘을 네비게이터로 표시한다.

- Android 4.0 ( API Level 14 ) 부터는 application icon 이 기본으로 action item 으로 작동하지 않는다. 기본으로 작동하게 하려면 setHomeButtonEnabled( true ) 를 호출해주어야 한다.

#### 리사이클러뷰
{% highlight java %}
android:clipToPadding:"false"
{% endhighlight %}
- 리사이클러뷰로 padding 을 줄때, 위 속성을 이용하여 해더나 푸터에 패딩을 주면 좋다.



