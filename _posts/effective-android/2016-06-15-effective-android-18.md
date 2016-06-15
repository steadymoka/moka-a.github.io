---
layout: post
title: "Effective android 18) Use executer service"
modified:
categories: effective-android
excerpt:
tags: [android, effective-android]
image:
  feature:
date: 2016-06-15
---
   
### 18. Use executer service
Let the service handle taking care of threads.

- Less error-prone
- Take advantage of thread pool

<br> 

### 18. Use executer service
서비스를 이용해서 쓰레드관리를 해라.

- 최대한 에러를 안나게 해라.
- 쓰레드풀의 이점을 이용해라.

<br><br>

##### 고찰

그냥 `new Thread(runnable)` 을 이용해서 처리하기에는 쓰레드 갯수 제한을 안둘경우 무한정으로 생성되 메모리 관리도 제대로 해주어야 하고, `Thread` 를 생성하는것 자체의 코스트도 크기 떄문에 여러 다중 요청이 필요한 경우에는 다른 것을 이용해서 하는것이 좋다. 

이럴때 이용하는것이 위 코드의 Executor 인터페이스를 활용한 것이다.

이 구조를 좀더 자세히 살펴보면 
최상위에 `Executor` 라는 인터페이스가 존재하는데 이것은 제공된 작업(Runnable)을 실행하는 객체가 구현해야할 인터페이스 이고, `ExecutorService` 또한 인터페이스 인데, `Executor` 의 라이프 사이클을 관리할수 있는 기술을 정의하고 있고, 콜백도 만들수 있다.

`Executors`라는 util 클래스를 통해서 객체를 만들수 있다. `newFixedThreadPool()` 라는 함수를 호출하면 실제로는 ExecutorService 를 implement 하고 있는 `ThreadPoolExecutor` 객체를 만들어서 반환하는데, 인터페이스 ExecutorService 로 반환한다.

`ExecutorService` 를 생성해 쓰레드 관리하는 예를 살짝 보면 

``` java
 // ExecutorService 사용법
 private ExecutorService executorService;

 private void init() {
    executorService = Executors.newFixedThreadPool( 5 );
 }

 public void enqueue( Runnable request ) {
        executorService.execute( request );
 }
```

만들어진 `executorService` 에 execute 로 runnable 객체를 넣어주면 된다. 이러면 runnable 이 구현하고 있는 코드들이 쓰레드풀에 맞게 실행이 된다. 

하지만,<br>
최대한 쓰레드 관리는 직접 안하는것이 좋을수도 있다. 그래서 대안으로 `RxAndroid` 를 이용하는것이 가장 좋을것 같다. Rx 로 `Ovservable` 를 만들어서 Scheduling 을 io 로 하면 위와 같이 `ThreadPool` 을 만들어서 동작을하게 된다. 만약에 newThread 로 하면 단위마다 새로운 쓰레드를 만들게 된다. 



[출처] [http://orhanobut.github.io/effective-android/](http://orhanobut.github.io/effective-android/)