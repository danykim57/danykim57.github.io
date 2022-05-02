---
title:  "스프링 기초 시리즈 - 웹 앱 데모"
header:
teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"
categories:
  - spring
tags:
  - Spring
---

Maven을 이용한 초간단 웹앱을 만들어 보았다.

그레이들을 빌드툴로 이용하는 곳이 많아 졌지만

레거시는 대부분 메이븐을 이용하기 때문에

메이븐으로 웹 앱 데모를 진행하였다.

디렉토리의 구조는 이런 식으로 설정하였다.

{% include figure image_path="/assets/images/sf11/sf11-1.png" alt="sf11-1"%}



메이븐에서 빌드 설정과 의존성 관리 파일인 pom.xml 파일을 다음과 같이 설정한다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>org.example</groupId>
    <artifactId>webappdemo</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>war</packaging>
    <name>webappdemo</name>
    <description>web app demo</description>

    <properties>
        <maven.compiler.source>11</maven.compiler.source>
        <maven.compiler.target>11</maven.compiler.target>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.2.20.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>4.0.1</version>
            <scope>provided</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-war-plugin</artifactId>
                <version>3.3.2</version>
            </plugin>
            <plugin>
                <groupId>org.eclipse.jetty</groupId>
                <artifactId>jetty-maven-plugin</artifactId>
                <version>9.4.46.v20220331</version>
            </plugin>
        </plugins>
    </build>
</project>
```

Spring 프레임워크 생태계에서 SpringBoot가 각광받는 이유가 

이런 플러그인과 의존성들을 SpringBoot에서는 미리 패키징되어서 제공되기 때문이다.

MavenCentral과 구글링으로 각각의 버젼에 호환되는 플러그인과 의존성을 추가하는 것은 상당히 번거로운 일이다.

자바 버젼에 따라서도 라이브러리나 프레임워크가 작동유무가 갈릴 수 있으므로

가능하면 SpringBoot를 애용하자.



war은 파일 형식으로서 JAR 파일, JSP(Jarver Server Page), Java 서블릿, 

Java 클래스, XML 파일, 태그 라이브러리, 정적 웹페이지등을 포함하고 있다.

war파일을 작동시키면 웹 앱을 만드는 필요한 설정을 WEB-INF라는 디렉토리에서 찾을려고 한다.

WEB-INF에 있는 jsp 디렉토레는 jsp 파일들을 담아두는 디렉토리로

jsp는 웹 브라우저에서 보여지는 뷰(View)를 담당해준다. 요청한 데이터 값을 보여주거나 

form등을 이용하여서 서버에 데이터 생성, 수정을 하도록 요청할 수 있다.

이 데모에서는 간단하게 endpoint를 통해서 파라미터와 쿼리로 요청을 해보겠다.

이전과 다르게 war로 패키징을 하면 main함수를 담은 클래스가 필요가 없다.

그러므로 간단하게 자바 웹앱 설정 클래스인 AppConfig와 요청을 핸들링해주는

컨트롤러 클래스로 MyController를 다음과 같이 작성하여주었다.

```java
package com.example.demo;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.ViewResolver;
import org.springframework.web.servlet.view.InternalResourceViewResolver;

@Configuration
public class AppConfig {

    @Bean
    public ViewResolver viewResolver() {
        return new InternalResourceViewResolver("/WEB-INF/jsp/", ".jsp");
    }
}
```

```java
package com.example.demo;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.*;
import org.springframework.web.servlet.ModelAndView;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.util.HashMap;
import java.util.Map;

@Controller
public class MyController {

    public MyController() {
        System.out.println("Creating controller!");
    }

//    @RequestMapping(path = "/greeting", method = RequestMethod.GET)
//    @GetMapping("/greeting")
//    public ModelAndView doGreeting(HttpServletRequest request, HttpServletResponse response) {
//        Map<String, String> values = new HashMap<>();
//        values.put("name", "Dan");
//        ModelAndView modelAndView = new ModelAndView("greet", values );
//        return modelAndView;
//    }

//    @GetMapping("/greeting")
//    public ModelAndView doGreeting(@RequestParam String mode) {
//        Map<String, String> values = new HashMap<>();
//        values.put("name", "Dan");
//        values.put("moodComment", "happy".equals(mode) ? "Feeling great" : "Meh");
//        ModelAndView modelAndView = new ModelAndView("greet", values );
//        return modelAndView;
//    }

    @GetMapping("/greeting/{aParam}")
    public ModelAndView doGreeting(@PathVariable("aParam") String param,  @RequestParam String mode) {
        Map<String, String> values = new HashMap<>();
        values.put("name", param);
        values.put("moodComment", "happy".equals(mode) ? "Feeling great" : "Meh");
        ModelAndView modelAndView = new ModelAndView("greet", values );
        return modelAndView;
    }
}

```

doGreeting 함수가 RequestMapping 어노테이션과 GetMapping 어노테이션으로 다른 버젼으로 되어있음을 볼 수 있다.

다음은 WEB-INF 디렉토리에 들어가 있는 설정 파일들과 jsp파일이다.

greet.jsp

```
<%@ page isELIgnored="false" %>

Hi ${name} - ${moodComment}

The time is <%= new java.util.Date() %>!
```

web.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<web-app>
    <servlet>
        <servlet-name>webappdemo</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <load-on-startup>1</load-on-startup>
    </servlet>

    <servlet-mapping>
        <servlet-name>webappdemo</servlet-name>
        <url-pattern>/webappdemo/*</url-pattern>
    </servlet-mapping>
</web-app>
```

webappdemo-servlet.xml
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="
        http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
    <context:component-scan base-package="com.example.demo"/>
</beans>
```
이제 인텔리제이에서 웹앱을 실행하고 디버깅 해보아야 하는데

이 때 과정이 상당히 복잡하기에

다음 포스팅에서 다루어 보도록 하겠다.