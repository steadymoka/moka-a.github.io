---
layout: post
title: "Effective android 04) Interface Seggregation"
modified:
categories: effective-android
excerpt:
tags: [android, effective-android]
image:
  feature:
date: 2015-11-04T23:39:55-04:00
---

### 4. Interface Seggregation
Clients should not be forced to depend upon interfaces that they don't use

Create specific interfaces for each feature. Assume that you have a listener which is optional, make it another interface
Split interfaces



### 4. Interface Seggregation
사용하지 않는 인터페이스에 의존되게 하면 안된다.

각각에 맞는 인터페이스를 만들고, 선택적인 리스너를 가지는것을 있다면, 다른 인터페이스를 만들어라.
인터페이스를 구분해라.



{% highlight java %}
// interface segregation principle - bad example
interface IWorker {
	public void work();
	public void eat();
}

class Worker implements IWorker{
	public void work() {
		// ....working
	}
	public void eat() {
		// ...... eating in launch break
	}
}

class SuperWorker implements IWorker{
	public void work() {
		//.... working much more
	}

	public void eat() {
		//.... eating in launch break
	}
}

class Manager {
	IWorker worker;

	public void setWorker(IWorker w) {
		worker=w;
	}

	public void manage() {
		worker.work();
	}
}
{% endhighlight %}

나쁜예이다.
IWorker 라는 인터페이스에 work(), eat() 가 한번에 정의되어 있다.
그리고 Worker , SuperWorker 가 각각 구현하고 있다. 여기까지는 갠찮다. 하지만 Robot 이라는것을 IWorker 를 구현한다고 하면, eat 이라는것은 필요가 없다.
이런것을 고려하여 인터페이스를 구분하여 work 를 정의하는것과 eat 을 정의하는것 따로 만들라는 소리이다.

{% highlight java %}
// interface segregation principle - good example
interface IWorker extends Feedable, Workable {
}

interface IWorkable {
	public void work();
}

interface IFeedable{
	public void eat();
}

class Worker implements IWorkable, IFeedable{
	public void work() {
		// ....working
	}

	public void eat() {
		//.... eating in launch break
	}
}

class Robot implements IWorkable{
	public void work() {
		// ....working
	}
}

class SuperWorker implements IWorkable, IFeedable{
	public void work() {
		//.... working much more
	}

	public void eat() {
		//.... eating in launch break
	}
}

class Manager {
	Workable worker;

	public void setWorker(Workable w) {
		worker=w;
	}

	public void manage() {
		worker.work();
	}
}
{% endhighlight %} 

결론은 이런식으로 해야 좀더 유연하고, 확장 가능하게 개발이 될수있다는 것이다.
현재 개발된 프로젝트를 생각해보니, 인터페이스를 구현하고 있는 부분에서 ignore 주석 처리해논것이 몇몇 생각이 난다. 또한 어떤 조건에 따라 다른 구현을 하도록 한것도 생각이 난다.
다음번에 이부분을 인퍼페이스를 분리하여 좀더 명확한 코드로 리펙토링 해야 겠다.

[출처] [http://orhanobut.github.io/effective-android/](http://orhanobut.github.io/effective-android/)