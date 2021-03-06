---
title:  "자바 lambda"
header:
teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"
categories:
  - java
tags:
  - BackEnd
---

 람다식은 작은 코드 덩어리로서 파라미터를 받아서 리턴값을 돌려준다.
 
람다는 함수와 비슷하지만 이름(signature)가 불필요하고 함수의 바디 부분에

바로 구현이 가능하다.

문법

```
parameter -> expression
```

다수의 파라미터를 쓰는 경우에는 괄호로 묶어야한다.

```
(parameter1, parameter2) -> expression
```

항상 즉시 리턴값을 반환하야여하고 변수나 flow control인 if나 for을 사용할 수 없다.

람다식의 예시는 다음과 같다.

```java
import java.util.ArrayList;

public class Main {
  public static void main(String[] args) {
    ArrayList<Integer> numbers = new ArrayList<Integer>();
    numbers.add(1);
    numbers.add(2);
    numbers.add(3);
    numbers.forEach( (n) -> { System.out.println(n); } );
    //컨수머를 사용할 경우 다음과 같이 이용할 수 있다.
    //컨수머는 한개의 함수만을 포함하고 있어서 이런식으로 값을 저장할 수 있다.
    //이러한 클래스를 single-method-interface(SMI)라고 한다.
    //Consumer<Integer> method = (n) -> {System.out.println(n); };
  }
}
```