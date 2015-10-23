---
layout: post
title: "안드로이드 노트2 - 2015.10.23"
modified:
categories: android
excerpt:
tags: [android, actionbar]
image:
  feature:
date: 2015-07-27T19:39:55-04:00
---
<br>
<br>

#### Glide 에서 콜백 받기
Glide 란 이미지 로드에 쓰이는 라이브러리 ( 구글에서 개발중이다. )
사용법은 piccaso 와 비슷하며 매우 간단하다.

이번에 소개할 내용은 glide 에 콜백을 받는 것에 대한것이다. <br>

###### bitmap 자원 콜백 

{% highlight java %}
   Glide.with( getContext() )
		.load( imageUrl )
		.asBitmap()
		.fitCenter()
		.into( new BitmapImageViewTarget( imageView_image ) {

			@Override
			protected void setResource( Bitmap resource ) {

				super.setResource( resource );
			}

			@Override
			public void onLoadFailed( Exception e, Drawable errorDrawable ) {

				super.onLoadFailed( e, errorDrawable );
			}

		} );
{% endhighlight %}

asBitmap() 함수를 호출한후 into 에 BitmapImageViewTarget 의 객체를 넘겨주어 콜백을 받으며 된다.
setResource 는 이미지를 로드한후 targetView 에 bitmap 을 set 하려고 불리는 함수이다.

위에 불린 함수 말고도 override 목록을 보면 다양한 콜백을 받을수있다.

###### drawable 자원 콜백 
Bitmap 을 받을 필요가 없다면, GlideDrawableImageViewTarget 객체를 넘겨주어 위와 같은 콜백들을 받을수 있다. 단, 여기서는 bitmap 이 넘어오는게 아니라 drawable 객체가 넘어온다.

<br>
<br>
<br>
<br>

#### 이미지 로딩중 표시하기
이미지 로딩중 progressBar 또는 placeHoldImage 를 띄워야 하는 경우가 있다. 아무것도 안띄우면 UX 가 매우 구리다.
이때 glide 를 사용하여 위의 콜백과 혼용해서 progressBar 를 띄우거나 placeHold 이미지, 또는 error시 띄울 이미지를 띄울수 있다.

사실 glide 자체적으로 placeHold 이미지와 error 이미지를 등록하는 기능이 제공되는데, ImageView 에 이미지가 들어가는 형태로 되면서 내가 원하는 방향으로 하기가 번거로웠다. 그래서 relativeLayout 을 이용하여 placeHold 이미지와 error 이미지를 겹쳐두고 위의 callback 함수들을 이용하여 사용하였다.

<br>
<br>
<br>
<br>

#### 이미지 로딩후 imageView 에 셋팅하기
이미지를 원경저장소에서 저장해두고 받아와서 set 해줄경우 내가 원하는 크기와 scaleType 으로 하는것이 매우 어렵다. 공감해주길 바란다..
가장 좋은 것은 image 의 비율을 이용하여여 imageView 의 가로나 세로의 길이를 고정해두고 받아온 이미지의 비율에 맞춰 imageView 의 layoutParams 를 이용해 크기를 조절해준후 받아온 이미지를 set 해주는 방식이 제일 베스트 인듯 하다. 

이렇게 하기 위해서 위의 glide 의 콜백들을 이용하여 bitmap 을 받아와 이미지의 크기를 알아내고, imageView 의 크기를 결정해준후 fitXY 속성을 주는것이 제일 깔끔한듯 하다.


<br>
<br>


