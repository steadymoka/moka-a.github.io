---
layout: post
title: "안드로이드) 노트2 - 2015.10.23"
modified:
categories: android
excerpt:
tags: [android]
image:
  feature:
date: 2015-10-23T19:39:55-04:00
---

#### Glide 에서 콜백 받기
Glide 란 이미지 로드에 쓰이는 라이브러리 ( 구글에서 개발중이다. )
사용법은 piccaso 와 비슷하며 매우 간단하다.

이번에 소개할 내용은 glide 에 콜백을 받는 것에 대한것이다. <br>

###### Glide, bitmap 자원 콜백 

``` java
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
```


asBitmap() 함수를 호출한후 into 에 BitmapImageViewTarget 의 객체를 넘겨주어 콜백을 받으며 된다.
setResource 는 이미지를 로드한후 targetView 에 bitmap 을 set 하려고 불리는 함수이다.

위에 불린 함수 말고도 override 목록을 보면 다양한 콜백을 받을수있다.

###### Glide, drawable 자원 콜백 
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
<br>
<br>

#### 이미지 리스트 (그리드뷰) pinterest 처럼 표시하기
이미지목록 또는 카드목록을 표시할 경우, 핀터레스트 처럼 가로/세로 비율에 맞춰서 세로길이가 자동으로 변하게 셋팅하고 싶을 때,
recyclerView 에 layoutManager 를 StaggeredGridLayoutManager 로 set 해주면 아이템뷰의 세로길이를 사진 비율에 맞게 해주면 자동으로 그아이템의 세로길이가 조절된다. 

<br>
<br>
<br>
<br>

#### 쓰레드/ 루퍼, 메시지 큐
메인쓰레드는 모두가 알것이다. 메인 스레드에는 루퍼와 메시지 큐 객체가 내부적으로 있다. 이 두 객체는 작업 스레드에서 메인 스레드로 작업을 전달하기 위한 객체이기도 하다.

보통 스레드는 run 함수 하나를 가지며, run 처리가 끝나면 스레드의 생명도 끝이다. 메인스레드도 run 함수가 있으며, 그 안에는 루퍼객체를 생성하고, loop 함수를 호출하여 무한 반복을 하며 처리를 시작한다. 루퍼가 생성되면 메시지 큐를 하나 가지게 되고 여기에 작업들이 계속 싸이고 처리를 하게된다.  따라서 다른 작업 스레드에서 메인스레드로 작업을 넘기고 싶다면 이 메시지 큐에 작업을 넣어주면 되는 것이다. 

여기서 메인스레드의 루퍼는 static 객체로 생성되어 어디서든 참조할 수 있다. 

작업스레드에서 메인스레드로 작업을 넘길수 있도록 해주는 것이 핸들러이다. 핸들러를 생성하여 sendMessage 함수를 이용하여 메시지큐로 보낼수있다. 메시지를 만들어 runnable 객체를 넘겨주는 방법 이외에도 핸들러 클래스의 재정의 함수 handleMessage 를 사용할 수도 있다. handleMessage 를 이용하는 방법은 what 멤버변수를 이용하여 여러가지 일을 구분하여 처리할때 유용할수 있다.

<br>
<br>
<br>
<br>

#### Image 가 메모리 얼마나 먹을까 ?
ARGB 이미지 1px -> 4Byte 메모리 차지
100 * 100 px 의 이미지라면, 40000 Byte 를 차지한다.
40000 Byte 면 40 KB : 보통의 아이콘 이미지가 100px * 100px 부근 이다

1000 * 1000 px 의 이미지라면 4000 KB , 4 MB 를 차지하게 된다. 

<br>
<br>
<br>
<br>

#### 안드로이드 GC 와 메모리 관리에 관해
안드로이드는 null, clear 등으로 참조를 끊는다고 해서 바로 gc 가 일어나지 않는다. 
일반적인 int/long/String 으로는 메모리를 OOM 날정도로 많이 올라갈일이 없다. 
메모리를 많이 차지하는 것으로는 네트워크나 파일IO 이고 가장 큰것은 이미지이다. 
이미지 대신 canvas를 이용하는게 리소스를 적게 먹는 방법이다.

4.x.x 버전 부터는 이미지가 화면에 노출되는 것이 아니면 안드로이드 OS 에서 자동으로 recycle 시키도록 되어 gc 의 대상이 바로 된다. 








