---
title:  "데이터베이스 시스템 개요"
header:
  teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"
categories: 
  - BackEnd, DBSM
tags:
  - DB
---
 데이터 베이스 관리 시스템(Data Base Management System)은 편리하고 효율적으로 서로 '관계'가 있는 데이터의 보관, 관리, 활용을 도와주는 시스템이다.
 
 데이터 베이스는 데이터들의 모임이다. 
 
 사용자에게 편리하고 효율적인 데이터 베이스가 충족할 원칙들이 있다.
 
 1. 원자성(Atomicity): 어느 과정(Process)이 다 수행되던지 수행되지 않아야 한다.
 2. 고립(Isolation): 데이터가 흩어지지 않고 잘모여있어서 데이터의 활용이 용이하여야 한다.
 3. 일관성(Consistency): 가능한 중복된 데이터가 없고 수정된 데이터가 바로 적용되어야 한다.
 4. 무결성(Integrity): 데이터는 주어진 제약조건인, 일관성 조건(Consistency Constraint)을 만족해야한다.
 5. 동시 액세스 문제: 서로 다른 프로그램이나 함수가 동시에 데이터를 수정하는 일이 일어나면 예상된 계산 값이 나오지 않고 다른 값이 나와서 동시 액세스가 일어나서는 안된다.
 6. 보안 문제: 권한을 가지지 않는 사용자가 데이터에 접근하거나 수정하는 일이 있어서는 안된다. 
 
 
 데이터 베이스 관리 시스템은 이해를 돕기 위해 레이어드 방식으로 추상화된 디자인으로 설계되었다.
 
 1. 물리적 단계(Physical level): 물리적 단계에서 데이터를 어떻게 저장하는지 기술하는 층.
 2. 논리적 단계(Logical level): 어떤 데이터가 논리적으로 관계되는지 기술하는 단계. 아래인 물리적 단계를 알필요는 없다.
 3. 뷰 단계(View level): 사용자가 간단하게 이용할 수 있도록 일부분의 데이터만 보여주는 층.

인스턴스와 스키마
인스턴스(Instance): 특정 시간의 데이터들의 모임.
스키마(Schema): 데이터베이스의 설계.

데이터 모델
1. 관계형 모델(Relational Model)
2. 개체-관계 모델(Entity-Relationship Model)
3. 객체-기반 데이터 모델(Object-oriented Data Model)
4. 객체 관계형 데이터 모델(Object-relational Data Model)
5. 반구조형 데이터 모델(Semistructured Data Model)
6. 네트워크형 데이터 모델(Network data model)
7. 관계형 데이터 모델(Hierarchical data model)

이와 같이 데이터 모델은 다양하지만 대부분은 스테디 셀러인 관계형 모델과 반구조형 데이터 모델이 쓰인다.

그외에도 NoSQL이라는 비정형 데이터 모델도 있지만 데이터 베이스의 특성인 ACID(Atomicity, Consistency, Integrity, Durability)를 충족시키지 못하기에
이 시리즈에서는 생략하도록 하겠다.

데이터 베이스 구조
1.중앙 집중 방식
2. 클라이언트-서버 방식
-2계층 구조(Two-tier Architecture): 클라이언트의 응용 프로그램이 직접 데이터 베이스에 접근하는 구조
-3계층 구조(Three-tier Architecture): 클라이언트의 응용 프로그램은는 전처리와 요청만 하고 서버 측의 응용 프로그램이 비즈니스 로직을 거쳐 데이터 베이스에 접근하는 구조

데이터 언어
데이터 조작 언어(Data Manipulation Language)
데이터 정의 언어(Data Definition Language)

SQL(Structured Query Language)은 조작과 정의 둘다 할 수 있는 언어이다.
