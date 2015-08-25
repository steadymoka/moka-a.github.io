---
layout: post
title: "Effective android 01) Single Responsibility"
modified:
categories: android
excerpt:
tags: [android, effective-android]
image:
  feature:
date: 2015-08-14T23:39:55-04:00
---


### 1. Single Responsibility

Each entities should have only one responsibility

Fragment is a layout and it should not contain network calls
Every activity should be responsible only for one thing, ie: registration activity, home page activitiy, splash activity etc
Adapters should not make any network call
Methods should do only one thing
Fragments should not open other fragments nor activity. Fragment should just return the result to the caller activity and let the activity to decide what to do next



### 1. 단일책임

각각의 객체들은 하나의 책임을 지어야 한다.

프레그먼트는 레이아웃이고, 네트워크 통신을 포함하면 안된다.
모든 액티비티는 오직 하나의 역활을 해야한다. ( 회원가입 액티비티, 홈 액티비티, 스플레쉬 액티비티 등 )
어댑터는 네트워크 요청을 할수없다.
메소드는 오직 하나의 일만 해야한다.
프레그먼트들은 다른 프레그먼트나 액티티를 열수없다. 프레그먼트는 단지 호출하는 액티비티의 결과를 리턴하고, 액티비티에게 다음 작업이 먼지 결정하게 한다. 

[출처][http://orhanobut.github.io/effective-android/](http://orhanobut.github.io/effective-android/)