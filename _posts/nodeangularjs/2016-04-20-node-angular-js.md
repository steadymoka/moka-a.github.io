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