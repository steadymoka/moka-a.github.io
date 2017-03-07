---
layout: post
title: "안드로이드 | AlarmManager, in action"
published: true
modified:
categories: android
excerpt: 안드로이드에서 반복적인 알림을 띄우거나, 작업을 해야할경우 AlarmManager 를 써야 합니다. 이번 포스팅에서는 어떻게 알람을 스케쥴링 하는지 자세히 살펴볼것입니다.
tags: [android, alarm, alarmManager]
image:
  feature:
date: 2016-12-04
---
<br>

> 매주 월요일 21:00 에 울리는 알람을 만들어주세요.<br>
매주 토,일 8:00 에 노티를 해주세요.

우리가 앱개발을 하면서 위와 같은 기능들이 필요할수가 있습니다. 단순히 보면, 안드로이드에서는 알람은 `AlarmManager`를 통해 등록만 해주면 되지만, 위와 같이 스케줄링과 함께 구현을 하려면 좀더 추가적인 작업이 필요하게 됩니다.

<br>

### AlarmManagerUtil
AlarmManager 에 특정 시간의 알람을 등록 하는 유틸 클래스를 만듭니다.

```java
public void setOnceAlarm(int hourOfDay, int minute, PendingIntent alarmPendingIntent) {
  if ( Build.VERSION.SDK_INT >= Build.VERSION_CODES.M )
    alarmManager.setExactAndAllowWhileIdle(AlarmManager.RTC_WAKEUP, getTriggerAtMillis(hourOfDay, minute), alarmPendingIntent);
    // alarmManager.setAlarmClock(new AlarmManager.AlarmClockInfo(getTriggerAtMillis(hourOfDay, minute), alarmPendingIntent), alarmPendingIntent);
    // 이전 포스팅 참고
  else if ( Build.VERSION.SDK_INT >= Build.VERSION_CODES.KITKAT )
    alarmManager.setExact(AlarmManager.RTC_WAKEUP, getTriggerAtMillis(hourOfDay, minute), alarmPendingIntent);
  else
    alarmManager.set(AlarmManager.RTC_WAKEUP, getTriggerAtMillis(hourOfDay, minute), alarmPendingIntent);
}



private long getTriggerAtMillis(int hourOfDay, int minute) {
  GregorianCalendar currentCalendar = (GregorianCalendar) GregorianCalendar.getInstance();
  int currentHourOfDay = currentCalendar.get(GregorianCalendar.HOUR_OF_DAY);
  int currentMinute = currentCalendar.get(GregorianCalendar.MINUTE);

  if ( currentHourOfDay < hourOfDay || ( currentHourOfDay == hourOfDay && currentMinute < minute ) )
    return getTimeInMillis(false, hourOfDay, minute);
  else
    return getTimeInMillis(true, hourOfDay, minute);
}



private long getTimeInMillis(boolean tomorrow, int hourOfDay, int minute) {
  GregorianCalendar calendar = (GregorianCalendar) GregorianCalendar.getInstance();

  if ( tomorrow )
    calendar.add(GregorianCalendar.DAY_OF_YEAR, 1);

  calendar.set(GregorianCalendar.HOUR_OF_DAY, hourOfDay);
  calendar.set(GregorianCalendar.MINUTE, minute);
  calendar.set(GregorianCalendar.SECOND, 0);
  calendar.set(GregorianCalendar.MILLISECOND, 0);

  return calendar.getTimeInMillis();
}
```

이 클래스를 통해 alarmManager 에 특정한 시간에 알람을 등록하는데, alarmManager 에 등록할때는 timestamp 형태로 등록을 해야 하기 때문에 약간의 처리 과정이 필요합니다.

기본적으로 받은 시 / 분 을 timestamp 로 바꿔서 등록하는데, 현재시간이 오후 3시인데 오후 1시의 알람을 등록 했다면 내일 오후 1시를 alarmManager 에 등록을 해야 하기 때문입니다. 그래서 현재 시간으로 등록하려는 알람의 시간을 체크하여 분기 합니다.

<br>

### Scheduler
스케줄러 라는 클래스를 만들어 여기서 아까 만들어둔 `AlarmManagerUtil`을 통해 alarm 을 등록 하도록 합니다.
이때 PendingIntent 를 만들어야 하는데, 여기에 Reciever, requestCode 등을 담아서 만듭니다.

여기서 중요한점이 alarmManager 는 requestCode 를 통해 alarm 을 구분하기 때문에 dayInt( 요일을 int 로 표현한 변수 )를 가지고 requestCode 를 만들어야 여러 알람을 등록 할수가 있습니다. 그리고 이 dayInt 를 intent 에 같이 담아서 보내도록 합니다.

