---
layout: post
title: "Android | MVP in Action Intro"
author:
published: true
modified:
categories: android
excerpt: 안드로이드의 패턴중 하나인 MVP 패턴에 대해. 샘플 Project 를 통해 실제로 어떻게 사용하는지 살펴 보자. BaseView, BasePresenter 등을 만들고 라이프사이클에 맞추어 동작하도록 하자.
tags: [android, mvp]
image:
  feature:
date: 2016-12-6
---
안드로이드 디자인 패턴 중 하나인 MVP 에 관한 내용입니다. 다른 패턴( MVC, MVCC .. )등 이 있지만 비교는 하지 않고 MVP 에 대해서만 설명할 예정이고, 샘플 프로젝트 및 코드등을 통해 실제 프로젝트에 적용 하기 까지의 과정을 적을 것입니다.

## WHY MVP
내가 생각 하는 MVP 의 가장 큰 장점은 Unit 테스트를 하기에 용이하다는 점이다. 안드로이드 소스코드 들을 테스트하려고 하다보면, 꼭 한번씩 context 가 필요하다던지, 뷰와 연관이 있거나, 디비 접근을 해야한다던지 메소드를 분리시키거나 등 어려움을 겪을수가 있다.

하지만 MVP 를 통해 Presenter 에서는 인터페이스를 통해 객체를 인젝션 하여 서로간의 의존도를 없앨수 있다.
따라서 테스트가 필요한 로직만을 테스트 할수 있게 된다.

<figure>
	<img src="/images/posting_mvp/ig_mvp_01.png" alt="image">
</figure>
위 그림은 MVP 를 그림으로 보여주는것인데, 인터페이스를 통한 MVP 의 과정을 잘 나타내는 듯 하다.

<br>
## HOW TO USE
우리가 짜는 안드로이드 소스코드 동작은 Fragment( or Activity ) 의 라이프 사이클을 통해 시작 한다고 볼수있다. 따라서 제일먼저 Fragment 를 만들게 되고 라이프 사이클에 맞추어 원하는 화면을 그리도록 한다. 원하는 화면을 그리기 위해서는 데이터가 필요하게 되고, 데이터를 가공을 할 필요가 있어진다.

이것을 Fragment 에 다 넣을수 있지만 적절하게 Presenter 와 Model 을 만들어 Presenter 를 통해 Model 에서 데이터를 가지고와 가공을하여 다시 Fragment 로 화면을 그리라고 하면 된다.

위의 말을 그대로 코드로 옮기면 MVP 구조잡는것은 끝 ! 이다.
