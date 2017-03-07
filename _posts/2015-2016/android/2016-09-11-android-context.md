---
layout: post
title: "안드로이드 | Context in action"
published: false
modified:
categories: android
excerpt:
tags: [android, context]
image:
  feature:
date: 2016-9-11
---

## Context ?
[developer.android.com](https://developer.android.com/reference/android/content/Context.html)에 나와있는 내용이다.

>Interface to global information about an application environment. This is an abstract class whose implementation is provided by the Android system. It allows access to application-specific resources and classes, as well as up-calls for application-level operations such as launching activities, broadcasting and receiving intents, etc.

Context 는 어플리케이션 환경에 대한 정보를 가지고 있는 전역 인터페이스이고, abstarct 클래스로 구현객체는 안드로이드 시스템에 의해 만들어진다. Context 는 어플리케이션의 특정한 자원 또는 클래스들에 접근가능하며 또한 Application level의 작업(Activity 실행/Intent 브로드 캐스팅/Intent 수신 등)을 수행하기 위해 호출할수 있다.


## Context in action
Context 에 관련된 글들은 구글에 검색해보면 엄청 많이 나오고, 엄청 복잡하다. 먼가 명확하게 정의 되는것 같지 않고 앱개발할때 어떻게 써야할지도 명확하게 정의가 되지 않는 경우가 많다.
