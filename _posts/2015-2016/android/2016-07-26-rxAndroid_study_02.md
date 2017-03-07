---
layout: post
title: "RxAndroid Sum Operator"
published: false
modified:
categories: android
excerpt:
tags: [android, rxAndroid, rxJava]
image:
  feature:
date: 2016-9-11
---
<p style="font-size: 16px">
이번 포스팅은 ConnectableObservable 에 subscribeOn() 과 observeOn() 로 쓰레드를 변경 해주었을 경우에, 구독 과 소비의 순서에 관한 것입니다. 쓰레드를 어떻게 설정해주냐에 따라서 내가 원하는 흐름대로 진행하도록 할수도 있고, 예상치 못하게 흘러 갈수도 있어, 주의해야할 사항이다. 
</p>
<hr>

``` java
OperatorSum
        .sumLongs(
                realm.where(DailyWallet::class.java).equalTo("date", DateUtil.getTodayInt_yyyyMMdd()).findAllAsync()
                        .asObservable()
                        .onBackpressureBuffer()
                        .filter { it.isLoaded }
                        .first()
                        .flatMap {
                            Log.wtf("thread-check", "------------------------------ In the Flat Map " + Thread.currentThread())
                            if (0 < it.size)
                                Observable.from(it)
                                        .observeOn(Schedulers.io())
                            else
                                Observable.empty()
                        }
                        .map {
                            it ->
                            Log.wtf("thread-check", "------------------------------ In the Map " + Thread.currentThread())
                            it.spentMoney
                        }
        )
        .observeOn(AndroidSchedulers.mainThread())
        .subscribe(
                {
                    Log.wtf("thread-check", "------------------------------ onNext Today Money " + Thread.currentThread())
                    textView_todaySpentMoney.text = String.format("%,d", it)
                }, { },
                {
                    Log.wtf("thread-check", "------------------------------ onComplete Today Money " + Thread.currentThread())
                    progressBar_todaySpentMoney.visibility = View.GONE
                }
        )

Log.wtf("thread-check", "------------------------------ aaaaaaaaaaaaa" + Thread.currentThread())


/** Thread Check
A/thread-check: ------------------------------ aaaaaaaaaaaaaThread[main,5,main]
A/thread-check: ------------------------------ In the Flat Map Thread[main,5,main]
A/thread-check: ------------------------------ In the Map Thread[RxIoScheduler-2,5,main]
*/
```