```java
fun registerAlarm(dayInt: Int, hourOfDay: Int, minute: Int) {
    AlarmManagerUtil.from(this).setOnceAlarm(hourOfDay, minute, getRepeatingAlarmPendingIntent(dayInt))
}

private fun getRepeatingAlarmPendingIntent(dayInt: Int): PendingIntent {
    val requestCode = dayInt + BASE_REQUEST_CODE
    val intent = Intent(this, AlarmReceiver::class.java)
    intent.putExtra(AlarmReceiver.KEY_DAY_INT, dayInt)

    return PendingIntent.getBroadcast(this, requestCode, intent, PendingIntent.FLAG_UPDATE_CURRENT)
}
```

이런식으로 원하는 요일의 시간을 등록합니다.

<br>

### Reciever / Service
위에서 인탠트에 `AlarmReceiver` 를 넣었기 때문에 원하는 시간이 되면 이 리시버가 불릴것입니다. 여기서는 별로 하는것 없이 바로 Service 를 다시 부르도록 하겠습니다. Service 는 IntentService 로써 다른 쓰레드에서 처리를 하기 위함입니다.

그리고 서비스에서 중요한게 다시 그 울린시간으로 `Scheduler` 를통해 재등록을 하는 것입니다. 앞선 포스팅에서 말했듯이, 최근 버전의 안드로이드에서는 repeat 하는 함수는 없고, 특정 시간에 한번만 울리는 함수만 있기 때문에 매주 반복되기 위해서는 재등록을 해야합니다.

재등록 할때도, 매주 수요일만 필요한 것이라도 일단은 항상(매일) 재등록을 하도록 해야합니다. 그러니까 오늘 원하는 시간에 AlarmManager 가 알람을 주면 내일 다시 울릴수 있도록 재등록을 해야 하고, 그리고 원하는 요일인지를 체크하여 Notification 을 띄운다던지 Activity 를 띄운다던지 하시면 됩니다.

```java
public class AlarmReceiver extends WakefulBroadcastReceiver {

    public static final String KEY_DAY_INT = "AlarmReceiver.KEY_DAY_INT";

    @Override
    public void onReceive(Context context, Intent intent) {
  	int dayInt = intent.getIntExtra(KEY_DAY_INT, -1);

  	if ( -1 != dayInt ) {
  		Intent service = new Intent(context, AlarmService.class);
  		service.putExtra(AlarmService.Companion.getKEY_DAY_INT(), dayInt);
  		startWakefulService(context, service);
  	}
    }
}
```

이때 `WakefulBroadcastReceiver` 를 통하는데 관련해서는 따로 찾아보시기 바랍니다.

```java
class AlarmService : IntentService("AlarmService") {
    var dayInt: Int? = 0

    override fun onHandleIntent(intent: Intent?) {
        dayInt = intent!!.getIntExtra(KEY_DAY_INT, -1)
        // ...
        AlarmScheduler.registerAlarm(this, dayInt, time)
        notifyAlarm()
        WakefulBroadcastReceiver.completeWakefulIntent(intent)
    }

    private fun notifyAlarm() {
        if (dayInt == 1) { // 일요일이라면
           // 필요한 작업 추가
        }
    }

    companion object {
        val KEY_DAY_INT = "AlarmService.KEY_DAY_INT"
    }
}
```

<br>

### BootReceiver
위와 같이 하면 알람이 매주 울리는 알람을 만들수가 있습니다. 하지만 여기서 한가지 또 중요한 사항이 있습니다. 앞선 포스팅에서도 있는데, 휴대폰이 꺼지고나면 alarmManager 의 알람이 모두 사라지게 되어있습니다. 따라서 휴대폰을 켰을때 다시 Alarm 을 등록 해주도록 `BootReceiver` 를 구현해야 합니다.


<br>

### 마무리
하지만 안드로이드에서 Alarm 이 되게 불안하게 등록 될수도 있고 안될수도있습니다. 그래서 구글의 calendar 앱같은것도 보면 앱이 켜질때마다 필요한 알람들을 항상 재 등록 해주기도 합니다.

<br>

### AlarmManager, 포스팅 더보기 / 실제 사용중인 앱
- [안드로이드 - AlarmManager, in Doz mode](http://moka-a.github.io/android/android-alarm/)
- [모닝카페-기상알람,가계부,일기](https://play.google.com/store/apps/details?id=com.moka.earylbird)

<br>
<br>
