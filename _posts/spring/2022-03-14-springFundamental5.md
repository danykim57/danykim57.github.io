---
title:  "스프링 기초 시리즈 - 의존성 주입 2"
header:
teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"
categories:
  - spring
tags:
  - Spring
---
  
ClassPath를 이용한 의존성 주입을 다음과 같이 진행해 보았었다.

```java
package com.dan.practice.demos.myapp;

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

이렇게 Constructor로 의존성 주입을 하지 않고

바로 객체를 생성하여서 쓰면 이런 모습이 된다.

```java
package com.dan.practice.demos.myapp;

public class MyService {

    private MyRepository repo;

    public MyService(MyRepository repo) {
        this.repo = new MyRepository();
    }

    public void doBusinessLogic() {
        System.out.println("doing my business");
        repo.doQuery();
    }
}
```

프로덕션에서 쓰이는 DB와 로컬에서의 DB를 바꾸어 사용하는 경우가 많은

일반적인 웹 앱 개발 상황에서 이런 코드는 적합하지 않다.



Constructor를 쓰지 않고 Setter로 의존성을 주입해보겠다.

MyService 클래스를 변경하였다.

```java
package com.dan.practice.demos.myapp;

public class MyService {

    private MyRepository repo;

    public MyService(MyRepository repo) {
        this.repo = repo;
    }

    public void doBusinessLogic() {
        System.out.println("doing my business");
        repo.doQuery();
    }

    public MyRepository getRepo() {
        return repo;
    }

    public void setRepo(MyRepository repo) {
        this.repo = repo;
    }
}
```

resources 아래의 applicationContext.xml을 다음과 같이 변경하였다.

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

App 클래스를 변경후에 실행시켜보겠다.

```java
package com.dan.practice.demos.myapp;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class App {
    public static void main(String[] args) {
        ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");

        MyService service = ctx.getBean(MyService.class);
        service.doBusinessLogic();

        MyRepository repository = ctx.getBean(MyRepository.class);
        repository.doQuery();
    }
}
```

성공적으로 실행이 된다.

```
BUILD SUCCESSFUL in 2s
3 actionable tasks: 2 executed, 1 up-to-date
8:58:46 PM: Execution finished ':App.main()'.
```





