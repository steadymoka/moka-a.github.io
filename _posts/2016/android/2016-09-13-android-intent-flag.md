---
layout: post
title: "안드로이드 | Intent Flag 의 함정"
published: false
modified:
categories: android
excerpt: 앱개발 하다각 flag 값을 어떻게 설정을 하니 activity 가 불리지 않는 현상이 발생하였고, 로그를 남기려고 한다.
tags: [android, alarm, alarmManager]
image:
  feature:
date: 2016-11-02
---
<br>

FLAG_ACTIVITY_CLEAR_TASK 을 한 상태로 FLAG_ACTIVITY_NEW_TASK 로 WakeUpActivity.class 를 start 하면 MainActivity 로 되있는 PendingIntent 도 열리지 않고 WakeUpActivity 가열린다.






<br>
<br>
