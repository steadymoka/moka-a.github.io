---
layout: post
title: "NodeJS / AngularJS NOTE"
modified:
categories: javascript
excerpt:
tags: [javascript]
image:
  feature:
date: 2016-04-20T19:39:55-04:00
---

#### 객체 복사
``` javascript

 var object = { ... };
 var copyObject = Object.assign( {}, object );

/*
    객체를 복사하는 방법은 이것외에도 많아 있다. 
    성능 같은것을 고려하여 한번더 정리가 필요하다.
*/
```

<br>

#### absolute 로 된 영역안에서만 스크롤 되도록 
예를들어 페이스북 알림창 같은 일정한 영역안에서만 스크롤이 되고 스크롤이 다되면 본문의 페이지가 스크롤 되지 않아야 할경우 처리

``` javascript
angular.element( '#mCSB_1' ).bind( 'mousewheel', function( e ) {

    if( (this.scrollTop == 0 && e.originalEvent.deltaY < 0) || (this.scrollTop == 287 && e.originalEvent.deltaY > 0) )
        e.preventDefault();
});
```

<br>

#### 루프 연산자
``` javascript
for( var i; i < length; i++ ) {
    var item = array[i];
    ...
}
// 가장 기본적인 for 문, 대부분의 언어의 문법과 동일

for( var item in array ) {

}
// for 문 진화 형태 ?

array.forEach( function( item, index ) {
    ...
} )
// 배열을 순차적으로 돌면서 function 함수 실행, `리턴 값이 없다``

array.map( function( item ) {
    return ..
} )
// map 으로 넘겨준 함수로 array 의 아이템이 하나씩 들어오면 리턴값 들로 `새 배열을 리턴`한다

array.filter( function( item ) {
    return true; // or false
} )
// 콜백 함수가 true를 반환하는 것만 모아서 `새 배열을 리턴`한다.

```

<br>

#### node 80번 포트로 pm2 돌릴때 루트권한 필요할경우
`sudo setcap cap_net_bind_service=+ep /usr/sbin/node`


