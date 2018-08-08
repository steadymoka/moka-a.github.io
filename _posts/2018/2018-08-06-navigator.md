---
layout: post
title: "Android Navigation"
author:
published: true
modified:
categories: android
excerpt: Android Navigation
tags: [android, jetpack]
image:
  feature:
date: 2018-8-6
---

## Android Navigation

<br>
### Principles of Navigation
Navigation 은 앱내에서 라우팅 역활을 도와줄 컴포넌트 이다. 
- 보통의 앱들은 처음 실행할때 항상 실행되는 화면이 있다. 이 화면은 뒤로가기 버튼을 계속 눌러 앱이 꺼지기 직전의 화면이 될 수 도 있다.
- 화면들이 바뀔때 스택 등을 활용하여 state 를 관리할 필요가 있다. 
- 딥링크등을 활용하여 원하는 화면을 띄워야 하며, 이때 이화면에 대한 상태를 만들 필요가 있다. 


<br>
### 참고자료들
아마 navigation 을 리서치 하다 보면 필히 [https://medium.com/google-developer-experts/android-navigation-components-part-1-236b2a479d44](https://medium.com/google-developer-experts/android-navigation-components-part-1-236b2a479d44) 이 블로그를 보게 될 것이다. 3부작으로 나눠서 포스팅이 되어 있는데, Google Android GDE 분이 포스팅을 하신 글이다. 

그리고, 공식 문서에도 설명과 사용법에 대해서 나와있기도 하다. [https://developer.android.com/topic/libraries/architecture/navigation/](https://developer.android.com/topic/libraries/architecture/navigation/) 하지만 한방에 이해가 가지는 않았지만, 코드랩에도 navigation 을 소개하고 있다. [https://codelabs.developers.google.com/codelabs/android-navigation/#0](https://codelabs.developers.google.com/codelabs/android-navigation/#0) 간단한 샘플 예제를 가지고 따라서 구현해 보는 코드랩 이다. 따라하면 뭔가 동작은 하는데, 사실 완전히 이해 하기가 조금 어려웠다.

<br>
### 주요 구성
Navigation 의 주요 구성으로는 새로운 `<navigation>` 태그를 이용한 xml 을 만들게 된다. 보통 graph 라고 부르며 화면들을 배치하고 화면간의 이동 및 전환 애니메이션을 정의하게 된다. 전환을 정의하기 위해서는 `action` 이라는 것을 이용한다. `action` 또한 xml 에 정의 한다. 그리고 이런게 정의된 xml 은 rendering 된 UI 로 볼수 있도록 android studio 에서 제공 하고 있다. 

구글은 Single Activity 를 권장한다. 하나의 Activity 에 Fragment 들을 활용하여 하나의 navigation 를 권장한다. 하지만 원한다면 여러개의 Activity 들과 여러개의 graph 를 그릴수 있다. 

<br>
### 구현
먼저 dependency 를 추가해준다.
``` java
dependencies {
    //Other Dependencies...
    
    implementation "android.arch.navigation:navigation-fragment:1.0.0-alpha01" 
    implementation "android.arch.navigation:navigation-ui:1.0.0-alpha01"
}
```

그리고 제일 먼저 할 일은 navigation graph resource 파일을 만드는 것이다. `New -> Android Resource File.` 에서 resourec type 을 `Navigation resource type` 으로 하여 만들어 준다.

해당 resource 에 graph 를 그려준다고 해서 아무것도 되는 것은 없고 단지 전체 그림만을 보여줄 뿐이다. 이 그림들을 동작하게 하기 위해서는 `Navigation Host` 가 필요하다. Navigation Host 는 NavController 를 통하여 화면 전환등을 작동 하게 된다. Navigation Host 는 아래와 같이 Activity 에 포함하게 된다. 

``` xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context="navexample.app.dario.com.MainActivity">

    <fragment
        android:id="@+id/nav_host"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:name="androidx.navigation.fragment.NavHostFragment"
        app:navGraph="@navigation/main_nav"
        app:defaultNavHost="true"/>

</android.support.constraint.ConstraintLayout>
```

`app:navGraph` 에 아까 만들어줬던 resource 를 넣어줌으로써, graph 가 Navigation Host 에 포함되게 되며, NavController 를 통하여 전체 흐름을 컨트롤 할수 있게 된다. 

그러면 이제 NavController 를 필요한 상황들이 많이 발생하게 될텐데, 
``` java
val navController = Navigation.findNavController(fragment)
```
를 통하여 Controller 를 가져올수 있다. 그리고 `navController.navigate(R.id.destination_id)` 등의 명령어를 통하여 화면 전환을 할 수 있다. 이전 화면으로 돌아가려면 `navController.navigateUp()` 를 호출 하면된다. 


화면 전화시 필요한 데이터를 넘기는 것은 bundle 을 이용하여 넘기게 된다. 또 다른 방법으로는 `safe arg` plugin 을 통해 좀더 쉽게 넘길수 있는 방법을 제공하고 있다. 





<br>
<br>

