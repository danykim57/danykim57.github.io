---
title:  "싱글턴 패턴"
header:
teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"
categories:
  - designPattern
tags:
  - designPattern
---

  싱글턴 패턴은 생성에 대한 디자인 패턴으로서 클래스가 글로벌 접근이 가능한 하나의 객체만을 가지고 있는 것을 보장해줍니다.
  
싱글턴 패턴의 이용:

- 싱글턴 패턴은 DB와 같은 공유자원에 접근하는 클래스에서 많이 사용됩니다.

싱글턴 패턴의 문제점:

- 일반적인 클래스의 생성자(Constructor)는 새로운 객체를 리턴할 수 있더록 합니다. 
- 글로벌 접근은 어떠한 코드든 객체에 접근을 가능하게 하여 해당 객체의 데이터가 예상하지 못한 값을 가질 수 있습니다.
- 묘듈간의 결합(Coupling)이 강해집니다. 종속성이 늘어나면서 한 묘듈의 변경에 대해서 다른 묘듈 또한 수정해야하게 됩니다.

싱글턴 패턴에 대한 해결책:

- 생성자의 접근을 private으로 만들어서 다른 객체가 new 키워드로 생성자에 접근하지 못하도록 막습니다.
- public 메소드를 static으로 만들어서 생성자처럼 행동하게 만듭니다. 실제로 이 메소드는 private 생성자를 호출하여서 객체를 하나 클래스의 static 필드에 놔두고 재사용되도록 합니다.
- 의존성 주입(Dependency Injection)으로 결합을 약화(Decoupling) 시킬 수 있습니다.

의존성 주입의 장단점:

장점:
- 코드가 테스트하기 더욱 용이해집니다
- 추상화 레이어를 통해서 모듈간의 의존성 방향이 명확해집니다.
- 이후의 추가적인 기능 추가나 마이그레이션에서 강점을 보입니다.

단점:
- 전체적인 클래스가 늘어나서 오히려 모듈들을 이해하기 힘들어 질 수 있다.
- 클래스 숫자의 증가는 클래스 로딩시간의 증가로 인해 런타임 시의 성능저하가 일어날 수 있다.

싱글턴의 장단점

장점:

- 클래스가 하나의 객체만 가지고 있는 것을 보장합니다
- 글로벌 액세스를 편리하게 그 객체에 접근을 할 수 있습니다.
- 객체를 한번만 생성하여서 객체 생성 비용이 줄어듭니다.

단점:

- SOLID원칙의 Single Responsibility Principle을 위배합니다. 이 객체는 두가지 이상의 일을 하게 됩니다.
- 테스트가 어렵습니다. 여러 객체가 이 객체에 접근이 가능하기 때문에 독립적이면서 순서에 영향을 받지 않아야하는 유닛테스트에서 쓰이기 힘듭니다.
- 멀티스레드 환경에서 각각의 스레드들이 싱글턴 객체를 생성하지 않도록 방지하여야합니다.

자바 싱글턴 예제 코드

```java
class Singleton {
    private static class singletonConstructor { //private한 Constructor
        private static final Singleton instance = new Singleton();
    }
    public static synchronized Singleton getInstance() { //생성자 접근을 가진 public static 메소드
        //cpu 캐시내의 복사를 막고 주메모리에 객체 상태를 관리하기 위해 synchronized를 붇임.
        return singletonConstructor.instance;
    }
}
```