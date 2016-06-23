---
layout: post
title: "RxAndroid Hot & Cold Observable"
modified:
categories: android
excerpt:
tags: [android, rxAndroid, rxJava]
image:
  feature:
date: 2016-06-19
---
<p style="font-size: 16px">
RxJava 스터디에서 Hot / Cold Observable 에 관해서 얘기를 했었는데, 공식문서와 각종 블로그 자료들을 보며, 좀더 자세하게 알아보고 이해해보자. 간단하게는 Hot 은 즉시 소모..  Cold 는 모아서 소모.. 인 느낌 !?
</p>
<hr>

# ConnectableOb.. & Observable / Hot & Cold

[http://reactivex.io/documentation/observable.html](http://reactivex.io/documentation/observable.html) <br>
“Hot” and “Cold” Observables 에 관하여 공식 사이트를 보면 다음과 같이 나와있다. 

>When does an Observable begin emitting its sequence of items? It depends on the Observable. A “hot” Observable may begin emitting items as soon as it is created, and so any observer who later subscribes to that Observable may start observing the sequence somewhere in the middle. A “cold” Observable, on the other hand, waits until an observer subscribes to it before it begins to emit items, and so such an observer is guaranteed to see the whole sequence from the beginning.

'언제 아이템들이 Emit 되는가' 는 Observable 에 따라 다릅니다. "Hot" 이면 만들어 졌을때 emit 되기 시작하고, 나중에 구독하는 Observer 는 중간에 아무대나 관찰을 시작 할수 있습니다. "Cold" 는 반대로 Observer 가 구독시작할때까지 기다리고, 구독시작할때 아이템들을 emit 하기 시작합니다. 따라서 Observer 가 모든 item 을 구독할수 있는 보장이 됩니다. 

>In some implementations of ReactiveX, there is also something called a “Connectable” Observable. Such an Observable does not begin emitting items until its Connect method is called, whether or not any observers have subscribed to it. 

ReactiveX 에는 "Connectable" Observable 이 있습니다. observer 가 구독을 하던말던 상관없이 connect 함수가 불리기 전엔 아이템을 emit 하지 않습니다.

<br><br>

## 그런데 ..

보통 `ConnectableObservable` 을 `Hot Observable` 라 부르고, 일반적으로 많이 쓰는 `Observable` 이 `Cold` 라 부릅니다. 그리고 `Observable` 을 확장한것이 `ConnectableObservable` 입니다. Observable 의 publish() 함수를 이용해서 `ConnectableObservable을` 만들수 있습니다. 

Observable 은 자동적으로 emit 이 되고, ConnectableObservable 은 수동적으로 됩니다. 즉 ConnectableObservable 은 트리거( connet 함수 )를 호출해야 emit 이 되기 시작 합니다. 그러면 위에서 정의했던 Hot 와 Cold 의 의미와는 대응이되지 않는듯 합니다. Hot / Cold 와 ConnectableObservable / Observable 을 그냥 따로 보는게 맞는듯 합니다. 

[http://www.java-allandsundry.com/2015/03/hot-and-cold-rx-java-observable.html](http://www.java-allandsundry.com/2015/03/hot-and-cold-rx-java-observable.html) 하지만 이런 링크들을 보면 Hot 을 ConnectableObservable 로 Cold 를 Observable 로 보고 있습니다. 

<br>

# ConnectableObservable 에 대해서 

### publish()
Observable 객체의 `publish()` 함수를 호출해주면 ConnectableObservable 를 반환합니다. 
`publish()`는 operator 한 종류로 보면 됩니다. ConnectableObservable 의 큰 특징은 subscribe 해주는것과 상관없이 connect 를 해주어야 item 을 emit 하기 시작합니다. ( 그냥 Observable 은 누군가가 subscribe 를 하는 순간부터 emit 을 한다. ) 그리고 emit 된 item 을 소비하는 구독자들은 여러개가 붙어 있더라도 한번의 연산만하고 거의 동시에 같은 값을 소비하게 됩니다. 따라서 connect 의 시점이 매우 중요하다고 볼수 있습니다.

### refCount()
> make a Connectable Observable behave like an ordinary Observable

ConnectableObservable 의 함수이고, ConnectableObservable 을 Observable 로 바꿔주는 역활을 한다. ConnectableObservable 은 connect 를 해주어야 emit 을 시작 하지만, refCount 를 통해 만들어진 Observable 은 기존 Observable 처럼 누군가 subscribe 시작할때 connect 되어 emit 을 시작합니다. 그리고 모든 구독자가 unsubscribe 하면 Observable 도 unsubscribe 됩니다. 

#### 그래서 refCount() 어떻게 써야하는가 ...

위의 설명은 저런데, refCount() 를 쓰는 목적은 publish() 을 통해서 얻어지는 이점 즉 multicast 의 기능을 쓰는데, connect 를 해주어야 하는 것을 안써주기 위함일 것입니다.

아래 코드를 보며 좀더 자세히 알아보겠습니다.

```java 
// 개미님이 쓰신 코드를 참고하였습니다. 
ConnectableObservable<String> observable = Observable
    .range(0, 4)
    .timestamp()
    .map(timestamped -> {
        System.out.println("\n________________map 연산________________\n");
        return String.format("[%d] %d", timestamped.getValue(), timestamped.getTimestampMillis());
    })
    .doOnNext(value -> count.increase())
    .publish();

observable.subscribe(value -> {
    System.out.println("subscriber1 : " + value);
});

observable.subscribe(value -> {
    System.out.println("subscriber2 : " + value);
});

observable.connect();
System.out.println("연산횟수 : " + count.count());        

/** 결과 
    ________________map 연산________________

    subscriber1 : [0] 1466580780476
    subscriber2 : [0] 1466580780476

    ________________map 연산________________

    subscriber1 : [1] 1466580780476
    subscriber2 : [1] 1466580780476

    ________________map 연산________________

    subscriber1 : [2] 1466580780477
    subscriber2 : [2] 1466580780477

    ________________map 연산________________

    subscriber1 : [3] 1466580780477
    subscriber2 : [3] 1466580780477
    연산횟수 : 4
*/
```

pulish() 를 이용해 ConnectableObservable 을 만들어 사용하면 위의 결과와 같이 multicast 되어 한번의 연산으로 모든 구독자들에게 emit 될 값을 보낼수가 있습니다. 여기서 refCount() 를 써보겠습니다. 단순히 refCount() 를 호출하고 connect 를 없애도 위와같이 내가 원하는 multicast 가 될거라고 예상했었는데, 그게 아니었습니다. 우선 코드를 보겠습니다.

```java
Observable<String> observable = Observable
    .range(0, 4)
    .timestamp()
    .map(timestamped -> {
        System.out.println("\n________________map 연산________________\n");
        return String.format("[%d] %d", timestamped.getValue(), timestamped.getTimestampMillis());
    })
    .doOnNext(value -> count.increase())
    .publish().refCount();

observable.subscribe(value -> {
    System.out.println("subscriber1 : " + value);
});
System.out.println("*******************************************************");
observable.subscribe(value -> {
    System.out.println("subscriber2 : " + value);
});

System.out.println("연산횟수 : " + count.count());  

/** 결과 
    ________________map 연산________________

    subscriber1 : [0] 1466581248156

    ________________map 연산________________

    subscriber1 : [1] 1466581248157

    ________________map 연산________________

    subscriber1 : [2] 1466581248157

    ________________map 연산________________

    subscriber1 : [3] 1466581248157
    *****************************************
    ________________map 연산________________

    subscriber2 : [0] 1466581248158

    ________________map 연산________________

    subscriber2 : [1] 1466581248158

    ________________map 연산________________

    subscriber2 : [2] 1466581248159

    ________________map 연산________________

    subscriber2 : [3] 1466581248159
    연산횟수 : 8
*/
```

위와 같이 결과는 전혀 multicast 가 되지 않았습니다. 사실 publish().refCount() 를 빼고 쓴것과 동일한 결과입니다. 이유는 refCount 로 만들어진 Observable 에 처음 구독자가 subscribe 를 하게 됨으로써 connect 가 된것인데, 모든 item 이 emit 되고 난후 두번째 구독자가 구독이 되기 때문입니다. 
위의 ** 이 찍힌 위치를 보면 알수 있습니다. 따라서 첫번째 구독자가 구독을 다하고, 두번째 구독자가 다시 새로운 스트림에 구독을 하게 됨으로써 얻고자했던 multicast 의 이점을 얻을수가 없습니다. 

그러면 도대체 refCount() 는 어디서 어떻게 사용해야 될지 의문이 듭니다. 

##### 쓰레드를 조절해서 ..

첫번째로는 emit 하는 쓰레드를 바꿔주는 방법이 있습니다. subscribeOn() 을 이용해서, subscribe 하는 쓰레드와 다른 쓰레드로 ( io 쓰레드 등 ) 바꿔주면 subscribe 하면 다른 쓰레드에서 emit 이 일어나고, 다른 두번째 subscriber 도 구독을 시킬수 있게 됩니다. 

그리고 여기서 재밌는점이 아이템을 소비하는 순서 입니다. 
<img src="/images/rx-1.png">
위 그림을 보시면 subscriber A 가 아이템을 소모하는데 200ms 가 걸리고, B 가 10ms 걸립니다. 그러면 B가 먼저 item 을 다 소비해버릴것 같지만 하나의 아이템이 emit 되면 A,B 순서대로 다 소비된후에 다음 아이템이 emit 이 되는것을 알수 있습니다.

##### Subject , Relay() 를 활용하여 ..

두번째로, Subject 을 활용하면 multicast 를 refCount 를 이용해서 활용할수 있는 방안이 있는것 같습니다. 이것 또한 개미님의 Rx Study 에서 예제로 보여주신 코드에서 가져온것 입니다. 아래는 PublishSubject 를 활용한 것입니다.

```java
PublishSubject<Integer> publishSubject = PublishSubject.create();

Observable<String> observable = publishSubject
        .timestamp()
        .map( timestamped -> {
            System.out.println("\n________________map 연산________________\n");
            count.increase();
            return String.format("[%d] %d", timestamped.getValue(), timestamped.getTimestampMillis());
        } )
        .publish().refCount();

observable.subscribe( value -> {
    System.out.println("subscriber1 : " + value);
});

observable.subscribe(value -> {
    System.out.println("subscriber2 : " + value);
});

publishSubject.onNext( 1 );
publishSubject.onNext( 2 );
publishSubject.onNext( 3 );
publishSubject.onNext( 4 );

System.out.println("연산횟수 : " + count.count());

/** 결과
    ________________map 연산________________

    subscriber1 : [1] 1466583114891
    subscriber2 : [1] 1466583114891

    ________________map 연산________________

    subscriber1 : [2] 1466583114891
    subscriber2 : [2] 1466583114891

    ________________map 연산________________

    subscriber1 : [3] 1466583114891
    subscriber2 : [3] 1466583114891

    ________________map 연산________________

    subscriber1 : [4] 1466583114892
    subscriber2 : [4] 1466583114892
    연산횟수 : 4
*/
```

위와 같이 Subject 를 이용하여 item 을 나중에 emit 시키면 원하는 multicast 를 할수 있었습니다. 그리고 ReplaySubject 를 이용하면 subscribe 하기 전에 emit 한 item 도 받을수 있습니다.

```java
ReplaySubject<Integer> replaySubject = ReplaySubject.create();

replaySubject.onNext( 1 );
replaySubject.onNext( 2 );
replaySubject.onNext( 3 );
replaySubject.onNext( 4 );

Observable<String> observable = replaySubject
        .timestamp()
        .map( timestamped -> {

            System.out.println("\n________________map 연산________________\n");
            count.increase();
            return String.format("[%d] %d", timestamped.getValue(), timestamped.getTimestampMillis());
        } )
        .replay().refCount();

observable.subscribe( value -> {
    System.out.println("subscriber1 : " + value);
});

observable.subscribe(value -> {
    System.out.println("subscriber2 : " + value);
});

System.out.println("연산횟수 : " + count.count());

/** 결과
    ________________map 연산________________

    subscriber1 : [1] 1466583051876

    ________________map 연산________________

    subscriber1 : [2] 1466583051877

    ________________map 연산________________

    subscriber1 : [3] 1466583051877

    ________________map 연산________________

    subscriber1 : [4] 1466583051877
    subscriber2 : [1] 1466583051876
    subscriber2 : [2] 1466583051877
    subscriber2 : [3] 1466583051877
    subscriber2 : [4] 1466583051877
    연산횟수 : 4
*/
```

하지만 이부분에서도 emit 을 미리 해논상태에서 multicast 를 받으려면 ReplaySubject 를 사용해야하며, pulish() 말고 replay() 를 이용하여 ConnectableObservable 로 만들어주어야 원하는 동작을 볼수 있습니다. replay() 는 subscribe 하기 이전에 emit 된 아이템을 연산없이 바로 받을수 있는 동작 인데, 첫번째 구독자가 subscribe 하면 replaySubject 를 통해 내려온 item 4개를 모두 구독한후 두번째 구독자가 subscribe 하기 때문에 그냥 publish() 로 하면 두번째 구독자는 받을  item 이 없게 됩니다. 따라서 replay() 를 통해 ConnectableObservable 를 만들어 사용하면 원하는 multicast 를 구현할수 있습니다.


