---
title:  "코틀린 개발자가 자바의 무엇을 그리워하는가."
header:
teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"
categories:
  - etc
tags:
  - 잡설
---

**번역 포스트입니다.**

**불변 참조 (Immutable references)**

자바는 시작 때부터 불변참조를 가지고 있었습니다..

불변참조를 다음에서 사용할 수 있습니다.

- 클래스내의 값(attributes)
- 메소드내의 파라미터
- 지역 변수들

```java
class Foo {
    final Object bar = new Object(); //bar를 재정의 할 수 없다.
    
    void baz(final Object que) { //que를 재정의 할 수 없다.
        final var corge = new Object();  //corge를 재정의 할 수 없다.
    }
}
```

불변참조는 고치기 힘든 버그들을 어느 정도 예방할 수 있습니다. 그러나, 흥미롭게도 final 키워드는 

생각보다 많은 자바 프로젝트들에서 쓰이질 않습니다.

예를 들어서, 스프링의 GenericBean은 불변값을 사용하지만 

불변 메소드나 불변 지역변수를 사용하지 않습니다.

slf4j의 DefaultLoggingEventBuilder는 위의 3가지(불변값, 불면 메소드, 불변 지역변수)를 다 사용하지 않습니다.



자바에서는 불변참조가 필수사항이 아닙니다. 기본값으로 참조는 가변적입니다(mutable). 그래서 대부분의 자바 코드들은

불변참조의 장점을 활용하지 못합니다.

코틀린에서는 참조의 가변성은 선택사항이 아닙니다. 모든 값과 변수는 val이나 var로 선언되어야한다.

코틀린에서 var과 val의 차이점

var:

  - 다른 데이터 타입으로 값의 재정의가 불가능하다.
  - 같은 데이터 타입이면 재정의가 가능하다.

val:
  - 선언시에 값이 할당되면 변경할 수 없다.
  - 변경을 할 경우에 컴파일러 에러가 발생한다.

자바에서의 var은 컴파일러가 추론적으로 데이터 타입을 할당할 수 있는 용도로만 쓰입니다.


**널값 안정성(Null Safety)**

자바에서는 변수의 값이 null인지 알 수가 없습니다. 명확하게 말하자면 Java8에서

Optional 타입을 소개하지만 Optional은 값이 null일 수 있다라는 것을 암시합니다. 다른 타입을 리턴할 경우에는

null이 아닐 것이다를 명시합니다.

그러나, Optional을 만든 개발자는 리턴값에서만 작동하도록 설계를 하였습니다. 자바 언어 문법에서 

메소드 파라미터나 메소드 리턴값이 null일 것이라는 것을 명시할 수 없습니다.


| Project   | Package                   | Non-null annotation | Nullable annotation |
|-----------|---------------------------|---------------------|---------------------|
| JSR 305   | javax.annotation          | @Nonnull            | @Nullable           |
| Spring    | org.springframework.lang  | @Nonnull            | @Nullable           |
| JetBrains | org.jetbrains.annotations | @Nonnull            | @Nullable           |


몇몇 라이브러리들은 특정 IDE환경에만 작동됩니다. 더해서, 이 라이브러리들 간의 호환성은 기대할 수 가 없습니다.
어떤 라이브러리를 사용할 지는 스택오버플로우의 질문자가 정하는 경우가 많습니다.

예시: [NullpointerException: stackoverflow](https://stackoverflow.com/questions/4963300/which-notnull-java-annotation-should-i-use)


코틀린에서는 모든 타입이 널값이 가능한지 아닌지를 결정해주어야 한다.

```kotlin
val nonNullable: String = computeNonNullableString()
val nullable: String? = computeNullableString()
```


**확장 함수(Extension functions)**

자바에서는 서브클래스가 하나의 클래스만을 확장(extend)할 수 있습니다.

```java
class Foo {}
class Bar extends Bar {}
```

서브클래싱은 두가지의 문제를 가지고 있습니다.

첫번째 문제는 몇몇 클래스는 final 키워드를 이용하여서 확장을 하지 못하도록 합니다.

예시) String 클래스

두번째 문제는 우리가 제어할 수 없는 메소드가 타입을 리턴할 경우에 클라이언트 개발자의 의도와는 다르게 이 타입으로만 작업을 하여야합니다.

이 문제로 자바 개발자들은 활용 클래스(utility classes)라는 개념을 만들었습니다.

활용 클래스는 static한 메소드들과 private한 Constructor로 이루어져있습니다. 고로, 활용 클래스는 인스턴스화 시킬수 없습니다.

활용 클래스가 이러한 형태를 가지는 이유는 자바가 메소드를 클래스 외부에서 사용되지 못하도록 막기 때문입니다. (자바에서는 일급 객체(First-Citizen Object)가 없습니다.)


