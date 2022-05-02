---
title:  "스프링 기초 시리즈 - DB"
header:
teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"
categories:
  - spring
tags:
  - Spring
---
  
  데이터를 저장하기 위해서 데이터베이스를 이용하여야만 한다.
  
자바 설정 클래스를 이용하여서 스프링 프레임워크에서 지원하는 DataSource 클래스를 이용하여 DB에 접근을 해보겠다.

이 포스팅에서는 h2 데이터베이스와 javax.sql을 사용하기에 Gradle에 의존성을 추기시켜주어야 한다.

build.gradle
```
plugins {
    id 'java'
}

group 'org.example'
version '1.0-SNAPSHOT'

repositories {
    mavenCentral()
}

dependencies {
    implementation 'org.springframework:spring-context:5.3.16'
    implementation 'org.springframework:spring-jdbc:5.3.16'
    implementation 'com.h2database:h2:2.1.210'
    implementation 'org.apache.tomcat:dbcp:6.0.53'
    testImplementation 'org.junit.jupiter:junit-jupiter-api:5.8.1'
    testRuntimeOnly 'org.junit.jupiter:junit-jupiter-engine:5.8.1'
}

test {
    useJUnitPlatform()
}
```

필요한 클래스들은 다음과 같다.

DemoApp 클래스

```java
package com.dan.practice.demos.myapp;

import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class DemoApp {
    public static void main(String[] args) {
        new AnnotationConfigApplicationContext(AppConfig.class);
    }
}

```

AppConfig 클래스

```java
package com.dan.practice.demos.myapp;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.jdbc.datasource.DriverManagerDataSource;

import javax.sql.DataSource;

@Configuration
@ComponentScan("com.dan.practice.demos.myapp")
public class AppConfig {

    @Bean
    public DataSource dataSource() {
        DriverManagerDataSource dataSource = new DriverManagerDataSource();
        dataSource.setDriverClassName("org.h2.Driver");
        dataSource.setUrl("jdbc:h2:mem:mydb");
        dataSource.setUsername("sa");
        dataSource.setPassword("");
        return dataSource;
    }
}
```

DAO 클래스

```java
package com.dan.practice.demos.myapp;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.support.JdbcDaoSupport;
import org.springframework.stereotype.Repository;

import javax.annotation.PostConstruct;
import javax.sql.DataSource;

@Repository
public class DAO extends JdbcDaoSupport {

    private DataSource dataSource;

    @Autowired
    public DAO(DataSource dataSource) {
        setDataSource(dataSource);
    }

    @PostConstruct
    public void doQuery() {
        String result = getJdbcTemplate().queryForObject("select 1 from dual", String.class);
        System.out.println("result = " + result);
    }

}
```


다른 방식으로는 Apache DBConnectionPool에서 지원하는 BasicDataSource를 이용한 방식이 있다.

로컬에 db를 저장하기 위해서 dataSource.setUrl을 변경하고 테스트를 위해서 DB_CLOSE_ON_EXIT=FALSE로 설정해주었다.

AppConfig 클래스만 변경하였다.

```java
package com.dan.practice.demos.myapp;

import org.apache.tomcat.dbcp.dbcp.BasicDataSource;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.datasource.DriverManagerDataSource;
import org.springframework.jdbc.datasource.SingleConnectionDataSource;

import javax.sql.DataSource;

@Configuration
@ComponentScan("com.dan.practice.demos.myapp")
public class AppConfig {

    @Bean
    public DataSource dataSource() {
        //Reusage of the same connection for the ease of test. Someone should close the connection via the close() method
//        SingleConnectionDataSource dataSource = new SingleConnectionDataSource();


        //This is Apache tomcat dbcp(database connetion pool)
        BasicDataSource dataSource = new BasicDataSource();
        dataSource.setDriverClassName("org.h2.Driver");
        //dataSource.setUrl("jdbc:h2:mem:mydb"); //h2 memory db
        dataSource.setUrl("jdbc:h2:file:~/ModernJava/mydb;AUTO_SERVER=TRUE;DB_CLOSE_ON_EXIT=FALSE");
        dataSource.setUsername("sa");
        dataSource.setPassword("");
        //dataSource.setMaxActive(5);  //Methods from BasicDataSource
        //dataSource.setMaxIdle(30000);
        return dataSource;
    }
}


```

