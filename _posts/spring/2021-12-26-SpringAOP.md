---
title:  "스프링 AOP"
header:
  teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"
categories: 
  - java
tags:
  - Spring
---

  Spring은 자바 기업용 어플리케이션을 위한 프레임워크이다. 기존에 J2EE라는 자바 기업용 어플리케이션을 위한 프레임워크가
  
있었지만 Sun Microsystem이 만들고 오라클이 구현하였던 J2EE는 전문적이지만 개발자들에게 사용상의 편의성이 안좋았기에

그에 대한 오픈소스로 나온 것이 Spring이다. 

 자바는 순수 객체지향 언어로서 모든 클래스들이 Object라는 클래스를 슈퍼클래스로 가지고 있다. 즉, 모든 클래스는 Object라는
 
클래스를 상속한다.
 
 자바가 추구하는 철학 중 하나가 객체 중심적으로 사고를 하도록 유도하고 기존에 있는 코드들이 계속 재활용이 되도록 하는 부분이다.
 
Spring도 이 점을 받아들여서 POJO(Plain Old Java Object)라는 개념 아래에서 자바가 지원하는 객체 클래스들을 

가능한 최대한 재활용한다.

 그러면서 기업용에 알맞게 사업상에 필요로 하는 여러 시각(Perspective)들의 요구에도 응해야 하는데 이를 위해서 나온 개념이
 
AOP(Aspect Oriented Programming)이다. 

 자바는 새로운 것을 만드는 것보다 기존에 있는 것을 활용하도록 즉, 객체 지향적으로 개발자들이 언어를 사용하도록 설계가 되어있다.
 
특정 제한들은 사용자들을 선택 딜레마에 빠지지 않게 하여 유용하지만 이 객체 지향적인 부분이 사용자들의 편의성을 해치는 방향이

될 수 있다는 점이 문제가 되었다. 객체들 간의 관계는 수직적이다. 부모-자식 관계처럼 수직 관계는 관리와 운영이 용이하지만

사촌과 같은 수평적인 관계에서는 약하다. 우리가 가족 관계를 알아볼때 사촌과 오촌과 같은 관계가 항상 부모님과 조부모님을 거쳐서

생각된다는 점을 생각하면 조금 더 쉬울 수 있겠다. 사업상에서는 가족관계도와 같은 자바의 객체지향적인 부분도 중요하지만

수평적 관계인 한 세대간의 관계도 중요하다. 

 AOP는 이런 수평적 관계를 관리하는 유용한 도구이다. AOP 자체가 직관적으로 설명이 힘든 것이 객체지향 프로그래밍을
 
보완해주기 위해서 나온 개념이기에 개체간의 관계에서의 방향성이 다르기 떄문이다. OOP는 코드관리를 위한

기술적인 관점에서의 접근법이고 AOP는 비즈니스와 도메인부분에서 일어나는 문제들을 해결하기 위한 접근법이다.

 AOP로 여러 다른 고려사항(Crosscutting concerns)들을 객체간의 상속관계를 무시하고 기능들을 넣을 수 있다는점이 굉장히
 
매력적이다. 예를 들어서 데이터 베이스의 조회나 수정을 요구하는 트랜스액션이나 시간을 재거나 타임스탬프를 남기는 것과 같이

부가기능들을 원하는 대로 여러 클래스에 동시에 적용시켜서 일일이 클래스들을 수정시키는 작업을 하지 않아도 된다.

 Spring에서의 AOP는 다른 프레임워크에서의 AOP와 사뭇 다른덴 그 이유는 Spring은 AOP를 제어역전(IoC)를 보완하기 위한
 
개념이지 AOP가 주가 되기 위해서 쓰이는 개념이 아니기에 그렇다.

 Spring 2.0은 최대한 다른 라이브러리 기능들이 함께 이용되는 것을 장려한다. Spring은 AOP 개념을 
 
스키마식 접근(schema-based approach, with XML)과 자바의 AspectJ 어노테이션 스타일(AspectJ Annotation Style)를 둘 다 지원한다.



 AOP에는 다음의 용어들이 있다.(Spring에서의 AOP가 아니다 AOP 개념에 대한 용어들이다)
 
Aspect: AOP의 모듈성을 위한 단위이다. 직역으로 시각, 시야와 같이 번역될 수 있다.

Join Point: 메소드의 실행 중의 특정 지점을 뜻한다. Spring AOP에서 join point는 항상 메소드의 실행을 뜻한다.

Advice: Join Point에서의 Aspect를 뜻한다. 중간에 실행을 가로채는 Interceptor라고도 불린다. 'before','after','around'가 있다.

Pointcut: Join Point의 술어(문장에서 동사가 들어가는 부분)형태이다. 보통 메소드의 이름을 뜻한다. 

Introduction: 새로운 메소드나 필드를 선언하는 것을 뜻한다. 새로운 인터페이스를 넣는 것 또한 Introduction이라고 한다.

Target Object: 하나 이상의 Aspect에 의해 조정된(advised) 객체를 뜻한다.

AOP Proxy: AOP Aspect의 메소드를 실행시키기 위해서 생성된 객체를 뜻한다. Spring AOP에서는 JDK dynamic proxy와 CGLIB proxy가 있다.

Weaving: 조정된 객체(advised object)를 만들기 위해서 다른 어플리케이션 타입과 객체들을 Aspect들로 엮는 것을 뜻한다.

Weaving은 컴파일, 로드, 실행시간에서 일어날 수 있게 할 수 있다.

  예제 실행코드는 위의 스프링 문서화에서 찾아 볼 수 있다.


출처:

https://docs.spring.io/spring-framework/docs/2.5.x/reference/aop.html

https://docs.oracle.com/javase/7/docs/api/java/lang/Object.html


