---
layout: post
title: "Effective android 02) Open/Closed Principle"
modified:
categories: android
excerpt:
tags: [android, effective-android]
image:
  feature:
date: 2015-10-29T23:39:55-04:00
---

### 2. Open/Closed Principle

A class should be closed to modification but open to extend
Design your class extendable for the new additions. Create base classes and use them. BaseActivity, BasaeFragment etc
Don't put everything in one class and modify it all the time.


### 2. Open/Closed Principle

클래스는 수정가능성에 대해 닫혀있어야 한다. 하지만 확장가능성에는 열려있어야 한다. 새로운 추가 기능을 확장하기 쉬워야 한다.
기본 클래스들을 만들고 그것들을 사용하라. BaseActivity, BaseFragment 등등 을 만들어라.
계속 해서 수정해야하는 하나의 클래스에 모든것을 넣지 말아야 한다.


### --
프레그먼트나, 액티비티를 사용할경우 BaseActivity 와 BaseFragment 를 만들어 사용하라는 얘기이다.
실제 현 프로젝트에 이렇게 사용중이긴하다. 하지만 내가 안드로이드를 처음 시작할때 공부했던 병아리라는 수퍼개발자가 이미 그렇게 사용하고 있어서 자연스럽게 원래 이렇게 쓰는거구나 라고 생각 했다.
이번에 이 글을 보면서 굳이 그렇게 한 이유를 좀더 제대로 알아야 겠다고 생각된다. 현 프로젝트에는 사실 BaseActivity 에는 별게 없긴하다. LocalyTics 에 연결되어 이벤트 보내는 함수, 생명주기에 따라 running 플레그 값을 두어 액티비티가 running 중인지 체크하는 함수가 있다. BaseFragment 에는 showToast 함수, showLoadingDialog 함수, 마찬가지로 로컬리틱스에 이벤트보내는 함수, 그리고 화면 꺼짐, 켜짐 유지 설정을 하는 함수가 있다.
확실히 로딩다이얼 로그나 토스트 띄우는건 많이 쓰긴한다. ( 사실 토스트띄우는 것은 Util 클래스로 해놓는게 더 좋을듯 하다 ),
이펙티브 안드로이드에 저렇게만 설명이 아니라, 예를 들어 어떻게 사용하면 좋을지 좀더 설명 해줬으면 더 좋았겠다는 생각을 한다. 


[출말][http://orhanobut.github.io/effective-android/](http://orhanobut.github.io/effective-android/)