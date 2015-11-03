---
layout: post
title: "Effective android 03) Liskov's Substitution"
modified:
categories: android
excerpt:
tags: [android, effective-android]
image:
  feature:
date: 2015-11-03T23:39:55-04:00
---

### 3. Liskov's Substitution

Derived types must be completely substitutable for their base types 
Do not change the base class methods' behavior
Always make super call in fragments/activities


### 3. Liskov's Substitution

파생된 타입들은 기본 타입에 대해서 대체 가능하여야 한다. 
절대 기본클래스의 메소드 동작을 바꾸지마라.
액티비티/프레그먼트에서 항상 super 을 호출 하여야 한다.


### --
객체지향적인 패턴에서 사용되는 법칙인듯하다.

구글 검색해보니 아래 예제가 있다.
{% highlight java %}
// Violation of Likov's Substitution Principle
class Rectangle
{
	protected int m_width;
	protected int m_height;

	public void setWidth(int width){
		m_width = width;
	}

	public void setHeight(int height){
		m_height = height;
	}

	public int getWidth(){
		return m_width;
	}

	public int getHeight(){
		return m_height;
	}

	public int getArea(){
		return m_width * m_height;
	}	
}

class Square extends Rectangle 
{
	public void setWidth(int width){
		m_width = width;
		m_height = width;
	}

	public void setHeight(int height){
		m_width = height;
		m_height = height;
	}

}

class LspTest
{
	private static Rectangle getNewRectangle()
	{
		// it can be an object returned by some factory ... 
		return new Square();
	}

	public static void main (String args[])
	{
		Rectangle r = LspTest.getNewRectangle();
        
		r.setWidth(5);
		r.setHeight(10);
		// user knows that r it's a rectangle. 
		// It assumes that he's able to set the width and height as for the base class

		System.out.println(r.getArea());
		// now he's surprised to see that the area is 100 instead of 50.
	}
}
{% endhighlight %}

기본으로 되는 Retagle 클래스가있고,
그것을 확장하여 Sqaure 클래스를 만들었다.

이제 테스트 코드에서 getNewRectangle() 함수를 이용하여 Square() 객체를 생성하여 리턴시켜준다.
하지만 리턴 타입이 Rectangle 임을 보아야 한다.

이제 여기에 setWidth() 와 setHeight() 함수를 이용하여 값을 넣어주었다.
여기 getNewRectagle() 함수를 얻어온 객체의 타입이 Rectagle 이기때문에 가로 세로가 5, 10 으로 들어가기때문에 getArea() 함수를 호출하면 50이 호출이 될거라 예상할수 있다.

하지만 Squar() 객체를 생성하여 리턴하였기때문에 이 객체가 Square 객체의 함수들을 담고있다.
타입이 Rectagle 이지만 Spuare 객체의 setWidth 와 setHeight 함수가 호출된다는것에 주의해야 겟다.

[출처] [http://orhanobut.github.io/effective-android/](http://orhanobut.github.io/effective-android/)