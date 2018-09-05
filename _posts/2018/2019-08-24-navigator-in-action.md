---
layout: post
title: "Android Navigation(Jetpack) in use"
author:
published: true
modified:
categories: android
excerpt: Android Navigation
tags: [android, jetpack]
image:
  feature:
date: 2018-8-23
---

## Android Navigation in use
Navigation 에 대한 기본적인 내용과 구현은 앞선 포스팅을 참고 하시기 바랍니다. [https://moka-a.github.io/android/navigator](https://moka-a.github.io/android/navigator/) 
아래 내용은 Navigation 을 프로젝트에 적용하면서 경험한 내용을 공유 합니다.

<br>
### 프로젝트 구조
Navigation 을 적용한 프로젝트는 ['하루하루-일기'](https://play.google.com/store/apps/details?id=io.moka.dayday_diary) 어플 이다. 구조는 왼쪽 Drawer 메뉴가 있고, 메인 화면, 상세화면 으로 이루어진 일반적인 어플리케이션 이다. 
<figure>
	<img src="/images/app-structure.jpg" alt="image">
</figure>


<br>
### Navigation 적용 - Single Activity? or Multi Activities?
Navigation 에 대한 가이드를 읽어 보면, 구글은 하나의 Activity 를 권장하고 있다. 하지만 기존 프로젝트 구조가 Single Activity 가 아니었고, 점진적으로 적용해보기로 하고 여러 Activity 구조를 그대로 가지고 가면서 Navigation 을 적용하기로 하였다. 

그리고 여러 Activity 로 하기로 한 또하나의 이유가 있는데, 메인화면에서 드로워 메뉴를 구성을 한후 어떤 메뉴를 들어갈 떄, 최상단에 화면이 들어오는 구조에 있어서는 하나의 Activity 로 하기에는 문제점이 있어 여러 Activity 로 진행하기로 하였다. 

``` xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.drawerlayout.widget.DrawerLayout
	xmlns:android="http://schemas.android.com/apk/res/android"
	xmlns:app="http://schemas.android.com/apk/res-auto"
	android:id="@+id/drawer_layout"
	android:layout_width="match_parent"
	android:layout_height="match_parent">

	<fragment
		android:id="@+id/fragment_host"
		android:name="androidx.navigation.fragment.NavHostFragment"
		android:layout_width="match_parent"
		android:layout_height="match_parent"
		app:defaultNavHost="true"
		app:navGraph="@navigation/main_nav_graph"/>

	<fragment
		android:id="@+id/navigation_drawer"
		android:name="io.moka.dayday_diary.ui.drawer.DrawerLayout"
		android:layout_width="320dp"
		android:layout_height="match_parent"
		android:layout_gravity="left"/>

</androidx.drawerlayout.widget.DrawerLayout>
```
`DrawerLayout` 을 최상단에 두고, `HostFragment` 를 위와 같이 둔다면, 왼쪽 드로워메뉴가 보일 때는 `HostFragment` 상단(z축으로 위)에 드로워메뉴가 보이게 될 것이다. 이때 특정 메뉴를 눌러 상세화면으로 들어가는 것을 Navigation 컴포넌트를 이용한다면 `HostFragment` 자리에 해당 화면이 들어가게 될 것이고, 드로워메뉴 상단으로 상세화면이 보이는 형태가 필요한 저는 이부분에서 고민을 하다가 일단은 여러 Activity 로 진행하기로 하였다. 

하지만 단점으로.. 여러 Activity 를 그대로 유지하면서 진행하게 된다면, 굳이 Navigation Component 를 써야 하나 하는 생각이 들수도 있다. Activity 마다 새로 nav_graph 를 만들어 줘야 할 수도 있기 때문이다.

<br>
### Pending Intent 
해당 앱에는 일기/기분일기 를 쓰는 화면이 바로 뜨는 위젯과, 일기를 쓸시간을 알려주는 노티가 뜨고 있다. 이 두부분을 개발 하려면 Pending Intent 가 필요하다. 이부분은 Google 의 Navigation Code Lab 부분을 참고하여 개발 하였다. 

``` java
fun getWriteDiaryPendingIntent(): PendingIntent {
    return NavDeepLinkBuilder(context)
            .setComponentName(MainNav::class.java)
            .setGraph(R.navigation.main_nav_graph)
            .setDestination(R.id.writeDiaryNav)
            .createPendingIntent()
}

fun getWriteMoodPendingIntent(): PendingIntent {
    return NavDeepLinkBuilder(context)
            .setComponentName(MainNav::class.java)
            .setGraph(R.navigation.main_nav_graph)
            .setDestination(R.id.writeMoodLayout)
            .createPendingIntent()
}
```
위와 같이 하면 Pending Intent 를 만들수 있는데, 여기서 좀 많은 삽질을 한 부분이 있다. 현재 앱의 구조로 처음 앱이 뜰때 `splash_nav -> main_nav` (SplashActivity -> MainActivity) 로 이루어 져있는데, `setComponent(MainNav::class.java)` 이부분을 해주지 않으면, MainNav 에서 내가 원하는 Destination 으로 navigation 이 되지 않는 문제가 있었다. 따라서 원하는 Destination 이 잘 동작하기 위해서는 해당 Destination 이 올라가 있는 Activity 를 componentName 으로 지정 해줘야지 정상 작동 할수 있었다.

<br>
### Transition 
화면 전환에서 원하는 Transition 이 있어 가이드대로 구현을 했는데, Transition 이 동작을 안하는 경우가 발생 하였다. 이는, navigation 하는 코드가 Main Thread 가 아닌곳에서 실행이 되고 있을 가능성이 있다. 그래도 화면 전환은 되는데 transition animation 은 먹히지 않는다.

<br>
### 알아봐야할 부분들
우선은 이렇에 하여 기본적으로 적용하였다. 구글 Navigation 문서를 보면 여러 Activity 마이그레이션 하는 가이드가 나와있는데, 해당 부분에 대한 설명이 구체적으로 나와있지 않아 직접 삽질을 해가면 적용해봐야 할 부분인것 같다. Navigation graph 를 보면 nested nav graph 를 할수 있는데, 이부분이 여러 Activity 를 Single Activity 구조로 마이그레이션 하는 과정이지 않을까 한다. 그래서 이부분에 대해서 좀더 진행해볼 예정이다.

또한 Navigation 을 어떤식으로 구현을 했는지 내부적으로 좀 살펴보며 좀더 이해를 구하는 작업을 해야 겠다는 생각이 든다.

<a href="podfreeca://home?index=3">TEST TEST</a>
<br>
<br>

