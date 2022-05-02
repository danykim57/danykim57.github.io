---
title:  "스프링 기초 시리즈 - 환경설정"
header:
teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"
categories:
  - spring
tags:
  - Spring
---
  
 스프링에서는 앱의 환경설정을 하는 방법이 3가지가 있다.
 
 - xml 파일을 이용한 환경설정
 - xml과 ClassPath를 이용한 환경설정
 - Annotation을 이용한 환경설정

ApplicationContext가 이 세가지를 지원한다는 것은 다음과 같이 알 수 있다.

작성하였던 App 클래스의 ApplicationContext를 클릭하고 Ctrl + h 를 눌러보면

연관된 계층(hierarchy)을 보여준다.

{% include figure image_path="/assets/images/sf2-1.png" alt="sf2"%}



1. xml 파일을 통한 환경설정 

다음과 같이 FileSystemXmlApplicationContext 객체를 "applicationContext.xml" 매개변수와 함께 생성하였다.

leSystemXmlApplicationContext를 누르고 cmd + p를 누르면 사용할 수 있는 매개변수의 타입들을 보여준다.

```java
package com.dan.practice.demos.myapp;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.FileSystemXmlApplicationContext;

public class App {
    public static void main(String[] args) {
        ApplicationContext ctx = new FileSystemXmlApplicationContext("applicationContext.xml");
    }
}


```

프로젝트 디렉토리 하위에 applicationContext.xml 파일을 만들었다.


{% include figure image_path="/assets/images/sf2-2.png" alt="sf2"%}

xml 파일은 다음과 같은 내용이 들어있다.

xml 버젼 1.0을 쓰고 인코딩은 표준 규격인 utf-8을 썼다.

xml의 namespace를 스프링 프레임워크의 빈을 쓰고 스키마 인스턴스는 w3에서 제공하는 표준을 쓰도록 한다.

마무리로 스프링 빈을 위한 스키마의 위치를 맵핑시킨다.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
</beans>
```

이제 App클래스로 돌아가서 초록생 실행 버튼을 누르거나 Ctrl + shift + R을 눌러보면

App클래스가 실행되면서 다음과 같은 문구가 떠야한다.

```
BUILD SUCCESSFUL in 698ms
3 actionable tasks: 1 executed, 2 up-to-date
10:01:03 PM: Execution finished ':App.main()'.
```

실제 일을 해주어야하는 빈을 생성하지 않았기 때문에 실행되고 바로 꺼지는 것을 확인 할 수 있다.

2. xml과 ClassPath를 이용한 환경설정

ClassPath를 통해 환경설정을 하기 위해서 다음과 같이 App 클래스를 변경하였다.

```java
package com.dan.practice.demos.myapp;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class App {
    public static void main(String[] args) {
        ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
    }
}
```

ClassPathXmlApplicationContext는 main 모듈 아래에 resources에서 환경설정 파일을 찾기에

프로젝트 디렉토리 안에 환경설정 파일이 있어도 못찾는다.

이번에는 xml파일을 resources라는 파일밑에 생성 주어야한다.

{% include figure image_path="/assets/images/sf2-3.png" alt="sf3"%}

xml 내용은 방금전과 똑같이 넣어주었다.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
</beans>
```

실행을 해 본 결과 이전 처럼 잘되었다.

```
BUILD SUCCESSFUL in 904ms
3 actionable tasks: 3 executed
10:04:04 PM: Execution finished ':App.main()'.
```

이제 환경설정에 빈을 넣어보도록 하자.

xml 파일을 다음과 같이 변경하여 보았다.

빈의 class 경로는 따로 자신의 프로젝트에 맞게 바꾸어 주어야한다.

이 부분 -> class="com.dan.practice.demos.myapp.MyService"

```xml
<?xml version="1.0" encoding="utf-8" ?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id ="service" class="com.dan.practice.demos.myapp.MyService"/>
</beans>
```

그리고 App과 같은 패키지에 MyService 클래스를 생성시켜서 다음과 같이 짜주었다

```java
package com.dan.practice.demos.myapp;

public class MyService {
    public void doSomething() {
        System.out.println("Bob, do something.");
    }
}
```

