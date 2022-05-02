---
title:  "스프링 기초 시리즈 - 의존성 주입"
header:
teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"
categories:
  - spring
tags:
  - Spring
---
   
 의존성 주입이란 한 객체가 다른 종속적인 객체를 받게 하는 행위를 말한다.

자바에서는 Class field에 다른 객체를 받는 변수를 선언하거 constructor를 변경하면서  이루어 진다.

스프링 프레임워크를 사용할 경우에는 환경설정도 바꾸어주어야 한다.

예제를 보자.

데이터베이스에 접근을 하는 MyRepository라는 클래스를 만들어 주었다.

```java
package com.dan.practice.demos.myapp;

public class MyRepository {
    public void doQuery() {
        System.out.println("Doing DB thingy");
    }
}

```

MyService를 MyRepository 객체를 받고 사용할 수 있도록 변경하였다.

```java
package com.dan.practice.demos.myapp;

import org.springframework.stereotype.Component;

@Component
public class MyService {

    private MyRepository repo;

    public MyService(MyRepository repo) {
        this.repo = repo;
    }

    public void doBusinessLogic() {
        System.out.println("doing my business");
        repo.doQuery();
    }
}
```

resources 디렉토리의 application-property.xml 파일을 변경해주었다.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="service" class="com.dan.practice.demos.myapp.MyService">
        <constructor-arg ref="repository"/>
    </bean>
    <bean id="repository" class="com.dan.practice.demos.myapp.MyRepository"/>
</beans>
```

이제 App 클래스를 실행시키면

```
BUILD SUCCESSFUL in 2s
3 actionable tasks: 3 executed
9:20:29 PM: Execution finished ':App.main()'.
```

성공적으로 실행이 된다.

디버깅 모드로 들어가면 의존성 주입을 확인할 수 있다.

{% include figure image_path="/assets/images/sf3/sf3-1.png" alt="sf3-1"%}

{% include figure image_path="/assets/images/sf3/sf3-2.png" alt="sf3-2"%}

{% include figure image_path="/assets/images/sf3/sf3-3.png" alt="sf3-3"%}

단축키 Option + F8을 이용하여 특정 메소드를 실행시켜볼 수 있다. (맥북에서는 Fn + Option + F8)

{% include figure image_path="/assets/images/sf3/sf3-4.png" alt="sf3-4"%}

콘솔창을 확인하면 다음과 같이 잘 프린트가 된다.

```
> Task :App.main()
doing my business
Doing DB thingy
```

의존성 주입을 하게되면 개발환경에 따라서 소스코드를 변경을 안하여도 된다.

또한 환경설정만 바꾸면 되기에 설정 변경도 용이하다.

다음에는 제어 역젼(Inversion of Control)을 다루어 보도록 하겠다.

