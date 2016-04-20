---
layout: post
title: "안드로이드) 노트3 - 2016.03.14"
modified:
categories: android
excerpt:
tags: [android, Service]
image:
  feature:
date: 2016-02-24T19:39:55-04:00
---

### Service ( startServer()  ->  onStartCommend() )
백그라운드(? -> 화면에 보이지 않는 작업, 비동기 처리가 아니다)에서 실행해야할경우 사용
-> 서비스를 호출한 액티비티(프레그먼트)가 죽더라도 서비스는 무기한 살아있을수 있다.
-> 서비스 내에서 stopSelf() 또는 다른 컴포넌트에서 stopService() 호출되기 전까지 살아있다

메인쓰레드에서 실행된다 -> 시간이 오래걸리는 작업 하면 안된다
-> 작업 쓰레드를 만들어서 쓰레드를 관리하는 용도로 쓰는 방향으로 사용


onCreate
- intent 를 이용해서 startService 를 할때 호출된다.
- 이때 이미 서비스가 한번 실행되었으면 호출되지 않고 onStartCommend() 가 호출된다

onStartCommend
- return 값 
    + START_NOT_STICKY :
