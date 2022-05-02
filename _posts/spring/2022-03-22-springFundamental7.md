---
title:  "스프링 기초 시리즈 - application.properties"
header:
teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"
categories:
  - spring 
tags:
  - Spring
---
  
스프링에서는 자바 클래스와 application.properties를 통해서 앱 환경을 변경할 수 있다.

환경 설정을 위한 자바 클래스를 다음과 같이 작성하였다.

```java
package com.dan.practice.demos.myapp;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.PropertySource;

@Configuration
@ComponentScan("com.dan.practice.demos.myapp")
@PropertySource("classpath:application.properties")
public class AppConfig {
}
```

이전 시간에 인터페이스와 구현으로 나눈 Service와 Repository 클래스를 다시 자바 클래스로 되돌리고

다음과 같이 작성하였다. 

MyService 클래스

```java
package com.dan.practice.demos.myapp;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Service;

@Service
public class MyService {
    @Value("${my.name}")
    private String name;

    private MyRepository repository;
    @Autowired
    public MyService(MyRepository repository) {
        this.repository = repository;
    }

    public void doBusinessLogic() {
        System.out.println("Doing business logic for " + name);
        repository.doQuery();
    }
}

```

MyRepository 클래스

```java
package com.dan.practice.demos.myapp;

import org.springframework.stereotype.Repository;

@Repository
public class MyRepository {
    public void doQuery() {
        System.out.println("Doing DB query!");
    }
}
```


그리고 resources 디렉토리 내에 application.properties 파일을 생성시키고

다음과 같은 밸류값을 적어주었다.

```
my.name=Dan
```

App 클래스 또한 다음과 같이 변경 시켜주었다.

```java
package com.dan.practice.demos.myapp;

import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class App {
    public static void main(String[] args) {
        ApplicationContext ctx = new AnnotationConfigApplicationContext(AppConfig.class);
        MyService service = ctx.getBean(MyService.class);
        service.doBusinessLogic();
    }
}
```

App 클래스를 실행할 경우 다음과 같이 콘솔에 뜬다.

```
> Task :App.main()
Doing business logic for Dan
Doing DB query!
```

이러한 방식으로 application.properties를 통해서

필요한 Value 값을 호출할 수 있다.

application.properties를 잘 사용할 수 있는 방법에서

@Profile을 이용하여서 개발용과 프로덕션용 환경설정을 다르게 설정할 수 있다.

기존의 AppConfig 클래스와 application.properties파일을 지우고

DevelopmentConfig 클래스, ProductionConfig 클래스, application-local.properties, application-prod.properties, MyServic 클래스, App 클래스를

생성해보았다.

프로젝트 디렉토리는 다음과 같은 형태이다.

% include figure image_path="/assets/images/sf7/sf7-1.png" alt="sf7-1"%}



DevelopmentConfig 클래스

```java
package com.dan.practice.demos.myapp.config;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Profile;
import org.springframework.context.annotation.PropertySource;

@Profile("local")
@Configuration
@ComponentScan("com.dan.practice.demos.myapp")
@PropertySource("classpath:application-local.properties")
public class DevelopmentConfig {
}

```


ProductionConfig 클래스

```java
package com.dan.practice.demos.myapp.config;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Profile;
import org.springframework.context.annotation.PropertySource;

@Profile("prod")
@Configuration
@ComponentScan("com.dan.practice.demos.myapp")
@PropertySource("classpath:application-prod.properties")
public class ProductionConfig {
}

```


application-local.properties
```
service.baseUrl=http://localhost:8082/MyService
```


application-prod.properties
```
service.baseUrl=https://prd.myservice.com:8082/MyApp
```

MyService 클래스

```java
package com.dan.practice.demos.myapp;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.EnvironmentAware;
import org.springframework.core.env.Environment;
import org.springframework.stereotype.Service;

import java.util.Arrays;

@Service
public class MyService implements EnvironmentAware {

    @Value("service.baseUrl")
    private String SERVICE_BASERURL;

    private MyRepository repository;

    private Environment environment;

    @Autowired
    public MyService(MyRepository repository) {
        this.repository = repository;
    }

    public void doBusinessLogic() {
        System.out.println("Doing business logic !");
        System.out.println("Active profiles: " + Arrays.toString(this.environment.getActiveProfiles()));
        System.out.println(String.format("Connecting to [%s]", SERVICE_BASERURL));
        repository.doQuery();
    }

    @Override
    public void setEnvironment(Environment environment) {
        this.environment = environment;
    }
}

```


App 클래스

```java
package com.dan.practice.demos.myapp;

import com.dan.practice.demos.myapp.config.DevelopmentConfig;
import com.dan.practice.demos.myapp.config.ProductionConfig;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class App {
    public static void main(String[] args) {
        System.setProperty("spring.profiles.active", "local");

        ApplicationContext ctx = new AnnotationConfigApplicationContext(DevelopmentConfig.class, ProductionConfig.class);
        MyService service = ctx.getBean(MyService.class);
        service.doBusinessLogic();
    }
}

```

App 클래스를 실행할 경우 이렇게 콘솔로 나온다.

```
> Task :App.main()
Doing business logic !
Active profiles: [local]
Connecting to [service.baseUrl]
Doing DB query!

```

App 클래스의 System.setProperty함수의 파라미터를 "local"에서 "prod"로 바꾸어 주었다.

```java
package com.dan.practice.demos.myapp;

import com.dan.practice.demos.myapp.config.DevelopmentConfig;
import com.dan.practice.demos.myapp.config.ProductionConfig;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class App {
    public static void main(String[] args) {
        System.setProperty("spring.profiles.active", "prod");

        ApplicationContext ctx = new AnnotationConfigApplicationContext(DevelopmentConfig.class, ProductionConfig.class);
        MyService service = ctx.getBean(MyService.class);
        service.doBusinessLogic();
    }
}
```

App클래스 실행에 의한 출력이 다음과 같이 바뀌었다.

```
> Task :App.main()
Doing business logic !
Active profiles: [prod]
Connecting to [service.baseUrl]
Doing DB query!
```

환경설정을 setProperty함수의 파라미터만 변경함으로서 Dev과 Prod간의 환경설정을

편하게 바꿀 수 있다.