---
layout: post
title: "안드로이드 | 노트4 - 2016.03.14"
modified:
categories: android
excerpt:
tags: [android, Service]
image:
  feature:
date: 2016-04-07
---

### Service ( startServer()  ->  onStartCommend() )
백그라운드(? -> 화면에 보이지 않는 작업, 비동기 처리가 아니다)에서 실행해야할경우 사용
-> 서비스를 호출한 액티비티(프레그먼트)가 죽더라도 서비스는 무기한 살아있을수 있다.
-> 서비스 내에서 stopSelf() 또는 다른 컴포넌트에서 stopService() 호출되기 전까지 살아있다

메인쓰레드에서 실행된다 -> 시간이 오래걸리는 작업 하면 안된다
-> 작업 쓰레드를 만들어서 쓰레드를 관리하는 용도로 쓰는 방향으로 사용


onCreate
- intent 를 이용해서 startService 를 할때 호출된다.
- 이때 이미 서비스가 한번 실행되었으면 호출되지 않고 onStartCommend() 가 호출된다

onStartCommend
- return 값
    + START_NOT_STICKY :

<br><br>

### View Background Animation

``` xml
<?xml version="1.0" encoding="utf-8"?>
<transition xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:drawable="@drawable/background_drawable1"/>
    <item android:drawable="@drawable/background_drawable2"/>
</transition>
```

애니메이션 넣고자하는 View 의 Background 에 resId 를 넣어주고, 코드상에서 바인딩된 해당 뷰의 getBackground() 를 가져오면 `Drawable` 객체로 나온다. 이 객체를 `TransitionDrawable` 로 캐스팅시켜 `startTransition(int durationMillis)` 를 호출하면 drawable1 에서 drawable2 로 Background 의 애니메이션이 동작한다.

<br><br>

### singleton static context

Sinlgeton 으로 만든 인스턴스에 context 가 필요한 경우가 있으면, 싱글톤 인스턴스를 갸져올 static 함수에 parameter 로 context 를 넘겨주어야 한다. 그러니까 이 객체는 항상 context 객체를 가지고 있어야 하는 것이다. 그럼 이것을 inject 를 해줄수 있을것 같은데 Dagger 를 좀더 공부해보고 알아보자.

<br><br>

### CoordinatorLayout / AppBarLayout
CoordinatorLayout 을 최 상위에 사용하고, 내부에 AppBarLayout 과 ViewPager(or RecyclerView) 를 이용해서 스크롤에 따른 행동을 편하게 할수있다. 복잡한 애니메이션 보다는 스크롤하면서 툴바만 생겼다 사라졌다 하도록 할경우, `AppBarLayout 에 flag 를 scroll|enterAlways` 를 사용하여 스크롤 올리면 툴바 사라지고 내릴때 바로 툴바 생기면서 사용할수 있다.

<br><br>

### AndroidManifest configChanges
> Activity 가 스스로 handling 할 환경 변화 ( Config Changes ) 를 나열해 주는 곳입니다.

화면에 변화가 있었을때, 액티비티를 재시작 안해주어도 될때 설정하도락 한다. ( ??.. )

<br><br>

### AndroidManifest windowSoftInputMode
1. 설정 X : adjustUnspecified 와 stateUnspecified 가 적용 된다.

2. adjustPan : 키보드가 올라오면 EditText에 맞춰 화면 UI가 실종 됩니다. (위 아래로 잘림)

3. adjustResize : 키보드가 올라와도 EditText와 UI가 화면에 보이도록 Activity를 resize 한다.

4. adjustUnspecified : 시스템이 알아서 상황에 맞는 옵션을 설정 한다. 키보드 조정에 대한 디폴트 값이다.

5. stateHidden : Acitivty 실행 시 키보드가 자동으로 올라오는 것을 방지 한다.

6. stateAlwaysHidden : Acitivty 실행 시 항상 키보드가 자동으로 올라오는 것을 방지 한다. (액티비티 이동 포함)

7. stateVisible : Acitivty 실행 시 키보드가 자동으로 올라 온다. (EditText에 포커스 맞춰짐)

8. stateAlwaysVisible : Acitivty 실행 시 항상 키보드가 자동으로 올라 온다. (EditText에 포커스 맞춰짐) (액티비티 이동 포함)

9. stateUnchanged : 키보드를 마지막 설정 상태로 유지 한다.

10. stateUnspecified : 키보드의 상태가 설정이 안된 상태이다. 시스템이 적절한 키보드를 상태를 설정해 주거나 테마에 따라 설정 해준다. 키보드 상태의 디폴트 값이다.

<br><br>

### 투명한 액티비티 만들기 Android Theme
```java
<style name="Theme.Moca.NoActionBar.Transparent" parent="Theme.Moca.NoActionBar">
    <item name="android:windowIsTranslucent">true</item>
    <item name="android:windowBackground">@color/transparent_moca</item>
</style>
```

위와 같이 하면 windowBackground 에 들어간 색상이 깔리면서 투명하게 된다.

`<item name="android:windowIsFloating">true</item>`
`<item name="android:backgroundDimEnabled">true</item>`

이 두가지 속성을 주면 액티비티가 다이얼로그 처럼 동작하게 된다. 이떄 오른쪽 왼쪽에 여백이 들어가게 되면서 원하지 않는 결과가 나올수가 있는데, 이부분에서 삽질을 많이 하게 될수도 있다.

<br><br>

### 소프트 키보드 변화 리스너
```java
private fun checkSoftKeyBoard() {
    rootView.viewTreeObserver.addOnGlobalLayoutListener {
        val heightDiff = rootView.rootView.height - rootView.height
        if (heightDiff > ScreenUtil.dipToPixel(activity, 200.0)) {
            // if more than 200 dp, it's probably a keyboard...
        }
        else {
        }
    }
}
```
