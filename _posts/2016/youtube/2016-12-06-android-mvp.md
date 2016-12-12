---
layout: post
title: "Android | MVP, What is it? Why to use?"
author:
published: true
modified:
categories: android
excerpt: 안드로이드의 패턴중 하나인 MVP 패턴에 대해서 알아보고, 어떤식으로 구조를 잡고 코드를 구현해 나가야 되는지 코드를 통해서 차근차근 살펴봅시다. 먼저 전체적으로 MVP 의 대한 개념을 주절주절 이지만 그림과 함께 설명 해보겠습니다.
tags: [android, mvp]
image:
  feature:
date: 2016-12-5
---
안드로이드 디자인 패턴 중 하나인 MVP 에 관한 내용입니다. 다른 패턴( MVC, MVCC .. )등 이 있지만 비교는 하지 않고 MVP 에 대해서만 설명할 예정이고, 샘플 프로젝트 및 코드등을 통해 실제 프로젝트에 적용 하기 까지의 과정을 적을 것입니다.

<br>

## WHY MVP
제가 생각 하는 MVP 의 가장 큰 장점은 Unit 테스트를 하기에 용이하다는 점입니다. 안드로이드 소스코드 들을 테스트하려고 하다보면, 꼭 한번씩 context 가 필요하다던지, 뷰와 연관이 있거나, 디비 접근을 해야한다던지 메소드를 분리시키거나 등 어려움을 겪을수가 있습니다.

하지만 MVP 를 통해 Presenter 에서는 인터페이스를 통해 객체를 인젝션 하여 서로간의 의존도를 없앨수 있습니다.
따라서 테스트가 필요한 로직만을 테스트 할수 있게 됩니다.

<figure>
	<img src="/images/posting_mvp/ig_mvp_01.png" alt="image">
</figure>
위 그림은 MVP 를 그림으로 보여주는것인데, 인터페이스를 통한 MVP 의 과정을 잘 나타내는 것 같습니다.

<br>

## WHAT & HOW TO USE
우리가 짜는 안드로이드 소스코드 동작은 말로 풀어써 쓰면, Fragment( or Activity ) 의 라이프 사이클을 통해 코드들이 화면에 구현이 되며, 따라서 제일먼저 Fragment 를 만들어 라이프 사이클에 맞추어 원하는 화면을 그리는 코드들을 작성하게 됩니다. 원하는 화면을 그리기 위해서는 데이터가 필요하게 되고, 데이터를 가공을 할 필요가 생길 것입니다.

>-> 앱시작 (Fragment 의라이프사이클 동작)<br>
-> 로컬이나 서버에서 데이터를 가져와서 가공<br>
-> Fragment 에 있는 뷰들에 set (setText(), setImageResource(), setVisibility() ... )

이것을 Fragment 에 다 넣을수 있지만 적절하게 코드들 나눌 필요성이 생겼고 (앱들의 코드가 방대해짐에따라), 적절한 역활과 규칙을 정해 나누기 시작했고, 그과정에 나온 것들이 `MVP`, `MVVP`, `MVC` 등등입니다.

그중에 MVP 는 앱들이 실행되면 Fragment 의 라이프사이클과 이벤트를 통해 화면을 갱신하기위해 필요한 로직들을 Presenter 와 Model 을 만들어 처리 하는것입니다. Fragment( View )는 Presenter 를 통해 Model 에서 데이터를 가져오고, 데이터를 가공을하여  Fragment 로 화면을 그리라고 하는 것입니다. 위의 그림이 이를 나타내는 것입니다.

그리고 처음 말했듯이 MVP 의 장점중 하나로, Presenter 를 안드로이드의 컴포넌트들이 필요한 View, Model 들로 부터 해방시킬수가 있습니다. 그러기 위해 Presenter 에는 View 와 Model 을 interface 를 통해서 통신을 하게 될 것입니다. 그러면 View 와 Model 과는 별개로 비지니스 로직을 테스트 할수 있게 될 것입니다.


자 그럼 위의 말을 그대로 코드로 옮기면 MVP 구조잡는것은 끝 !

<br>

## 마무리
일단 대략적으로 MVP 에 대한 설명을 해보았다. 다음 포스팅에서 구체적으로 코드를 통해 살펴 볼 것입니다. 아래의 동영상 링크를 따라 가시면 동영상 자료를 보실수 있습니다.

<br>

#### 다른 MVP 포스팅 및 샘플 프로젝트
- [Android - MVP Presenter/View/Model Code Lab](http://moka-a.github.io/android/android-mvp-01/) - 차근차근 MVP 구조를 구현해 해봅니다.
- [Android - MVP in action with my app](http://moka-a.github.io/android/android-mvp-02/) - 실제 운영중인 서비스에서 어떻게 사용하고 있는지 살펴봅니다.
- [Android - MVP in action Reference](http://moka-a.github.io/android/android-mvp-03/)
- [MVP 로 작성된 Sample Project - GitHub](https://github.com/moka-a/moka-sample-android)
- [동영상 설명 보기](https://youtu.be/Pydw-dzy2Vg)

<br>
<br>
