---
layout: post
title: "Effective android 05) Dependency Inversion"
modified:
categories: android
excerpt:
tags: [android, effective-android]
image:
  feature:
date: 2015-11-17T23:39:55-04:00
---

### 5. Dependency Inversion
High-level modules should not depend on low-level modules. Both should depend on abstractions.
Abstractions should not depend on details. Details should depend on abstractions.

To communicate between activity and fragment, create an interface and use it
To communicate between adapters and fragment/activity, use interface
As a thumb rule, always relies on abstraction.
Use abstractions as much as possible while defining variables,parameters.



### 5. Dependency Inversion
높은 레벨의 모듈들은 낮은 레벨의 모듈과 의존성이 없어야 한다. 두개다 추상화가 잘 되어있어야 한다.
추상화는 의존적이지 않아야 하고, 추상화에 의해 구체화 된다.

액티비티와 프레그먼트사이의 통신에서, 인터페이스를 만들어 사용하는 것과, 어댑터와 프레그먼트/액티비티 와의 통신에서 인터페이스를 이용하는것이 룰이고, 이것이 추상화에 의한것이다.

최대한 변수들과 파라미터를 이용하여 추상화 하라.


### --

{% highlight java %}
// Dependency Inversion Principle - Bad example
// 나쁜 예

class Worker {

	public void work() {

		// ....working

	}

}

class Manager {

	Worker worker;

	public void setWorker(Worker w) {
		worker = w;
	}

	public void manage() {
		worker.work();
	}
}

class SuperWorker {
	public void work() {
		//.... working much more
	}
}
{% endhighlight %}

{% highlight java %}
// Dependency Inversion Principle - Good example
// 좋은예

interface IWorker {
	public void work();
}

class Worker implements IWorker{
	public void work() {
		// ....working
	}
}

class SuperWorker  implements IWorker{
	public void work() {
		//.... working much more
	}
}

class Manager {
	IWorker worker;

	public void setWorker(IWorker w) {
		worker = w;
	}

	public void manage() {
		worker.work();
	}
}
{% endhighlight %}

인터페이스를 이용하여 추상화 시켜놓으면 인터페이스의 함수를 호출함으로써 그 인터페이스를 구현하고 있는 모든 클래스에서 호출되도록 할수 있다. 
하지만 이럴경우, IWorker 를 확장하는 경우 모든 클래스가 영향을 받을수가 있어 문제가 생길위험이 있을수 있다. 따라서 저번시간에 보았던 개념도 잘 적용하여 잘 써야 할듯 하다.

[출처] [http://orhanobut.github.io/effective-android/](http://orhanobut.github.io/effective-android/)