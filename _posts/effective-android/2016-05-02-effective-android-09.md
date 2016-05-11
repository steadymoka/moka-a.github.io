---
layout: post
title: "Effective android 09) Enhanced loop"
modified:
categories: effective-android
excerpt:
tags: [android, effective-android]
image:
  feature:
date: 2016-05-02
---

### 9. Enhanced loop
- Enhanced loop throws NPE if the list is null
- Enhanced loop use iterators in behind
- Not good for concurrency
- Use for loop if you need the count index
- Use traditional for loop for arraylist for performance-critic. Traditional for loop is 3x faster than enhanced loop for ArrayList. Because in ArrayList, accessing an element is constant O(1)

<br> 

### 9. Enhanced loop
- Enhanced loop (for-each문) 는 리스트가 비어있으면 Null PointException 을 뱉는다.
- Enhanced loop 는 내부적으로 iterators 를 사용한다. 동시성 처리에 적합하지 않다.
- index 를 셀 필요가 있으면 for 루프를 이용해라
- arraylist 성능에 좋은 전통적인 for 문을 사용해라. 전통적인 for 문은 arraylist 에서 for-each 문 보다 3배 빠르다. 왜냐하면 arraylist 에서는 요소에 접근하는것은 상수(?)이다. 


<br><br>

#### 고찰
for 문에는 일반적인 for 문, for in 문 ( for-each ), iterator( 써보지는 않았다 .. ), 등이 있는걸로 알고있다.
자바스크립트에서는 map, filter, 등등으로 람다식을 넘겨주는걸로도 가능하고, rxJava 를 이용하여 이러한 문법을 이용할수 있다고는 알고있다. 사실 어떤걸 쓸지는 성능같은거는 별로 신경안쓰고 코딩하기 편한걸로 쓰긴하지만. 확실히 어떻게 동작하고 어떤 차이가 있는지는 알고있는게 좋을것 같다.

..
https://www.youtube.com/watch?v=MZOf3pOAM6A&feature=youtu.be 
관련 영상이다. 도움이될것 같다.


근데 구글 디벨로퍼 http://developer.android.com/intl/ko/training/articles/perf-tips.html#Loops 에서는 for-each 문이 제일 빠르다고 하는데, 위의 글에서는 전통적인 for 문이 더 빠르다고 하니 둘중 하나가 틀린것 같다. 

일반적으로는 for-each 문을 쓰도록 해야겠다 .




[출처] [http://orhanobut.github.io/effective-android/](http://orhanobut.github.io/effective-android/)