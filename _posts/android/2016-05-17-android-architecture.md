---
layout: post
title: "#1. Android Architecture, Flux/Agera 살펴보기"
modified:
categories: android
excerpt:
tags: [android, architecture]
image:
  feature:
date: 2016-05-17
---
<p style="font-size: 16px">
화면이 복잡해지고 유저 인터렉션이 많아 질수록 이벤트 들이 많이 생기고 여기저기 화면에서 데이터 간의 의존성과 연쇄적인 갱신이 많이 일어나게 된다. 그러다 보면 예상치 못한 결과가 나오기 일쑤다. 이런 문제를 해결할수 있는 방안으로 <code class="highlighter-rouge">flux</code> 를 활용 해보려고 한다.
</p>
<hr>

# Flux
`Flux` 는 Facebook 에서 발표한 웹 어플리케이션을 만들기 위한 아키텍쳐이다. 단방향 데이터 흐름을 가지며 뷰 컴포넌트를 구성하는 `reactJs` 를 보완하는 역할을 한다. <br>
[http://haruair.github.io/flux/](http://haruair.github.io/flux/) 에 flux 문서 한글 번역이 있다. 
<br>
<br>

### Flux is
Flux 의 핵심적인 3가지는 `Dispatcher`, `Store`, `View` 이다. 
<figure>
    <img src="/images/flux_detail.png" alt="image" style="">
</figure>
<br>
`Actions Creator` 에 뷰에서 일어나는 액션들이 모두 정의되어 있고, 뷰에서 해당 액션이 일어났을때 호출하게 된다. <br>
<br>
`Actions Creator` 은 `Dispatcher` 를 가지고 있고, `Dispather` 에 액션이 일어났다고 알려주게 된다. <br>
<br>
`Dispatcher` 에서는 받은 이벤트를 가지고 `Action` 을 만들게 된다. 그리고 만든 `Action` 으로 이벤트를 날린다.<br>
<br>
이 이벤트는 `Store` 에서 받게 되며, `switch` 문으로 어떤 이벤트인지에 따라서 `Store` 의 데이터를 변화시킨다. <br>
<br>
`Store` 에서 데이터가 변경되면 해당 데이터를 가지고 있는 뷰들에서 이벤트를 받고, 이벤트를 받으면 `Store` 에 접근해 최신 데이터를 가지고와서 뷰를 업데이트를 한다.<br>
<br>
여기서 `Store` 는 싱글톤 으로 구현되 어디서든 접근이 가능하다.<br>
<br>

##### Q. 근데 여기서 굳이 Store 에 이벤트를 subscribe 를 해서 받을 필요가 있는가.
Store 는 싱글톤으로 구현되어 있기때문에 바로 인스턴스에 접근이 가능하고, 해당 데이터를 조작하는 액션을 Store 기존에 이벤트를 받아서 호출되지만 바로 접근해서 호출하는게 더 좋지 않을까. 샘플로 보고있는 프로젝트에서는 Dispatcher 가 없어도 전혀 지장이 없어보인다. 다른 예시를 찾아보자.<br>
<br>
<br>

# Google Agera
>Agera is a set of classes and interfaces to help write functional, asynchronous, and reactive applications for Android.

`Agera` 는 안드로이드에서 functional, asynchronous 그리고 reactive 앱을 위한 클래스들과 인터페이스들의 집합이다. `Agera` 는 2016년 4월 19일에 구글에서 공개한 라이브러리 인데, 설명만 봐서는 딱 `RxAndroid` 인데,  wiki 를 읽어보면 flux 또는 redux 의 구조를 나타낸듯 하다.
<br>
<br>

#### Agera is
`Agera` Sample Project 를 분석해 보았다. 여기에는 정말 심플하게 `Observable` 와 `Updatable` 만을 사용해서 구성해 놓았다. <br>
간단히 설명하면, `Observable`은 이벤트를 발생시키는 놈, `Updatable`은 이벤트를 관찰 하는 놈이라 보면된다. `Observable`에 Updatable 객체를 add 시켜줌으로써 관찰 하게 된다. `Observable` 도 `Updatable`이 될수 있다.<br>
<br>

내부적으로 `Worker` 라는 놈이 있는데, 이놈이 `Observable` 와 `Updatable` 들을 가지고 있으며 관리 하고 있다. `WorkerHandler` 는 static 클래스 싱글톤으로 되어있고 `Worker` 들에게 메시지를 보내는 핸들러라 보면된다. <br>
`BaseObservable` 에 보면 `dispatchUpdate()` 가 구현 되어 있는데, 이함수를 호출하게 되면 우선 `WorkerHandler` (Observable 에 관한 여러 메시지를 받아서 처리하는 핸들러이다. 업데이트, 추가, 삭제 등등 ) 가 메시지를 받아 Worker 에게 알려주면 `Worker` 가 자신의 Updatable 모두에게 Noti 를 보내게 된다. 그럼 `Updatable` 을 구현하고있던 곳에서 update() 함수가 호출된다.<br>
<br>
공식 예제 프로젝트에는 `Fragment`가 `Updatable`를 implement 하고 있고, 프레그먼트 생명주기에 맞춰 `Observable` 를 생성해 this 를 넘겨주고, 해제하고 있다. 그리고 리프레쉬라는 이벤트가 발생하면 `Observable`이 캐치해 데이터를 최신화 시켜두고 다시 프레그먼트에 이벤트를 줘서 거기서 UI 갱신을 해주는데, 이때 `Observable`에서 최신화된 데이터를 가지고와서 뿌려준다. <br>
<br>
이렇게 보면 `Observable` 이 위 `Flux`의 Store 와 Dispatcher 역활을 한다고 볼수있고, `Updatable`은 View 의 역할이라고 할수있겠다.<br>
<br>
Agera 위키에 보면 좀더 다양한 컴포넌트들이 있는데 샘플 프로젝트에는 위의 것만 사용하여 구성이 되어 있어 아쉬운점이 많았다. 좀더 문서를 읽어볼 필요가 있을듯 하다. 
<br>
<br>

#### 흠흠 .....
Agera 이슈에 보면 RxAndroid 와의 관계에 대한 토론한 글도 있고, 샘플 프로젝트지만 봤을때는, 그냥 Rx 를 이용해 구독, 관찰해서 구현하면 될것 같긴하다. 사실 어떻게 보면 간단하게 데이터만 바라보고 데이터가 변경되면 이벤트만 날리게 구현하면 될것 같은데, 직접 구현해보면서 발전시켜 나갈필요가 있을듯 하다. 다음 포스팅에는 실제로 구현해본후 후기를 쓰도록 할 예정이다.

<br>
<br>
[참고문서]<br>
Agera 공식 github [[https://github.com/google/agera/wiki](https://github.com/google/agera/wiki)] <br>
드라마앤컴퍼니 rfrost님 [[http://developer.dramancompany.com/2016/03/...](http://developer.dramancompany.com/2016/03/%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C%EC%97%90-flux-%EC%A0%81%EC%9A%A9%ED%95%98%EA%B8%B0/)]




