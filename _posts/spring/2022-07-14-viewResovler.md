---
title:  스프링 부트와 뷰 리졸버(ViewResolver)
header:
teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"
categories:
  - spring
tags:
  - Spring
---

 스프링 부트의 강력한 기능은 AutoConfiguation이다.
 
요청 클라이언트에게 반환되는 뷰(View)의 처리는 뷰 리졸버에 의해서 일어나는데

스프링 부트는 WebMvcAutoConfiguration를 이용하여서 처리한다.

이 설정은 InternalResourceViewResolver과 BeanNameViewResolver 빈설정을 변경시킨다.

타임리프의 의존성을 추가시키면 수동으로 처리해야할 설정을 자동으로 처리해준다.

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
    <version>${spring-boot-starter-thymeleaf.version}</version>
</dependency>
```

타임리프를 이용하면서 뷰리졸버를 커스텀하는데는 ThymeleafViewResolver 클래스만 오버라이딩해주어도 충분하다.

다른 탬플릿 엔진(mustache, groovy-templates)을 이용하여도 비슷한 효과를 볼 수 있다.

DispatcherServlet은 등록되어있는 어플리케이션 컨텍스트에서 맞는 결과를 찾을 때 까지 등록된 뷰리졸버 순으로

찾아보기에 뷰 리졸버의 등록 순서가 중요하다.

