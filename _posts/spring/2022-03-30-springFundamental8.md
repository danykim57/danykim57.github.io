---
title:  "스프링 기초 시리즈 - Dynamic Proxy"
header:
teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"
categories:
  - spring
tags:
  - Spring
---
  
 런타임 때 실시간으로 조합을 하여서 인스턴스를 생성하는 것을 자바에서 Dynamic Proxy라고 한다.

Proxy는 특정 함수 호출을 Proxy의 자원들을 통해서 기능들을 추가시키는 wrapper나 front를 

뜻한다. (Proxy 패턴이라고도 하는데 Facade 패턴과 유사하다)

Dynamic Proxy는 하나의 메소드를 가지고 있는 하나의 클래스로 여러 다른 임의의 클래스와 

함수들에 대한 메소드 콜을 처리한다.

자바의 Dynamic Proxy는 프레임워크에서 효력을 강하게 발휘하는데 스프링에서 

Dynamic Proxy는 JdkDynamicAopProxy와 CGLIB을 통해서 구현된다.


다음은 JdkDynamicAopProxy의 예제이다.

App Class

```java
package com.dan.practice.demos.myapp;

import org.springframework.aop.framework.ProxyFactoryBean;

public class App {
    public static void main(String[] args) {
        IPerson p= new Person();

        ProxyFactoryBean proxyFactory = new ProxyFactoryBean();
        proxyFactory.setTarget(p);
        Object proxy = proxyFactory.getObject();

        IPerson bean = (IPerson) proxy;
        bean.greet();
    }
}
```

IPerson Interface

```java
package com.dan.practice.demos.myapp;

public interface IPerson {
    void greet();
}
```

Person Class

```java
package com.dan.practice.demos.myapp;

public class Person implements IPerson {
    @Override
    public void greet() {
        System.out.println("Hello there!");
    }
}
```

App Class의 14번 째줄 bean.greet();에  breakpoint를 걸고 디버깅모드를 실행시켜보았다.

{% include figure image_path="/assets/images/sf8/sf8-1.png" alt="sf8-1"%}

다음과 같이 proxy 객체가 Proxy로 생성되고 함수가 JdkDynamicAopProxy을 통해 생성된 것을

확인 할 수 있다.

IPerson이라고는 인터페이스를 통해서 Person 클래스를 구현한 이유는

JdkDynamicAopProxy를 사용하기 위해서는 인터페이스를 이용하여만 하기 때문이다.

<br>

다음은 CGLIB을 통한 DynamicProxy이다.

IPerson Interface는 삭제하고 Person은 더이상 IPerson을 구현하지 않는다.

App Class

```java
package com.dan.practice.demos.myapp;

import org.springframework.aop.framework.ProxyFactoryBean;

public class App {
    public static void main(String[] args) {
        Person p= new Person();

        ProxyFactoryBean proxyFactory = new ProxyFactoryBean();
        proxyFactory.setTarget(p);
        Object proxy = proxyFactory.getObject();

        Person bean = (Person) proxy;
        bean.greet();
    }
}
```

Person Class

```java
package com.dan.practice.demos.myapp;

public class Person {
    public void greet() {
        System.out.println("Hello there!");
    }
}
```

이전과 같이 App Class의 bean.greet()에 Breakpoint를 걸고 디버깅모드를 실행시켜주었다.

{% include figure image_path="/assets/images/sf8/sf8-2.png" alt="sf8-2"%}

이전과 다르게 CGLIB의 Enhancer 클래스를 통해서 proxy가 생성된 것을 확인할 수 있다.


Spring AOP는 JDK Dynamic Proxy와 CGLIB을 이용한다.

Spring AOP는 특정 함수 호출이 발생하면 IoC 컨테이너를 통하여서 Proxy Bean을 생성한다.

이 Bean을 추가적인 기능들을 주입하 위해서 가로채서(intercept) 감싸준다(wrapp).

런타임(On the fly)에 자바의 리플렉션의 Proxy클래스를 동적으로 이용을 하여서 

Dynamic Proxy라고 한다.

JDK Dynamic Proxy는 InvocationHandler라는 인터페이스의 invoke 함수를 통해서 발생하는데

제한 사항이 Interface에 대한 Proxy만을 생성해준다. 

CGLIB(Code Generator Library)는 클래스의 바이트코드를 조작하여서 Proxy 객체를

생성해주는 라이브러리이다.

CGLib은 Enhancer 클래스를 통해서 Proxy를 생성시킨다. 

바이트코드를 직접 조작하기 때문에 JDK Dynamic Proxy보다 성능이 좋지만

 1. cglib.proxy.Enhancer를 사용하여야 하고
 2. default 생성자를 만들어주어야 하며
 3. Target의 생성자를 두번 호출

위의 3가지 문제 때문에 Spring AOP에서는 JdK Dynamic Proxy를 권장하였지만

Spring Boot version 4 부터는 

 1. cglib.proxy.Enhancer가 Spring Core 패키지에 있음
 2. objenesis 라이브러리로 사용자가 default 생성자를 만들 필요가 없고
 3. 생성자가 2번 호출되는 문제가 해결됨

Spring Boot version 4 부터는 CGLIB이 기본값이 되었다.

출처: https://gmoon92.github.io/spring/aop/2019/04/20/jdk-dynamic-proxy-and-cglib.html



  