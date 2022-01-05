---
title:  "자바8 함수형 인터페이스"
header:
  teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"
categories: 
  - Java
tags:
  - Java8
---
자바8에서는 함수형 인터페이스를 지원한다.

함수형 인터페이스는 1개의 추상메소드를 가지고 있는 인터페이스이다.

즉, 다른 다수의 메소드는 존재해도 추상메소드는 하나여야만 한다.

함수형 인터페이스가 중요한 이유는 자바에서는 람다식을 사용할 때 함수형 인터페이스를 통해서 사용할 수 있기 때문이다.

람다식은 익명의 함수로써 사용자가 이 함수를 일급 객체(First Class Language Citizen)으로 이용하게 해준다.

자바는 순수 객체 지향 언어로서 기본 단위가 클래스를 바탕으로 하는데  

단지 함수 하나를 생성하고 쓰기 위해서 클래스를 만드는 것은 너무나 번거로운 일이다.

이런 반복적으로 나오는 코드들인 보일러 플레이트 코드를 피하면서 간단하게 사용할 함수만 만들도록 도와주는게 함수형 인터페이스이다.

₩₩₩java
@FunctionalInterface
interface CustomInterface<T> {
    T call();
}
₩₩₩

[^posts]: Footnote test.