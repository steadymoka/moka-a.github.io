---
layout: post
title: "안드로이드 | AlarmManager, in Doz mode"
published: true
modified:
categories: android
excerpt:
tags: [android, alarm, alarmManager]
image:
  feature:
date: 2016-9-13
---
안드로이드 마쉬멜로버전이 나오면서 도즈모드가 추가되었다. 도즈모드는 휴대폰을 사용 안할때 배터리 절약을 위해 최소한의 동작만을 하며 나머지 불필요한 백그라운드 작업은 모두 정지 시키는 기능이다.

- 네트워크 작업 불가능
- JobScheduler, AlarmManager, etc..
- Wake Lock 무시
- 등등..


여러 기능들이 막히게 된다. 하지만 GCM 을 통해서는 푸쉬를 받을수 있다고 한다.

하지만 알람앱을 GCM 을 통해 울리게 할수는 없을 것이다. GCM 도 어쨋든 네트워크 연결이 되어 있어야 되기 때문이다. 따라서 도즈 모드로 알람앱을 만들경우 어떻게 해야 하는지 살펴 봐야 한다.


<br>

## Scheduling repeating alarm
[https://developer.android.com/training/scheduling/alarms.html](https://developer.android.com/training/scheduling/alarms.html)
공식 사이트에서는 `AlarmManager` 를 이용을 하기를 권장하며, 앱이 실행되지 않은상태에서도 동작할수 있고, 불필요하게 리소스를 잡아먹는것을 방지 할수이 있다고 한다.

그러나 데이터를 동기화 하기위한 반복 작업에서는 `AlarmManager` 대신에 GCM 과 `SyncAdapter` 를 사용하기를 권하고 있다. `SyncAdapter` 는 `AlarmManager` 와 같은 스케쥴링 옵션을 제공하며 좀더 유연하게 컨트롤 할수 있다.

아무튼 반복작업을 위한 `AlarmManager` 의 함수로는 우선은 두가지가 있다. `setInExectRepeating()` 과 `setRepeating()` 함수이다. 후자의 함수가 정확한 시간에 알림을 주기 위한 함수이지만, 구글에서는 알람시계같은 반복 알람이 아니라면 전자의 함수를 이용하기를 권하고 있다. 전자의 함수가 배터리 소모를 좀더 줄일수 있고, 네트워크 통신을 한다면 여러앱에서 동시 다발적으로 일어나는 통신으로 인한 부하를 없앨수 있기 때문이다.

그리고 주의사항으로, 폰이 꺼지면 등록된 AlarmManager 가 모두 초기화 된다. 따라서 BootReceiver 에서 트리거를 받아 AlarmManager 를 등록 해주어야 한다.

<br>

## API level 19
하지만 안드로이드 **api level 19 (KITKAT 4.4)** 이상부터는 `setRepeating()` 으로 지정해도 알람이 정확하게 오지 않는다. `setExact()` 함수를 이용해야지 정확한 시간에 알람이 오게 되어있다.

<br>

## API level 23
그리고 안드로이드 **api level 23 (Marshmallow 6.0)** 이상부터는 위에서 언급했듯이 Doz 모드가 추가되어 `setExact()` 로도 정확한 시간을 보장할수가 없다. 따라서 새로 추가된 함수 `setExactAndAllowWhileIdle()` 를 이용하여 알람을 설정 해주어야 한다. 참고로 정확한 시간의 알람이 아니라면 `setAndAllowWhileIdle()` 함수를 이용하면 된다.

따라서 알람앱을 만들기 위해서는 api level 을 분기하여 각각에 맞는 함수를 이용해서 처리를 해주어야 한다. 이렇게 잘해주더라도 AlarmManager 가 내가 생각 했던거와는 다르게 동작하는 경우가 생길수 있다. 테스트할대는 잘되다가 꼭 한번씩 안될때가 있는데, 이럴때 디버깅을 하기 위해 adb 의 덤프를 보는 방법이 있다.  `adb shell dumpsys alarm` 명령을 치면 현재 Alarmmanager 에 등록된 알람을 모두 볼수있다. 보고자 하는 앱의 패키지명으로 검색 하여 등록된 시간을 확인하며 디버깅을 할수있다.

### setAlarmClock()
그러면 이렇게 `setExactAndAllowWhileIdle()` 함수를 이용하여 알람을 설정 해주고 `adb` 명령어를 이용해 테스트를 해보면, 위의 함수로만은 가끔 정확한 시간에 울리지 않는것을 확인할수가 있다. 휴대폰이 도즈모드 이다가, 다시 사용을 할때 그때 해당 시간에 받지 못한 알람이 울리는 경우가 발생하였다. 따라서 알람앱 같은 정확한 시간에 울려야 하는 앱이라면은 다른 함수 `setAlarmClock()` 을 이용하면 해결을 할수가 있다. 하지만 위의 함수는 알람이 울리기 전에 도즈모드를 풀어버리고 울리는 방식이라 GDG 블로그에 보면 되도록이면 쓰지말라고 되어있다.
