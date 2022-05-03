---
title:  "자바와 Boilerplate"
header:
teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"
categories:
  - java
tags:
  - BackEnd
---
  1990년대 초반까지 C언어는 여러가지 불편한 점이 있었다. 자바는 C언어가 가지고 있는 여러가지 문제점들을

해결하기 위해서 나온 언어인데 재미있는 점은 간단한 "Hello, World"를 출력시키는데도 너무나도 복잡해 보인다는 것 이다.

```cpp
//C에서의 Hello, World!
#include <stdio.h>
int main() {
   printf("Hello, World!");
   return 0;
}
```

```java
//자바에서의 Hello, World!
class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, World!"); 
    }
}
```


```python
//파이썬에서의 Hello, World!
print("Hello, World")
```

C언어로 된 프로그램의 시작점을 알리는 최소 단위는 함수이다.

파이썬은 애초에 스크립트 언어처럼 쓰이기 위해서 나온 언어라서 더더욱 간단하다.

그에 비해서 자바로 된 프로그램의 시작점을 알리는 최소 단위는 클래스이다.

10년 전에 자바 책을 첫 몇 페이지를 보자마자 쓰레기통에 던져버리고 한번도 안본 이유가

도대체 public이 무엇이고 static이 무엇이고 void는 또 어떤 것이며 왜 출력함수가

System.out.println()로 길어야 되는가 등 너무나도 복잡해 보이는 문법 때문이였다.

영어를 공부하는 것으로 비유하자면 일단 기본 회화를 배워야하는데 문법책부터 꺼내서

외워라고 강요당하는 느낌이다.

객체 지향 프로그래밍이 추구하는 좀 더 분명하게(explicitly) 표현해주고

각각의 코드 덩어리들 간의 불필요한 관계를 끊어주면서(decoupling)

코드의 수정을 용이하게 한다는 컴퓨터 언어의 디자인이 비대한 형식(boilerplate)을 가져왔다.

Constructor, getter, setter, 등은 더더욱 코드를 더 비대하게 만들었고

다양한 디자인 패턴들인 builder, strategy, factory등 더더욱 비대해졌다.

이것을 해결해보겠다고 자바의 annotation에 런타임 동안에 돌아가는 기능을 넣어놓으면서도

코드는 여전히 비대하다.

밑의 코드는 Lombok을 사용하여서 boilerplate를 그나마 많이 줄여놓은 예제 코드이다.
```java
@Entity
@Getter @Setter @NoArgsConstructor
public class User implements Serializable {

    private @Id Long id;

    private String firstName;
    private String lastName;
    private int age;

    public User(String firstName, String lastName, int age) {
        this.firstName = firstName;
        this.lastName = lastName;
        this.age = age;
    }
}
```

정말 뛰어난 참가자들이 많은 자바의 커뮤니티가 아니였으면 굳이 자바를 선택할 이유가 없다고 생각이 든다.

최근의 구직 조건들을 보더라도 자바의 프레임워크에 대한 이해도는 요구하면서 코틀린이나 파이썬 언어의

이해도를 요구하는 구직문들이 늘어나는 추세이다. 

자바로 프로그램을 짜야하는데 boilerplate가 너무 많아서 인텔리제이와 같은 IDE를 써야하는데

이 IDE가 이펙티브 자바를 참고하여서 만들어졌기 때문에 문제(boilerplate)를 해결하기 위한 학습(IDE)을 위한 학습(Effective Java)을 하여야하는

상황이 되어있다.

이런 상황이면 이미 자바 커뮤니티 자체가 총체적으로 너무나 큰 매몰비용의 늪에 빠져서 헤어나올 수 없는것 이라고 봐도

무방하지 않을까.

Log4j Lookup 호환성 취약점은 이런 현상의 실증적인 증거이지 않을까 싶다.