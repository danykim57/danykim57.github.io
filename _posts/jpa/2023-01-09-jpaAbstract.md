---
title:  JPA이란
header:
teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"
categories:
  - spring
tags:
  - Spring
---

 JPA(Java Persistence API)는 자바에서 쓰이는 ORM(Object-Relational Mapping) 기술 표준이다.
 
- JPA는 실제 구현체가 아니고 규격이다.

 JPA의 대표적인 구현체에는 Hibernate가 있다.
 
자바는 객체 지향 프로그래밍(Object-Oriented Program) 언어로서 대표적인 데이터 베이스의 한종류인

관계형 데이터베이스(Relational Database)와는 만들어진 목적과 메커니즘이 다르다.

 이 중간 단계에서 둘을 이어주는 역할을 하는 것이 ORM 이다.
 
JPA의 장점

 - 자바 언어로 쿼리를 조작하여서 보낼 수 있다. 선언적 언어인 SQL을 안써도 된다.
 - 매핑하는 Entity 클래스를 만들어주면서 ERD에 신경을 덜 써도 된다.
 - 스키마 변경이 일어나도 좀 더 유연하게 대처가 가능하다.
 - 한 데이터베이스 언어에 종속되지 않고 변경이 쉽게 하능하다.

JPA의 단점
 - 학습 곡선이 처음에 높다.
 - 객체 지향 프로그래밍과 관계형 데이터베이스의 특징을 모르면 오히려 안쓰니만 못할 수 있다.
 - Join을 많이써야하는 경우에는 JPQL을 써서 SQL을 결국은 짜주어야한다.
 - 잘못된 설계가 성능저하를 불러올 수 있다.


개인적으로 JPA를 써야하는 가장 큰 이유 2가지
 - 개발자에게 일어나는 선언적 언어인 SQL과 명령적 언어인 자바간의 컨텍스트 스위치를 없앨 수 있다.
 - 데이터베이스 스키마 변경에 굉장히 효율적으로 대응할 수 있다.
