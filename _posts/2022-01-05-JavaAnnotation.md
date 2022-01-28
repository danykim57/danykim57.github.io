---
title:  "자바 어노테이션"
header:
  teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"
categories: 
  - BackEnd
tags:
  - Java
---
출처: Java in a Nutshell, 7th Edition By Ben Evans, David Flanagan

  자바에는 어노테이션이라는 인터페이스 타입이 있다. 어노테이션이 하는 일은 실제로는 아무 것도 없다.
  
그저 메소드나 클래스 위에 붙어서 추가적인 설명을 해주는 역할이다.

  예를 들자면 @Override라는 어노테이션이 메소드 위에 붙어 있으면 그저
  
해당 메소드가 그 메소드의 슈퍼클래스를 덮어 쒸운다(Override)는 추가적인 설명을 전달할 뿐이다.

  어노테이션은 원래 이론적으로는 프로그램의 의미론(semantic)적인 부분을 변경하지 않고 실행 이전의 단계나
  
컴파일러에게 추가적인 정보를 주는 역할을 하도록 만들어졌었다. 

  재미있는 점은 실제로 어노테이션이 별다른 기능이 없어야 함에도 불구하고 추가적으로
  
 런타임(프로그램이 구동하는 시간) 동안에 어노테이션이 제대로 작동하도록 기능을 추가해야되는 지경에 이르렀다.
 
  예를 들어서 스프링 프레임워크에서 대표적인 어노테이션인 @Autowired는 클래스의 의존성을 주입해주는

어노테이션이다. @Autowired는 적절한 컨테이너(객체를 생성 관리해주는 스프링 프레임 워크의 주요 기능) 외에는
 
작동을 원래 하면 안되지만 기능의 특성상 작동이 되게 되어 있다.

  이러한 실용적인 문제들로 자바는 어노테이션의 커스터마이징을 지원하는데 @Interface을 이용해서

커스터마이징이 가능하다.

  Java8에서는 타입 어노테이션(Type Annotation)이 추가되었다.
  
 Java8부터 추가된 새로운 타입인 ElementType의 Type_Parameter와 Type_Use가 있다.
 
 @NotNull과 같이 NullPointerException으로 예외처리 대신에 간단하게 타입 체크를 해줄 수 있다.


 예제)
```
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.Type)
public @interface JsonSerializable {
}

```

출처: 

https://www.baeldung.com/java-custom-annotation


   
  
[^posts]: Footnote test.