이 방식으로 메소드가 필요한 타입으로 존재하지 않을 경우 활용 클래스는 그 타입을 파라미터로 받아서 필요한 처리를 할 수 있도록 도와줍니다.

```java
class StringUtils {                                          

    private StringUtils() {}                                 

    static String capitalize(String string) {                
        return string.substring(0, 1).toUpperCase()
            + string.substring(1);                           
    }
}

String string = randomString();                              
String capitalizedString = StringUtils.capitalize(string); 
```


이전에는 개발자들이 활용 클래스를 프로젝트내에서 만들었지만 지금은 Apache Commons Lang이나 Guava를 통해서 처리합니다.

코틀린은 같은 문제를 확장함수(extension function)들로 해결하였습니다.

>
> > 코틀린은 데코레이터와 같은 디자인 패턴이나 클래스 상속없이도 클래스나 인터페이스의 새로운 기능을 확장할 수 있도록 지원합니다. 
> 특별한 확장(extensions)이라는 특별한 선언을 통해서 이것이 가능합니다.
> 예를 들어서 수정권한이 없는 서드파티 라이브러리의 클래스나 인터페이스에 새로운 함수를 추가할 수 있습니다. 이 함수들은 원래 클래스에 속해있는 것 처럼 불러 올 수 있는데 이것을 확장 함수(extension function)라고 합니다.
> 확장함수를 선언하기 위해서 receiver 타입에 함수의 이름을 접두어로 붙입니다.
> 

```kotlin
fun String.capitalize2(): String {                            
    return substring(0, 1).uppercase() + substring(1);
}

val string = randomString()
val capitalizedString = string.capitalize2()  
```

이 함수들은 정적(static)이라서 원래 있는 타입에 새로운 행동(behavior)을 부여한 것처럼 행동하지만 사실은 그렇지 않습니다.

생성된 바이트 코드는 자바의 정적 메소드와 흡사합니다. 그러나, 자바와 다르게 문법은 더 명확하고 함수 체이닝을 지원합니다. 

Reified generics

제너릭은 자바5에 도입되었습니다. 그러나, 하위 호환성(backward compatibility) 문제로 자바5 바이트코드는 자바5 이전의 바이트코드들과 호환이 되어야 했고

제너릭 타입이 생성된 바이트코드로 되어있지 않은 이유입니다: 이것을 type erasure라고 부릅니다.

반대로는 reified generics이 있습니다. 제너릭 타입이 바이트코드로 쓰여지면 reified generics가 됩니다. 

제너릭 타입이 컴파일 때만 고려되는 것은 다음과 같은 문제를 야기합니다. 다음의 두 메소드 시그니쳐는 같은 바이트코드를 생성하여서 코드가 유효하지 않습니다.

```java
class Bag {
    int compute(List<Foo> persons) {}
    int compute(List<Bar> persons) {}
}
```

또다른 문제는 컨테이너 값들 중에서 특정 타입의 값을 뽑아내느냐 입니다.

```java
public interface BeanFactory {
    <T> T getBean(Class<T> requiredType);
}
```

개발자들은 Class<T>라는 파라미터를 넣어서 메소드 바디에서 타입을 알 수 있더록 하였습니다.

만약에 자바가 reified generics를 지원하였다면 코드는 다음과 같아졌을 것 입니다.

```java
public interface BeanFactory {
    <T> T getBean();
}
```

코들린에 reified generics가 있으면 코드는 다음과 같아집니다.

```kotlin
interface BeanFactory {
    fun <T> getBean(): T
}
```

함수 호출은 다음과 같을 것 이다.

```kotlin
val factory = getBeanFactory()
val anyBean = getBean<Any>()
```

코틀린은 여전히 JVM의 요구사항(JVM Specification)과 자바 컴파일러로 생성된 바이트코드와 호환이 되어야합니다.

이것은 inlining으로 해결할 수 있습니다.

inlining: 컴파일러가 인라인 메소드를 함수 바디와 교체하도록 하는 방법.

```kotlin
inline fun <reified T : Any> BeanFactory.getBean(): T = getBean(T::class.java)
```


결론

 코틀린의 기능들인 immutable references, null safety, extension functions, reified generics를 이용하면 

Kotlin Routes나 Beans DSL과 같은 DSL(Domain Specific Language)를 만들 수 있습니다.

```kotlin
beans {
  bean {
    router {
      GET("/hello") { ServerResponse.ok().body("Hello world!") }
    }
  }
}
```

JVM의 커스터마이징을 위해서만 자바를 사용하고 프로젝트 개발은 앞으로도 코틀린으로만 하게 될 것 같습니다.


참조: 

[What I miss in Java, the perspectives of a Kotlin developer](https://blog.frankel.ch/miss-in-java-kotlin-developer/)
[Kotlin-val-var의-차이점](https://velog.io/@jojo_devstory/Kotlin-val-var%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90)


