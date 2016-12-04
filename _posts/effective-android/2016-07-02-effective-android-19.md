---
layout: post
title: "Effective android 19) Do not log sensitive information"
modified:
categories: effective-android
excerpt:
tags: [android, effective-android]
image:
  feature:
date: 2016-06-15
---
Effective Android 번역및 회고 글입니다


### 19. Do not log sensitive information
- Use proguard to remove logs
- Do not log passwords
- Do not log usernames
- Do not log credit card information
- Do not log 3rd party data
- Do not log device information

<br>

### 19. Do not log sensitive information
- 프로가드릉 이용해서 로그들을 지워라
- 패스워드들을 로그로 남기지 마라
- 유저의 이름들을 로그로 남기지 마라
- 신용카드의 정보를 로그로 남기지 마라
- 3rd party 데이터를 로그로 남기지 마라
- 기기정보를 로그로 남기지 마라

<br><br>

##### 그래서
이번 세션은 로그에 남기지 말아야 할것들의 대한 것이다. 민감한 정보들은 당연히 로그로 남기지 않아야 한다. 특히 AWS 를 사용할경우 접속 정보를 잘못 남겼다가 깃허브로 공개되면 요금 폭탄을 맞을수도 있다.

여기서 한가지 새롭게 알게된 사실은 프로가드를 이용해서 로그를 지울수 있다는 것이다. 사실 아직 프로가드를 제대로 써본적이 없어 이런게 지원한다는것을 몰랐었다.

프로가드를 이용해 로그를 지우는 방법은, 프로가드 룰을 적은 txt 파일은 만들어 로그를 지우도록 코드를 작성하면 된다.

```java
-assumenosideeffects class android.util.Log {
    public static boolean isLoggable(java.lang.String, int);
    public static int d(...);
    public static int w(...);
    public static int v(...);
    public static int i(...);
}
```

이런식으로 프로가드룰을 정의하고, gradle 파일에 `proguardFiles getDefaultProguardFile('proguard-android-optimize.txt')` 이런식으로 옵션을 써주면 된다.


하지만 로그들로 디버깅, 앱이 제대로 동작하는지 체크하기윈한 용도나, warning 사항을 알리기 위해 사용되기도 하는데, 이런 로그들은 굳이 지울필요가 있는가 이기도 하다. 지금 필요할때 썼다가 나중에 다시 디버깅할때 로그가 없어 불편할수도 있다. 로그를 몇줄 쓴다고 앱 성능이 떨어지지는 않는다. 단지 몇 KB 용량이 늘어날수도 있겠지만 미미한 수준이다.

무튼 민감한 정보가 로그로 남지 않도록 주의해야할 필요는 있다.

[출처] [http://orhanobut.github.io/effective-android/](http://orhanobut.github.io/effective-android/)
