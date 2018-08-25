---
layout: post
title: "Android Navigation in Jetpack"
author:
published: true
modified:
categories: android
excerpt: Android Navigation
tags: [android, jetpack]
image:
  feature:
date: 2018-8-10
---
<br>
### Principles of Navigation
Navigation 은 앱내에서 라우팅 역활을 도와줄 컴포넌트 이다. 아래는 공식 홈페이지에 나와있는 Navigation 에 대한 개요이다.
- The app should have a fixed starting destination
- A stack is used to represent the "navigation state" of an app
- The Up button never exits your app
- Up and Back are equivalent within your app's task
- Deep linking to a destination or navigating to the same destination should yield the same stack


<br>
### 참고자료들
아마 navigation 을 리서치 하다 보면 필히 [https://medium.com/google-developer-experts/android-navigation-components-part-1-236b2a479d44](https://medium.com/google-developer-experts/android-navigation-components-part-1-236b2a479d44) 이 블로그를 보게 될 것이다. 3부작으로 나눠서 포스팅이 되어 있는데, Google Android GDE 분이 포스팅을 하신 글이다. 

그리고, 공식 문서에도 설명과 사용법에 대해서 나와있기도 하다. [https://developer.android.com/topic/libraries/architecture/navigation/](https://developer.android.com/topic/libraries/architecture/navigation/) 하지만 한방에 이해가 가지는 않았지만, 코드랩에도 navigation 을 소개하고 있다. [https://codelabs.developers.google.com/codelabs/android-navigation/#0](https://codelabs.developers.google.com/codelabs/android-navigation/#0) 간단한 샘플 예제를 가지고 따라서 구현해 보는 코드랩 이다.

<br>
### 주요 구성 및 컨셉
Navigation 의 주요 구성으로는 새로운 `<navigation>` 태그를 이용한 xml 을 만들게 된다. 보통 graph 라고 부르며 화면들을 배치하고 화면간의 이동 및 전환 애니메이션을 정의하게 된다. 전환을 정의하기 위해서는 `action` 이라는 것을 이용한다. `action` 또한 xml 에 정의 한다. 그리고 이런게 정의된 xml 은 rendering 된 UI 로 볼수 있도록 android studio 에서 제공 하고 있다. 

구글은 Single Activity 를 권장한다. 하나의 Activity 에 Fragment 들을 활용하여 하나의 navigation 를 권장한다. 하지만 원한다면 여러개의 Activity 들과 여러개의 graph 를 그릴수 있다. 

그리고 `NavController` 라는 것을 이용하여 화면 전환을 하게 된다. 

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

#### Navigation xml 
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

`app:navGraph` 에 아까 만들어줬던 resource 를 넣어줌으로써, graph 가 Navigation Host 에 포함되게 되며, NavController 를 통하여 전체 흐름을 컨트롤 할수 있게 된다. 이렇게 Activity 에 `Host Fragment` 를 넣어 줌으로써 navigation component 를 쓸 준비가 다 된 것이다. 

#### Controller
이제 NavController 를 이용하여 화면전환을 할수 있게 되었다. 그러면 NavController 가 있어야 한다. 
``` java
// Host Fragment 로 navigation 된 곳에서는 fragment 를 이요하여 찾아 올수 있다.
val navController = NavHostFragment.findNavController(fragment)
// 또는 view 를 이용하여 찾을수도 있다. 
val navController = Navigation.findNavController(view)
```
를 통하여 Controller 를 가져올수 있다. ( navigation controller 가 정확히 어디에 있는지, view, fragment 를 통해서 가져오는 것에 대해서 좀더 알아볼 필요가 있다. ) 
그리고 `navController.navigate(R.id.destination_id)` 등의 명령어를 통하여 화면 전환을 할 수 있다. 이전 화면으로 돌아가려면 `navController.navigateUp()` 를 호출 하면된다. 

#### 데이터 전송
화면 전화시 필요한 데이터를 넘기는 것은 bundle 을 이용하여 넘기게 된다. 또 다른 방법으로는 `safe arg` plugin 을 통해 좀더 쉽게 넘길수 있는 방법을 제공하고 있다. 
먼저 bundle 을 이용한 방법을 보면 우선,

``` xml
<fragment
   android:id="@+id/confirmationFragment"
   android:name="com.example.cashdog.cashdog.ConfirmationFragment"
   android:label="fragment_confirmation"
   tools:layout="@layout/fragment_confirmation">

   <!-- argument 태그를 이용하여 받으려는 데이터의 key, value 를 정의 -->   
   <argument android:name="amount" android:defaultValue=”0” />
```

한 후, controller 를 이용하여 navigate 할 때, bundle 을 넘겨주고 bundle 을 받아서 값을 꺼내오면 된다.

``` java
// 데이터를 보내고, 화면 전환을 하려는 곳
var bundle = bundleOf("amount" to amount)
view.findNavController().navigate(R.id.confirmationAction, bundle)

// 데이터를 받는 곳 ( 목적지 )
val tv = view.findViewById(R.id.textViewAmount)
tv.text = arguments.getString("amount")
```

이게 기본적인 데이터 전송 방법인데, 이렇게 보낼때 약간의 문제가 받는 쪽에서는 type 을 실수로 다르게 받아 에러가 발생할 수 도 있다. 물론 개발단에서 대부분 발견 할 수 있는 버그 이지만, naviagtion 에서는 `safe arg` 라는 것으로 code generating 을 통해 type 을 미리 알 수 있도록 해주는 plugin 을 제공한다. 

``` s
apply plugin: 'com.android.application'
apply plugin: 'androidx.navigation.safeargs'

android {
   //...
}
```

``` xml
<fragment
    android:id="@+id/confirmationFragment"
    android:name="com.example.buybuddy.buybuddy.ConfirmationFragment"
    android:label="fragment_confirmation"
    tools:layout="@layout/fragment_confirmation">
    
    <!-- 위와 비슷하지만 type 을 추가해준다 -->
    <argument android:name="amount" android:defaultValue="1" app:type="integer"/>
</fragment>
```
여기서 타입은 `integer` `string` `long` `boolean` 등등이 된다
그리고 데이터를 받고 보내는 코드를 살펴보면, direction 과 generating 된 코드들을 이용하여 데이터를 받아서 쓰면 된다. 

``` java
// 보내는 곳 
override fun onClick(v: View?) {
   val amountTv: EditText = view!!.findViewById(R.id.editTextAmount)
   val amount = amountTv.text.toString().toInt()
   val action = SpecifyAmountFragmentDirections.confirmationAction(amount)
   action.amount = amount
   v.findNavController().navigate(action)
}

// 받는 곳
override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
    val tv: TextView = view.findViewById(R.id.textViewAmount)
    val amount = ConfirmationFragmentArgs.fromBundle(arguments).amount
    tv.text = amount.toString()
}
```

<br>

이렇게 하면 기본적으로 navigation 을 이용하여 앱 화면을 routing 을 할수 있다.





<br>
<br>