App 클래스를 다음과 같이 변경하였다.

```java
package com.dan.practice.demos.myapp;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class App {
    public static void main(String[] args) {
        ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
        MyService bean  = ctx.getBean(MyService.class);
        bean.doSomething();
    }
}
```


App 클래스를 실행시키면 다음과 같이 일어나야 한다.

```
10:10:02 PM: Executing ':App.main()'...

> Task :compileJava
> Task :processResources
> Task :classes

> Task :App.main()
Bob, do something.

BUILD SUCCESSFUL in 934ms
3 actionable tasks: 3 executed
10:10:03 PM: Execution finished ':App.main()'.
```


여기서 스프링이 싱글턴 패턴(Singleton)을 쓰는지 확인해보자.

싱글턴은 하나의 클래스에 하나의 객체만 사용가능하게 만드는 디자인 패턴이다.

다음과 같이 App 클래스를 변경해보자

```java
package com.dan.practice.demos.myapp;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class App {
    public static void main(String[] args) {
        ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
        MyService bean1  = ctx.getBean(MyService.class);
        MyService bean2  = ctx.getBean(MyService.class);
        MyService bean3  = ctx.getBean(MyService.class);
    }
}
```

Ctrl + shift + D 로 디버깅 모드로 들어가서 Step Over(F8)로 확인을 해보면 다음과 같이 객체가 같다는 것을 확인 할 수 있다.

{% include figure image_path="/assets/images/sf2-4.png" alt="sf4"%}

스프링 빈의 기본 스코프 값이 싱글턴인 것을 다른 것으로 바꿀 수 있다.

이번에는 객체를 복사하여서 쓰는 프로토타입 패턴을 써볼 것 이다.

resources 아래의 xml파일의 빈에 스코프라는 옵션을 넣어 주자.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id ="service" class="com.dan.practice.demos.myapp.MyService" scope="prototype"/>
</beans>
```

App 클래스에서 디버깅을 돌리면 이번에는 다른 객체들이 쓰였다는 것을 확인할 수 있다.

{% include figure image_path="/assets/images/sf2-5.png" alt="sf5"%}

스코프를 변경하므로서 빈을 어떻게 사용할 것인지 바꿀 수 있다.

싱클턴을 쓰면 객체가 ApplicationContext가 사라질 때 까지 유지되고

Prototype은 사용자가 원하는 시기에 사라지게 만들 수 있으며

http request 스코프를 이용하면 객체가 http 요청 동안에만 존재하게 만들 수 있다.


3. 어노테이션을 이용한 환경설정

빈을 추기할 때마다 xml파일을 건드리는 것은 너무나 번거로운 일이다.

자바의 어노테이션을 이용하여서 빈 환경설정을 해보도록 하겠다.

일단 한번은 환경설정 파일을 손봐야 한다.

resources 내의 xml파일을 다음과 같이 변경하였다.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context  http://www.springframework.org/schema/context/spring-context.xsd">
    <context:component-scan base-package="com.dan.practice.demos.myapp"/>
</beans>
```

이제 컴포넌트 스캔을 사용할 수 있다.

MyService를 다음과 같이 변경하도록 하자.

```java
package com.dan.practice.demos.myapp;

import org.springframework.stereotype.Component;

@Component
public class MyService {
    public void doSomething() {
        System.out.println("Bob, do something.");
    }
}
```

xml 파일에서 ClassPath 설정을 없애버려서 Component scan이 작동하여야만 앱이 성공적으로 실행 될 수 있다.

```
10:51:46 PM: Executing ':App.main()'...

> Task :compileJava UP-TO-DATE
> Task :processResources
> Task :classes
> Task :App.main()

BUILD SUCCESSFUL in 885ms
3 actionable tasks: 2 executed, 1 up-to-date
10:51:47 PM: Execution finished ':App.main()'.
```

성공적으로 실행되었다.

스프링에서 환경설정을 바꾸는 3가지 방법을 알아보았다.

다음에는 스프링 코어 개념 중 하나인 의존성 주입을 다루어보도록 하겠다.









