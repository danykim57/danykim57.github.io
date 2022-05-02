---
title:  "스프링 기초 시리즈 - 인터페이스와 구현 분리"
header:
teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"
categories:
  - spring
tags:
  - Spring
---
  SOLID 원칙의 인터페이스 분리 원칙을 스프링에서 이러한 형태로 적용이 가능하다.
  
이전에 만들었던 MyService 클래스를 인터페이스로 만들고 구현체를 따로 작성해보겠다.

```java
package com.dan.practice.demos.myapp.business;

public interface MyService {
    void doBusinessLogic();
}

```
MyService 구현체: MyServiceImpl

```java
package com.dan.practice.demos.myapp.business;

import com.dan.practice.demos.myapp.data.MyRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.stereotype.Service;

@Service
public class MyServiceImpl implements MyService {

    private MyRepository repo;

    @Autowired
    public MyServiceImpl(@Qualifier("myRepositoryImpl")MyRepository repo) {
        this.repo = repo;
    }

    @Override
    public void doBusinessLogic() {
        System.out.println("doing my business");
        repo.doQuery();
    }
}
```
MyService 구현체: AnotherServiceImpl

```java
package com.dan.practice.demos.myapp.business;

import com.dan.practice.demos.myapp.data.AnotherRepositoryImpl;
import com.dan.practice.demos.myapp.data.MyRepositoryImpl;
import org.springframework.beans.factory.annotation.Autowired;

public class AnotherServiceImpl implements MyService {

    private AnotherRepositoryImpl repo;
    
    @Autowired
    public AnotherServiceImpl(AnotherRepositoryImpl repo) {
        this.repo = repo;
    }

    @Override
    public void doBusinessLogic() {
        System.out.println("doing another business");
        repo.doQuery();
    }
}

```

MyRepository 또한 인터페이스로 변형시켜주고 따로 구현체를 작성해보았다.

```java
package com.dan.practice.demos.myapp.data;

public interface MyRepository {
    void doQuery();
}

```

MyRepository 구현체: MyRepositoryImpl

```java
package com.dan.practice.demos.myapp.data;

import org.springframework.stereotype.Repository;

@Repository
public class MyRepositoryImpl implements MyRepository {
    @Override
    public void doQuery() {
        System.out.println("Doing DB thingy");
    }
}

```

MyRepository 구현체: AnotherRepositoryImpl

```java
package com.dan.practice.demos.myapp.data;

import org.springframework.stereotype.Repository;

@Repository
public class AnotherRepositoryImpl implements MyRepository {
    @Override
    public void doQuery() {
        System.out.println("Doing another DB thingy");
    }
}

```

App 클래스도 다음과 같이 변경시켜주었다.

```java
package com.dan.practice.demos.myapp;

import com.dan.practice.demos.myapp.business.AnotherServiceImpl;
import com.dan.practice.demos.myapp.business.MyService;
import com.dan.practice.demos.myapp.business.MyServiceImpl;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class App {
    public static void main(String[] args) {
        ApplicationContext ctx = new AnnotationConfigApplicationContext(AppConfig.class);

        MyService service1 = ctx.getBean(MyServiceImpl.class);
        MyService service2 = ctx.getBean(AnotherServiceImpl.class);
    }
}
```

앱설정을 자바 클래스로 하기 위해서 AppConfig라는 클래스를 생성시키고

앱설정 클래스를 알리는 @Configuration과 스프링 컨텍스트가 필요로하는 정보인

컴보넌트 스캔을 해야할 레퍼런스를 @ComponentScan("com.dan.practice.demos.myapp")로

설정해주었다.

```java
package com.dan.practice.demos.myapp;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Scope;

@Configuration
@ComponentScan("com.dan.practice.demos.myapp")
public class AppConfig {
}

```

인터페이스와 구현체를 다음과 같이 분리할 경우에 다음과 같은 이점이 있다.

- 객체의 규모가 커질 수록 필요에 따라서 인터페이스로 잘게 나누면서 확장성을 확보할 수 있다.
- 객체가 불필요한 상태나 기능을 가지는 것을 예방할 수 있다.

