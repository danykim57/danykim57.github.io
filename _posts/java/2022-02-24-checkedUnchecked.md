---
title:  "체크드, 언체크드 예외 - Checked and Unchecked Exception"
header:
teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"
categories:
  - java
tags:
  - BackEnd
---

  자바에서 메소드를 실행시켰을 때 의도하지 않은 경우인 예외가 생겼을 때 메소드는 런타임 시스템에 Exception 객체를 넘겨주게 된다.
  
이 Exception 객체는 해당 메소드가 실행했을 때의 상태(state)와 같은 발생한 에러에 관련된 정보를 가지고 있다.

이런 예외 객체를 다루는 과정을 throwing an exception이라고 한다.

  아래는 자바의 Exception 객체가 throwing되는 것을 보여주는 그림들이다.

{% include figure image_path="/assets/images/ExceptionCallStack1.png" alt="CallStack" caption="CallStack" %}



{% include figure image_path="/assets/images/ExceptionCallStack2.png" alt="CallStack2" caption="CallStack2" %}

 Exception 클래스는 크게 두가지로 나눌 수 있는데 하나는 체크드 예외(Checked Exception)와 언체크드 예외(Unchecked Exception)이다.

이 두 단어에는 주어가 빠져있는데 단어를 풀어서 보면 컴파일러가 체크하는 예외와 컴파일러가 체크하지 않는 예외라고 볼 수 있다.

체크드 예외는 컴파일러 시점에 언체크드 예외는 JVM의 런타임 시점에 발생된다.

즉 언체크드 예외는 런타임 예외(RuntimeException), 에러(Error), 그리고 런타임 예외를 상속받는 서브 클래스들이다.

예외 클래스를 크게 이렇게 나눈 이유는 프로그램을 돌리는 중에서 JVM 안에서 런타임에 일어나는 예외나 에러는 NullPointerException, ArithmeticException,

BufferOverflowException,  IllegalArgumentException, AnnotationFormatError, AssertionError, AWTError등이 있는데

이런 예외와 에러들의 공통점은 대부분의 경우에 기존의 Catch handling Exception과 같이 따로 예외 처리를 할 수 없는 경우들이다.

자바 언어를 디자인한 사람이 애초에 자바 API Client를 만드는 자바 개발자가 그 자바 코드를 수정하여서 해결할 수 있는 예외와 에러가 아니기에 건드리지도 말라는 식으로

만든 분류법이다.

 Checked Exception, Unchecked Exception과 세트로 나오는 질문이 스프링의 트랜잭션에서 예외가 발생할 때 롤백을 하느냐 아니냐라는 질문이 나온다.

트랜잭션은 처음부터 끝까지 다 완료되어야 하는 일련의 행동들의 덩어리라고 생각하면 된다.

그래서 트랜잭션은 상황에 따라서 다른 것을 지칭하게 되는데 스프링 프레임워크 트랜잭션 매니지먼트에서는 Java Transaction API(JTA), JDBC, Hibernate, Java Persistence API(JPA),

Java Data Object(JDO)와 같이 API에서 쓰이는 데이터의 조회, 수정에 대한 일련의 행동들의 덩어리로 보도록 하자.

해당 문제는 좀 더 추가 설명이 필요한데 왜냐하면 예외상황이 발생하였을 때 롤백을 할 것 이냐 아니냐는 사용자가 정할 수 있다.

그러나 기본값은 unchecked Exception은 트랜잭션이 롤백하도록 checked Exception은 롤백을 하지 아니한다.

"In its default configuration, the Spring Framework’s transaction infrastructure code only marks a transaction for rollback in the case of runtime,
unchecked exceptions; that is, when the thrown exception is an instance or subclass of RuntimeException. 
( Errors will also - by default - result in a rollback). 
Checked exceptions that are thrown from a transactional method do not result in rollback in the default configuration."

처음부터 끝까지 처리되어야하는 트랜잭션이 도중에 에러나 예외로 멈추면 처음상태로 돌아가는 것이 올바르기에 이렇게 만든 것 같지만

실제로스프링에서 기본값을 이렇게 한 이유는 이전에 EJB(Enterprise Java Bean)에서 이렇게 하였기 때문이라고 한다.



출처: [Oracle Java Tutorial](https://docs.oracle.com/javase/tutorial/essential/exceptions/index.html)
[Spring.io doc 3.2.0.RC1 Transaction] (https://docs.spring.io/spring-framework/docs/3.2.0.RC1/reference/html/transaction.html)
[Spring.io doc 4.2.x Transaction] (https://docs.spring.io/spring-framework/docs/4.2.x/spring-framework-reference/html/transaction.html)