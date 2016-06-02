---
layout: post
title: "Effective android 17) Use switch where possible"
modified:
categories: effective-android
excerpt:
tags: [android, effective-android]
image:
  feature:
date: 2016-06-02
---
   
### 17. Use switch where possible
- With java 7, String can be used in the switch cases
- You can use switch in onClick(), I prefer ButterKnife to split methods, more readable with that way
- Use enums/constants to make readable cases
- Use "fall through" approach. When the case is the same with the next one, you can put a comment and tell that it will fall through. This will help the next developer to understand that this is done intentinally.

<br> 

### 17. Use switch where possible
- java 7 은, String 이 switch 에서 분기될수 있다.
- onClick() 에서 switch 를 사용할수있고, ButterKnife 를 사용하여 메소드를 분리하는것이 더 읽이 좋은 방법이다. 
- enum 과 constant 를 사용해서 케이스들을 읽기 쉽도록 하여라
- `fall through` 접근법을 사용해라. 어떠한 경우가 다음에 똑같이 발생했을때, 코멘트를 할수있고, fall through 가 될것이다. 코멘트를 해놓으면 나중에 개발할때 그것이 어떤것인지 도움이 될것이다.

<br><br>

##### 고찰
enum 이 성능상 약간 떨어진다고 하나, 지금의 기기에서는 아주 미미한 수준의 성능차이이다. 따라서 좀더 보기쉽고 읽기 쉬운 코드를 만들수있는 enum 을 사용하는것이 더 좋다고 생각 한다. 

클린 코드 책을 다시 봐야겠다. 

[출처] [http://orhanobut.github.io/effective-android/](http://orhanobut.github.io/effective-android/)         