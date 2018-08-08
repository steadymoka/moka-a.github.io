---
layout: post
title: "Thread 와 Coroutine"
author:
published: true
modified:
categories: etc
excerpt: Thread 와 Coroutine 에 대해서
tags: [thread, coroutine]
image:
  feature:
date: 2018-7-20
---

## Thread 와 Coroutine 
CPU가 어떠한 프로그램을 처리 할 때, block 이 발생할 수 있는데, 이때 CPU가 놀지 않고 계속 일을 할 수 있게 위해 Thread 와 Coroutine 등의 개념이 생겼다. 

<br>
### Thread
Thread 는 CPU 가 놀지 않게 하기 위해, 여러개의 Thread 의 작업들을 계속해서 왔다갔다 (컨텍스트 스위칭) 하면서 처리를 하는 방식이다. 하나의 쓰레드가 프로그램을 처리를 하다가 블록킹이 되는 시점 (IO 작업 또는 네트워크 작업) 등이 생길수 있는데, 이 때 해당 쓰레드는 블락이 되어 일을 안하지만, 다른 쓰레드에서는 계속 할일이 남아 있다면 CPU 를 놀지 않고 일하게 할수 있다.

하지만 모든 쓰레드가 모두 블록이 되었다면 CPU 는 놀게 될 것 이다. 아래 그림 처럼 모든 쓰레드가 블록이 되었을 상황이다. 

<figure>
	<img src="/images/thread_coroutine_01.png" alt="image">
</figure>

위의 화살표는 여러 쓰레드가 독립된 작업을 동시에 진행하는 것 처럼 보이지만 빠른 속도로 왔다 갔다 하면서 프로그램을 실행한다는 것을 의미하는 그림이다.

<br>
### Coroutine
코루틴은 프로세서의 실행 순서를 직접 컨트롤 있게 해주는 기법이다. 어떤 하나의 작업중에 블록이 되는 상황이 생긴다면 이떄, 해당 프로세서의 실행지점을 다른 코루틴으로 넘기게 된다. 그럼 실행을 받은 코루틴에서 다시 자신의 작업이 끝날때 까지 또는 자신의 작업이 블록이 되기 까지 실행을 하게 되고, 그 시점에서 다시 실행 권한을 넘기게 된다. 이렇게 하여 CPU 가 놀지 않게 하는 개념이다. 

<figure>
	<img src="/images/thread_coroutine_02.png" alt="image">
</figure>

그림과 같이 CPU 가 노는 시간이 없이 쭉 작업을 할 수 있게 되어 효율적으로 자원을 이용할 수 있다. 코루틴의 개념에서의 단점은 하나의 작업에서 다른 코루틴으로 실행권한을 넘겨주지 않으면 다른 작업들이 시작을 할 수 없다는 단점이 있다.

<br>
<br>
