---
layout: post
title: "안드로이드)[펌]이벤트 전달 메커니즘"
modified:
categories: android
excerpt:
tags: [android, touch, event]
image:
  feature:
date: 2015-07-27T19:39:55-04:00
---

[안드로이드] Event 처리 메커니즘

출처 : [http://ecogeo.tistory.com/251](http://ecogeo.tistory.com/251)
안드로이드의 이벤트 처리 과정에 대한 글(http://blog.naver.com/osk1004?Redirect=Log&logNo=50069078782 )을 참조하여 나름대로 분석하여 메모한 결과를 적어본다.

#### 개략적인 이벤트 처리 과정

    액티비티 생성시 액티비티의 윈도우를 WindowManagerService에 등록해둠
    이벤트 발생시 네이티브 라이브러리(EventHub)를 통해 이벤트 읽음
    이벤트 큐(KeyInputQueue)에 이벤트 쌓임
    이벤트 디스패치 쓰레드(InputDispatcherThread)는 이벤트큐에서 이벤트를 꺼내어
    WindowManagerService의 디스패치 메소드 호출
    WindowManagerService는 등록된 애플리케이션의 윈도우에 이벤트를 전달
    이벤트를 전달받은 윈도우는 하위 UI 컴포넌트 트리를 찾아가며 리스너 콜백함수 실행


### 이벤트 전달을 위한 준비

WindowManagerService는 system_server 프로세스에서 실행중인 서비스이다. WindowManagerService에서 감지된 이벤트를 애플리케이션 프로세스의 UI 컴포넌트에 전달하기 위해서 둘 사이에 연결고리가 미리 만들어져 있어야 한다. 통신은 AIDL을 통해서 이루어진다.

애플리케이션의 윈도우를 WindowManagerService에 등록하는 과정

    1.ActivityManagerService는 ActivityThread를 호출하여 액티비티 런치.
      ActivityThread.performLaunchActivity()에서 Activity인스턴스 생성하고 activity.attach() 호출
    2.Activity는 attach()에서 PhoneWindow 객체 생성. 이 PhoneWindow는 액티비티내 뷰들의 root로서 DecorView 인스턴스 포함.

      mWindow = PolicyManager.makeNewWindow(this);
     
    3.ActivityManagerService는 ActivityThread를 호출하여 액티비티를 resume시킴.
      WindowManager 인스턴스가 생성되고 decorView가 WindowManager에 추가됨.

      ActivityThread.handleResumeActivity()
     
    4.WindowManager의 addView(decor)에서 ViewRoot 인스턴스를 생성하고 viewRoot.setView(decor) 호출
    5.viewroot.setView(decor)에서 IWindowSession을 통해 WindowManagerService에 IWindow인스턴스를 추가

      IWindowSession.add(window) 
     

#### DecorView 클래스
- FrameLayout을 상속받으며, PhoneWindow의 내부 클래스로 정의됨
- 표준 윈도우 프레임 및 데코레이션을 포함하는 최상위 윈도우 뷰
- 윈도우 매니저에 윈도우로서 추가됨

#### ViewRoot 클래스
- WindowManager와 View 사이의 protocol을 위한 구현 포함
- Handler를 상속받음
- IWindow 서비스 구현 클래스(W)를 내부 클래스로 포함 :  class W extends IWindow.Stub

#### 관련 AIDL
IWindowSession.aidl : 애플리케이션 --> WindowManagerService

    int add(IWindow window, ... ...); // 윈도우를 WindowManagerService에 추가
    void remove(IWindow window);   

IWindow.aidl : WindowManagerService --> 애플리케이션

    void dispatchKey(in KeyEvent event); //이벤트를 애플리케이션에 전달
    void dispatchPointer(in MotionEvent event, ...);
    void dispatchTrackball(in MotionEvent event, ...);

KeyEvent.aidl, MotionEvent.aidl : 프로세스간 전달되는 이벤트 정보


#### 이벤트 감지 및 디스패치
이벤트를 검출하고 애플리케이션으로 디스패치하는 로직은 WindowManagerService.java에 구현되어 있다. WindowManagerService는 InputDispatcherThread와 KeyInputQueue 구현 클래스를 이용하여 이벤트를 읽어들이고 적절한 윈도우에 전달하는 일을 한다.

#### WindowManagerService 클래스
- KeyInputQueue 구현 클래스(KeyQ) 및 InputDispatcherThread 클래스 포함
- WindowManagerService 인스턴스 생성시 KeyInputQueue 생성 및 InputDispatcherThread 쓰레드 시작
- InputDispatcherThread는 이벤트 타입에 따라 WindowManagerService의 디스패치 메소드 호출.

       dispatchKey(KeyEvent); // 예를들어 키보드 이벤트인 경우

- 디스패치 메소드는 현재 포커스를 가진 윈도우를 찾아 이벤트 전달

     mKeyWaiter.waitForNextEventTarget(); // WindowState 찾음
     windowState.mClient.dispatchKey(event); // windowState.mClient는 IWindow 객체

- IWindow 객체는 액티비티가 resume인 상태가 되면서 ViewRoot가 WindowServiceManager에 전달한 것

#### KeyInputQueue 클래스
- 안드로이드에서 진짜 이벤트 큐 역할
- 인스턴스 생성시 새로운 쓰레드가 시작되면서 native boolean readEvent() 메소드를 무한루프 호출.
- 리눅스 입력 디바이스로부터 실제 이벤트를 읽어들이는 로직은 네이티브 코드로 구현됨 : EventHub
- 이벤트 읽는 과정 

   KeyInputQueue.java -> JNI -> com_android_server_KeyInputQueue.cpp -> EventHub.cpp -> Device

#### InputDispatcherThread 클래스
- Event-Dispatch Thread 구현 클래스(?)
- 무한루프 돌면서 이벤트 큐에서 이벤트를 꺼내 WindowManagerService의 디스패치 메소드 호출

#### 이벤트 유형
- 키보드 : RawInputEvent.CLASS_KEYBOARD
- 트랙볼 : RawInputEvent.CLASS_TRACKBALL
- 터치스크린 : RawInputEvent.CLASS_TOUCHSCREEN
- 설정 변경 : RawInputEvent.CLASS_CONFIGURATION_CHANGED

#### 이벤트 정보를 담고 있는 핵심 클래스
- 키보드 이벤트 : KeyEvent
- 터치 or 트랙볼 이벤트 : MotionEvent


### 애플리케이션에서 이벤트 처리 과정(키 이벤트 중심)

이벤트를 전달받은 애플리케이션 윈도우는, 뷰 트리의 최상위부터 시작해서 실제 포커스를 가진 뷰까지 경로를 따라 이벤트를 디스패치한다.

    이벤트가 발생하면 WindowManagerService는 이벤트 큐의 이벤트를 IWindow에 전달

    IWindow.dispatchKey(event);
     
    IWindow(ViewRoot.W 내부클래스가 구현)는 이벤트를 ViewRoot의 dispatchKey(event)에 다시 전달
    ViewRoot.dispatchKey()에서는 sendMessageAtTime(msg) 메소드를 통해 메시지 형태로 이벤트를 전달(왜 갑자기 여기서 Handler 메시지 형태로 이벤트를 전달하는가?)
    보내진 이벤트 메시지는 Handler를 상속받은 ViewRoot의 handleMessage(Message)가 처리
    handleMessage()는 deliverKeyEventToViewHierarchy(event)를 호출
    deliverKeyEventToViewHierarchy()는 decor view의 dispatchKeyEvent(event) 호출

    mView.dispatchKeyEvent(event);
     
    decor view의 dispatchKeyEvent()에서는 현재 뷰에 리스너가 등록되어 있으면 현재 view의 리스너 콜백함수를 호출함(즉 드디어 이벤트가 처리됨)
    등록된 리스너가 없으면 KeyEvent의 dispatch(callback) 호출 : callback은 view 자신
    KeyEvent.dispatch()는 다시 callback view의 onKeyDown() 호출 : 키 누름 이벤트인 경우
    view의 onKeyDown()은 setPressed() 호출 : setPressed() ->dispatchSetPressed()
    dispatchSetPressed()는 하위 View 클래스(예를들어 ViewGroup)에서 적절히 오버라이드됨
    ViewGroup의 dispatchSetPressed()에서는 자식 뷰들의 setPressed()를 호출
    이런식으로 최종 타겟 UI 컴포넌트까지 이벤트가 디스패치됨


#### View 클래스
- KeyEvent.Callback 구현
- 주요 디스패치 메소드 :

    dispatchKeyEvent(KeyEvent event);
    dispatchTouchEvent(MotionEvent event);
    dispatchTrackballEvent(MotionEvent event);

#### ViewGroup 클래스
- XXXLayout 들의 부모 클래스
- dispatchSetPressed(boolean pressed) 메소드 코드 :

{% highlight java %}
        final View[] children = mChildren;
        final int count = mChildrenCount;
        for (int i = 0; i < count; i++) {
            children[i].setPressed(pressed);
        }

{% endhighlight %}