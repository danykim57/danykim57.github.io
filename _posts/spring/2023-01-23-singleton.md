---
title: 싱글턴
header:
teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"
categories:
  - spring
tags:
  - Spring
---

 디자인 패턴에서의 싱글턴(Singleton)과 스프링에서의 싱글턴에는 차이가 있다.

공통점: 객체를 하나만 생성해서 재활용한다.

차이점: 디자인 패턴에서는 하나의 클래스 타입에 하나의 객체를 스프링에서는 하나의 이름에 하나의 객체를 생성한다.

예제:

 - 싱글턴 디자인 패턴
```java
public class HelloService{
  public static HelloService getInstance() {
    if (instanceNotCreated()) {
      createHelloServiceInstance();
    }
    return helloService;
  }
}
```

 - 스프링 싱글턴
```java
@Configuration
public class BeanConfig {
  @Bean
  public HelloService helloService1() {
    return new HelloService();
  }
  
  @Bean
  public HelloSerivce helloService2() {
    return new HelloService();
  }
}
```
