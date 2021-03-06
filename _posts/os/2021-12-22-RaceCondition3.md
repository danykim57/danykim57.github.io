---
title:  "레이스 컨디션 - 3"
header:
  teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"
categories: 
  - os
tags:
  - Concurrency
---
   
- 모니터(Monitors)

  - 동시성에서 레이스 컨디션과 항상 같이 나오는 현상이 교착상태(DeadLock)이다.
  - 세마포어에서도 교착상태가 일어날 수 있는데 세마포어 변수를 낮추는 down 함수가 두번 동시에 일어날 경우에 변수값이 0보다 낮은 값으로 설정되면서 교착상태가 일어날 수 있다.
  - Brinch Hansen과 Hoare가 이에 대한 해결책으로 동기화(Synchronization)의 기본 형태를 제시하였는데 그것이 모니터 이다.
  - 모니터는 프로시저, 변수, 여러 데이터 구조가 합쳐진 덩어리이다. 
  - 메모리에 탑재되어 준비가 된 프로세스는 프로시저(보통 함수의 형태를 하고 있다)를 모니터에 언제든지 불러 올 수 있지만 모니터 안에 있는 데이터 구조들이 모니터 밖에 있는 프로시저들에 의해서 변경되는 것을 막는 형태를 하고 있다.
  - 모니터는 컴퓨터 언어에서 지원을 해주어야 해서 C언어에서는 지원을 하지 않는다. 
  - 모니터는 상호배제를 지키기 위해서 어떤 경우에서든 한 프로세스만이 모니터 안에서 작동을 할 수 있다.
  - 컴퓨터 언어에서 지원이 필요하다는 것은 컴파일러에서 이런 상호배제가 진행된다는 것 이다.
  - 컴파일러 레벨에서 오류 체킹이 진행되면 이 언어를 사용하는 개발자가 실수를 할 일을 미연에 방지를 시키는 효과가 있다.
  - TDD(Test Drivedn Development)에서의 주요 방식 중 하나가 개발자가 잘못된 방식으로 코드를 짤 경우에
  - 컴파일러로 에러를 띄우게 하는 방법이다. 이런 컴파일러 상의 제한사항이 에러와 버그의 발생을 방지시킨다.
  - 이전에 공급자-소비자 문제(Producer-Consumer Problem)에서 보았던 공유자원 접근에 대한 대기열 버퍼가 가득 차면서 오버플로우가 일어나면서 이전에 대기되고 있던 프로세스가 공유자원에 접근할 수 있게되는 사건을 방지하기 위해서 모니터에서는 Pthreads에서 쓰이던 상태변수(conditional variables)를 이용한다.
  - 상태변수를 이용하는 상호배제를 더 세련되게 하기 위해서 Hansen은 잠들어있는 프로세스를 깨운 다른 프로세스가 바로 퇴장되록 하는 방법을 도입하였다. 
  - 이 메뉴얼은 잠든 프로세서를 깨운 프로세스가 계속 활동을 하는 상황을 미연에 방지할 수 있기 때문이다.
  - 이것과는 반대로 잠들어있는 프로세스가 깨어나도 깨운 프로세스가 퇴장하기 전까지 자다깬 프로세스가 활동을 못하게 제한을 거는 방법도 있다.
  - 또한 프로세스가 깨우기 위해서 보낸 신호(singal)이 통신도중에 잘못되는 경우가 있을 수 있는데 이는 프로레스의
  - 생명주기 상태(LifeCycle State)를 관리하면 문제가 될 것이 없다. 


- 모니터를 지원하는 언어인 자바(Java)는 사용자환경에서의 쓰레드와 프로시저인 메소드들을 클래스로 묶어서 사용하는 순수 객체지향 언어이다. 
- Synchronized라는 키워드를 메소드 선언에 이용하여서 해당 메소드, 함수가 돌아가는 와중에는 다른 개체가 끼어들지 못하도록 상호배제를 지원한다.
- 일반적인 모니터와 달리 자바에서의 모니터는 상태변수를 지원하지 않는다.
- 그러나 wait과 notify 메소드를 지원하여서 상태변수의 기능을 대체하도록 하였다. 
- 상태변수의 부제로 자바에서는 우리가 sleep and wait에서 보았던 레이스 컨디션이 일어날 수 있는데 자바에서는 분명한 예외처리(Explicit Exception)를 통해서 이를 해결하였다.

- C나 C++에서는 컴파일러 레벨에서 모니터를 지원하지 않기 때문에 up과 down 함수를 모방한 어셈블리 코드를 라이브러리로 지원을 하여서 동시성을 해결하였다.

- 세마포어는 커널레벨에서 너무 많은 작업을 하여서 효율성이 그렇게 좋지 못하고 모니터는 컴퓨터 언어가 지원하지 않으면 사용하지 못하는 방식이다. 이 문제점들을 해결하기 위한 것이 Message Passing인데 이것은 다음 장에서 알아보자.
   
  
출처: 

Modern Operating Systems 4th Edition by Andrew Tanenbaum