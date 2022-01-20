---
title:  "자바에서의 Abstract과 Interface"
header:
  teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"
categories:
  - Java, BackEnd
tags:
  - Java
---

  Abstract 클래스와 Interface는 비슷한 점이 많다.

둘다 객체생성(instantiate)을 하지 못하고

뒤에 만들어질 다른 클래스(SubClass)들을 위해서 생성시켜주는 타입들이다.

객체 생성이 안된다는 것은 new 키워드를 이용한 객체 생성이

불가능하다는 것을 의미한다. 그러므로 Abstract과 Interface는

원래 구현(implementation) 파트가 존재하지 않는다.

 Abstract 클래스는 다음과 같은 형태를 가지고 있다.

```
public abstract class MyAbstractClass {
    public abstract void meh();  //추상 메소드
    public void sayBoy() {       //구현이 들어간 메소드
        System.out.println("Boi! ");
    }
}
```

Abstract 클래스는 위와 같이 추상 메소드(Abstract Method)나 구현이 포함된 메소드가 있다.

추상 메소드는 Abstract 클래스를 상속받는 서브클래스가 항상 구현을 해주어야한다.

구현이 포함된 일반 메소드는 서브클래스에서 덮어씌울 수 있다.

특별하게 수정이 필요하지 않으면 그대로 이용할 수 있다.

 서브클래스의 예시는 다음과 같다.

```
public class MySubClass extends MyAbstractClass{
    public void meh() {
        System.out.println("Meh ");
    }

    public void sayBoy() {
        System.out.println("Edited boi");
    }
```

 Interface는 다음과 같은 형태를 가지고 있다.

```
interface MyInterface {
  public void wham(int num); // 인터페이스 메소드
  public void boom(String someWords); // 인터페이스 메소드
}
```

위의 Interface의 wham이나 boom처럼 메소드가 이름과 매개변수(parameter)만을 가지고 있는 형태를

메소드 시그니쳐(Method Signature)라고 부른다.


  클래스가 Abstract를 상속받을 때는 extends 키워드를 사용한다.

```
public class MySubclass extends MyAbstractClass {

    @Override
    public void meh() {
        System.out.println("Meh");
    }
}
```


  클래스가 Interface를 구현해야할 때 implements 키워드를 사용한다.

```
public class MySubclass2 implements MyInterface {
    @Override
    public void boom(String someWords) {
        System.out.println("Say Boom." + someWords);
    }

    @Override
    public void wham(int num) {
        for (int i = 0; i < num; i++)
            System.out.println("Wham ");
    }
}
```

인터페이스에 들어갈 수 있는 메소드의 종류는 총 4가지이다.

![](../assets/images/InterfaceStreamOracle.jpg)

link: https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html

인터페이스에 있는 메소드들은 Static Methods, Instance Methods, Abstract Methods, Default Methods로 나누어진다.

인스턴스 메소드(Instance Method)는 생성된 객체에서 쓰일 수 있는 메소드이다.

추상 메소드(Abstract Method)는 구현인 바디({})가 없는 메소드로 인터페이스를 implements 한 서브클래스가 구현부분을 완성시켜야 그 서브 클래스가 컴파일 될 수 있다.

자바8에서는 인터페이스에서 쓸 수 있는 스테틱 메소드(Static Method)와 디폴트 메소드(Default Method)가 추가되었다.

스테틱 메소드(Static Method)는 객체에 포함되는 것이 아닌 온전히 그 클래스에 완전히 종속된 메소드이다.

디폴트 메소드는(Default Method)는 인터페이스에서 구현된 메소드이다.

그래서 누군가 인터페이스에서 메소드를 구현할 수 있느냐 라고 묻는다면

자바 버젼에 따라 다르다라고 답을 할 수 있다.

자바8에서 제공하는 디폴트 메소드로 구현을 할 수 있지만 이전 버젼에서는

안된다라고 하면 더 자세한 답이다.


4가지 메소드가 포함된 인터페이스의 예제는 다음과 같다.

```
public interface MyInterfaceClass {
    int thisIsConstant = 1; //상수 변수

    public void wham(int num); // 인스턴스 메소드 (static 키워드가 없다)
    public abstract void boom(String someWords); // 추상 메소드 (Abstract가 없어도 서브클래스에서 구현해주어야한다.)
    static void belongHere() {  //스테틱 메소드 스테틱 메소드는 클래스 메소드라고도 불린다.
        System.out.println("This is belongHere method belong MyInterfaceClass");
    }

    default void yee() { //디폴트 메소드
        System.out.println("Say Yeee");
    }

}
```


디폴트 메소드를 이용할 때 주의해야할 점이 있다.

자바에서 같은 디폴트 메소드가 클래스와 인터페이스에 있을 때

클래스에 있는 디폴트 메소드가 우선권을 가져간다.




지금까지 대략적인 Abstract과 Interface의 개요를 보았다.

그렇다면 어떤 경우에 Abstract을 쓰고 Interface를 쓰는가?

각각의 타입을 쓰는 것은 개발자의 재량이지만

자바에서는 특정 제한을 두어서 아 두가지 타입을 구분하고 있다.

1. 추상 클래스(Abstract Class)는 다른 클래스를 상속받을 수 없다.

2. 인터페이스(Interface)는 상수가 아닌 변수를 필드에 가질 수 없다.
 
3. API를 만들고 추가로 메소드를 넣어주어야할 때는 인터페이스가 더 좋다.
 
4. 추상 클래스를 이용할 경우 추상 메소드가 아닌 메소드를 다른 서브클래스의 수정없이 간편하게 추가할 수 있다.









[^posts]: References 참조

Java in a Nutshell, 7th Edition By Ben Evans, David Flanagan

link: http://tutorials.jenkov.com/java/abstract-classes.html

link: https://docs.oracle.com/javase/tutorial/java/concepts/interface.html

link: https://www.baeldung.com/java-static-default-methods

link: https://www.geeksforgeeks.org/instance-methods-in-java/

Advanced Java by Ken Kousen link: https://learning.oreilly.com/videos/advanced-java-development/9781491960400/
