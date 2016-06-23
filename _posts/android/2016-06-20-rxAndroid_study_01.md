---
layout: post
title: "RxAndroid ConnectableObservable 과 Thread"
modified:
categories: android
excerpt:
tags: [android, rxAndroid, rxJava]
image:
  feature:
date: 2016-06-19
---
<p style="font-size: 16px">
Slack 의 android-rxjava 채널에서 steve 님의 고민.. 스터디에서 gaemi 님의 Hot / Cold Observable 에 대해 .. 이해 하기 위한 과정으로 두번째 포스팅 입니다. 이번 포스팅은 ConnectableObservable 에 subscribeOn() 과 observeOn() 을 달았을때, 구독 과 소비의 순서에 관한 것입니다.
</p>
<hr>

# ConnectableObservable 과 쓰레드

저번 [포스팅](http://moka-a.github.io/android/rxAndroid_study/)에서 ConnectableObservable 에 대해서 살펴 보았는데, 이때 살펴본 코드들은 따로 쓰레드 설정을 하지 않고 테스트를 진행했었습니다. 아무튼 이제 ConnectableObservable 에 대해 조금 이해를 한후 gdg-slack 채널 android-rxjava 에서 nobrain_steve 님이 고민하시던 내용이 당시에 이해가 잘 안갔었는데, 이제는 조금 이해가지 않을까 하며 다시 살펴 보았습니다. 주된 고민사항은 ConnectableObservable 에 두개의 subscriber 가 구독하고 있을때 `item 이 소비되는 순서` 와 `item 이 emit되는 순서` 에 관한 고민입니다. 

### 첫번째 테스트

```java
Observable<String> observable = Observable
        .range(0, 3)
        .timestamp()
        .map(timestamped -> {

            Log.d(TAG, "_____________연산__________ Thread : " + Thread.currentThread());
            return String.format("[%d] %d", timestamped.getValue(), timestamped.getTimestampMillis());
        })
        .doOnNext(value -> count.increase())
        .subscribeOn(Schedulers.io()).publish().refCount();

observable.subscribe(value -> {
    try {
        Thread.sleep(700);
        Log.d(TAG, "subscriber1 : " + value + " Thread : " + Thread.currentThread());
    }
    catch ( InterruptedException e ) {
        e.printStackTrace();
    }
});

observable.subscribe(value -> {
    try {
        Thread.sleep(10);
        Log.d(TAG, "subscriber2 : " + value + " Thread : " + Thread.currentThread());
    }
    catch ( InterruptedException e ) {
        e.printStackTrace();
    }
});

Log.d(TAG, "연산횟수 : " + count.count());

/** 결과
    _____________연산__________ Thread : Thread[RxIoScheduler-2,5,main]
    subscriber1 : [0] 1466663377575 Thread : Thread[RxIoScheduler-2,5,main]
    subscriber2 : [0] 1466663377575 Thread : Thread[RxIoScheduler-2,5,main]
    _____________연산__________ Thread : Thread[RxIoScheduler-2,5,main]
    subscriber1 : [1] 1466663378287 Thread : Thread[RxIoScheduler-2,5,main]
    subscriber2 : [1] 1466663378287 Thread : Thread[RxIoScheduler-2,5,main]
    _____________연산__________  Thread : Thread[RxIoScheduler-2,5,main]
    subscriber1 : [2] 1466663378998 Thread : Thread[RxIoScheduler-2,5,main]
    subscriber2 : [2] 1466663378998 Thread : Thread[RxIoScheduler-2,5,main]
*/
```

먼저 subscribeOn(Schedulers.io()) 인 ConnectableObservable 을 만들고 거기에 두개의 subscriber 가 구독 하고 있습니다. 두개의 subscriber 는 osbserveOn() 을 안썼습니다. 이렇게 되면 아이템을 소비하는 쓰레드가 subscribeOn() 에서 설정해준 쓰레드와 동일하게 처리가 됩니다. 즉, item 을 emit 하는 쓰레드와 각각(2개)의 subscriber 가 그 item 을 소비하는 쓰레드가 모두 동일하다는 얘기 입니다. 

rx-java 에서 Schedulers.io() 를 사용하면 쓰레드풀을 이용하여 처리를 하는것이기 때문에 어떤 쓰레드로 들어갈지 모르게 됩니다. 하지만 위와 같이 subscribeOn 만 설정하면 위에서 말했듯이 emit 과 소비하는 쓰레드가 정확하게 동일하게 됩니다. 쓰레드풀 안에서도 하나의 동일한 쓰레드에서 처리된다는 말입니다. 따라서 item 1 을 emit 하고 첫번째 subscriber 가 소비하고 두번째 subscriber 가 소비한후 두번째 item 이 emit 이 됩니다. 그래서 만약 첫번째 subscriber 가 소바하는 시간이 길어진다면 끝날때까지 기다렸다가 2번째 subscriber 가 소비하고, 그것이 끝나야 다음 item 이 emit 될수 있는 것입니다. 위의 결과를 보면 알수 있습니다. 

<img src="/images/rx-1.png">
위가 이것을 나타내고 있는 그림입니다. 

### 두번째 테스트

```java
Observable<String> observable = Observable
        .range(0, 3)
        .timestamp()
        .map(timestamped -> {

            Log.d(TAG, "_____________연산__________ Thread : " + Thread.currentThread());
            return String.format("[%d] %d", timestamped.getValue(), timestamped.getTimestampMillis());
        })
        .doOnNext(value -> count.increase())
        .subscribeOn(Schedulers.io()).publish().refCount();

observable.observeOn(Schedulers.io()).subscribe(value -> {
    try {
        Thread.sleep(700);
        Log.d(TAG, "subscriber1 : " + value + " Thread : " + Thread.currentThread());
    }
    catch ( InterruptedException e ) {
        e.printStackTrace();
    }
});

observable.observeOn(Schedulers.io()).subscribe(value -> {
    try {
        Thread.sleep(10);
        Log.d(TAG, "subscriber2 : " + value + " Thread : " + Thread.currentThread());
    }
    catch ( InterruptedException e ) {
        e.printStackTrace();
    }
});

Log.d(TAG, "연산횟수 : " + count.count());

/** 결과
    _____________연산__________ Thread : Thread[RxIoScheduler-3,5,main]
    _____________연산__________ Thread : Thread[RxIoScheduler-3,5,main]
    _____________연산__________  Thread : Thread[RxIoScheduler-3,5,main]
    subscriber2 : [0] 1466663377575 Thread : Thread[RxIoScheduler-4,5,main]
    subscriber2 : [1] 1466663378287 Thread : Thread[RxIoScheduler-4,5,main]
    subscriber2 : [2] 1466663378998 Thread : Thread[RxIoScheduler-4,5,main]
    subscriber1 : [0] 1466663377575 Thread : Thread[RxIoScheduler-2,5,main]
    subscriber1 : [1] 1466663378287 Thread : Thread[RxIoScheduler-2,5,main]
    subscriber1 : [2] 1466663378998 Thread : Thread[RxIoScheduler-2,5,main]
*/
```

각각의 subscriber 에 observeOn(Schedulers.io()) 를 달았습니다. 이렇게 되면 emit, 각각 소비하는 쓰레드가 모두 달라진걸 볼수 있고, 각자의 쓰레드에서 각각 처리되는것을 볼수 있습니다. 따라서 item 을 소비하는데 subscriber2 가 10ms 고 subscriber1 이 700ms 걸리기 때문에, subscriber2 가 먼저 전부다 소비하는 모습을 볼수있습니다. 

### 세번째 테스트

```java
Observable<String> observable = Observable
        .range(0, 3)
        .timestamp()
        .map(timestamped -> {

            Log.d(TAG, "_____________연산__________ Thread : " + Thread.currentThread());
            return String.format("[%d] %d", timestamped.getValue(), timestamped.getTimestampMillis());
        })
        .doOnNext(value -> count.increase())
        .subscribeOn(Schedulers.io()).publish().refCount();

observable.observeOn(AndroidSchedulers.mainThread()).subscribe(value -> {
    try {
        Thread.sleep(700);
        Log.d(TAG, "subscriber1 : " + value + " Thread : " + Thread.currentThread());
    }
    catch ( InterruptedException e ) {
        e.printStackTrace();
    }
});

observable.observeOn(AndroidSchedulers.mainThread()).subscribe(value -> {
    try {
        Thread.sleep(10);
        Log.d(TAG, "subscriber2 : " + value + " Thread : " + Thread.currentThread());
    }
    catch ( InterruptedException e ) {
        e.printStackTrace();
    }
});

Log.d(TAG, "연산횟수 : " + count.count());

/** 결과
    _____________연산__________ Thread : Thread[RxIoScheduler-3,5,main]
    _____________연산__________ Thread : Thread[RxIoScheduler-3,5,main]
    _____________연산__________  Thread : Thread[RxIoScheduler-3,5,main]
    subscriber1 : [0] 1466663377575 Thread : Thread[main,5,main]
    subscriber1 : [1] 1466663378287 Thread : Thread[main,5,main]
    subscriber1 : [2] 1466663378998 Thread : Thread[main,5,main]
    subscriber2 : [0] 1466663377575 Thread : Thread[main,5,main]
    subscriber2 : [1] 1466663378287 Thread : Thread[main,5,main]
    subscriber2 : [2] 1466663378998 Thread : Thread[main,5,main]
*/
```

이번에는 emit 되는 쓰레드는 io 쓰레드로, 각 subscriber 가 소비하는 쓰레드를 Android Main 쓰레드로 설정 하였습니다. 보시면 emit 과 소비는 별 상관관계 없이 처리되는 것으로 보이고, 같은 쓰레드에서 소비되고 있는 부분에서 첫번째 subscriber 가 700ms 로 오래 걸림에도 불구하고 모두 다 소비하고 난다음에 두번째 subscriber 가 소비하는 모습을 볼수 있습니다. 

이것으로 보았을때, 만약 같은 쓰레드에서 여러 subscriber 가 구독을 하여 소비를 하고 있다고 하면 구독을 요청한 순서대로 emit 된 모든 아이템들을 순서대로 소비한다고 볼수있습니다. 위의 코드에서는 첫번째 subscriber 가 1을 소비하기도 전에 1,2,3 이 emit 되어있는 상태이고 이 emit 된 모든 아이템을 소비해야 두번째 subscriber 가 소비하는 모습입니다. 



