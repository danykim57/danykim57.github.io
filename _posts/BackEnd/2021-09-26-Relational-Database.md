---
title:  "관계형 데이터베이스 개요"
header:
  teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"
categories: 
  - BackEnd, DBMS
tags:
  - DB
---

관계형 데이터베이스 용어:

관계형 데이터베이스는 행과 열로 이루어진 테이블의 모임이다.

테이블의 각행은 값 사이의 관계를 표현한다.

테이블은 릴레이션(relation)이라는 수학적인 개념과 관련이 있다.

테이블은 릴레이션(relation)이라고도 불린다.

테이블의 행은 튜플(tuple)이라고도 불린다.

테이블의 열은 속성(attribute)이라고도 불린다.

행들 또는 튜플들의 특정 집합을 릴레이션 인스턴스(Relation instance)라고 한다.

열들 또는 속성들의 특정 집합을 도메인(domain)이라고 한다.

도메인은 원자적(atomic)이어야 한다. 다른 말로, 더 이상 도메인의 요소들이 나누어질 수 없어야 한다.

도메인의 원자성은 권장 사항이지 필수 사항은 아니다. 원자적이지 않는 도메인을 허용하는 관계형 데이터 모델의 설계도 있다.

데이터베이스의 논리적 설계를 데이터베이스 스키마(database schema)라고 한다.

특정 시간 때의 데이터베이스의 데이터의 저장 파일, 스냅샷을 데이터베이스 인스턴스(database instance)라고 한다.

변수의 값이나 데이터베이스의 속성을 변경 할 수는 있지만 일반적으로는 스키마를 변경하지는 않는다.

그러므로, 가능한 처음에 스키마의 설계가 잘되어야 한다.

키:

한 테이블, 릴레이션 안에서 각 행, 투플을 유일하게 구별 할 수 있는 속성값을 수퍼 키(superkey)라고 한다. e.g) id 속성

수퍼 키는 각 행의 구별을 위해서 관계가 없은 속성이 수퍼 키의 부분집합이 될 수 도 있다.

이런 수퍼 키의 부분집합이지만 수퍼 키가 아닌 최소한의 요건을 가진 키를 후보 키(candidate key)라고 한다.

데이터베이스 설계자에 의해 선택된 후보 키를 주 키(primary key)라고 한다.

다른 테이블을 참조하는 키를 외래 키(foreign key)라고 한다.

외래 키의 생성으로 참조하는 릴레이션(referencing relation)과 참조되는 릴레이션(referenced relation)이 생긴다.

참조 무결성 제약조건(referential integrity)란 참조하는 릴레이션에 투플의 특정 속성에 있는 값이 참조되는 릴레이션에 특정 속성의

하나 이상의 투플이 있어야 한다는 것이다.

다른 말로, 참조하는 테이블의 행의 한 열의 값이 참조되는 테이블의 행의 한 열의 값과 하나 이상이 같아야 한다.



스키마 다이어그램(schema diagram): 데이터 베이스의 논리적 설계를 시각적으로 표현한 것. 데이터베이스의 릴레이션, 속성, 주 키, 외래 키가 표현되어있어야 한다.





널(Null) 값

널(Null)은 값이 없거나 알려지지 않은 특별한 값이다.

1. 널값은 비교가 조금 더 복잡하다. e.g) 1 < NULL 그래서 SQL에서 널값을 비교 할 경우 unknown으로 처리하는데 true나 false외의 논리값을 생성한다.

is null 이나 is not null을 써야만 한다.

2. SQL에서 select distinct 절을 사용할 때 중복된 투플들을 제거해주어야 null을 가진 다른 값들을 다 가져올 수 있다.

관계형 데이터 베이스 모델에서는 위와 같이 널을 이용할 경우 복잡해져서 널의 이용을 추천하지 않는다. 

반대로 ORM(Object Relational Mapping) 기술들에서는 널이 유용하다. 이건 자바의 ORM인 JPA를 포스팅 할때 추가 하겠다.





[^posts]: Footnote test.