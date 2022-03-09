---
title:  "스프링 기초 시리즈 - 의존성과 ApplicationContext"
header:
teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"
categories:
  - spring
tags:
  - Spring
---
  
  인텔리제이 아이디어를 이용하여서 오픈 소스인 스프링 프레임워크가 어떻게 구성되어있는지 알아보는 시리즈이다.
  
스프링을 사용할 때 주요 참조 사이트는 다음과 같다.

[Spring Doc](https://docs.spring.io/spring-framework/docs/current/reference/html/)

[Spring Guides](https://spring.io/guides)

스프링은 오픈소스 프로젝트로 문서화와 유저 가이드가 잘되어 있다.


이 시리즈에서는 다음의 기술들을 활용한다.

 - IntelliJ IDEA Community
 - Gradle
 - Spring Framework

현재 맥 환경에서 프로젝트를 진행하고 있어서 Windows로 하는 법은 생략하도록 하겠다.



 인텔리제이를 쓰는 이유는 단축키를 이용하여서 간편하게 소스 코드로 넘어갈 수 있고 자동완성 기능의 편의성등 IDE로서

훌륭하게 생산성을 향상시켜준다.

 빌드 패키지 기술로서 Gradle이 간편하게 사용할 수 있어서 Gradle을 사용하도록 한다.

 스프링 부트를 이용하지 않고 수동으로 의존성을 찾고 적용해 보았다.



인텔리제이에서 Gradle과 자바버젼(여기서는 openjdk-17) 선택하고 신규 프로젝트를 만들었다.

{% include figure image_path="/assets/images/sf1-1.png" alt="sf1" %}

프로젝트 이름 작성

{% include figure image_path="/assets/images/sf1-2.png" alt="sf2" %}

프로젝트 생성 후에 그레이들이 빌드를 해준다.

{% include figure image_path="/assets/images/sf1-3.png" alt="sf3" %}

의존성을 찾게해주는 사이트가 여럿 있는데 그 중에서 우리는 서치 메이븐을 사용할 것 이다.

이제 서치 메이븐으로 가서 필요로한 스프링을 찾아보자.

[search maven](https://search.maven.org/)

메이븐 리포지토리라는 사이트도 있으니 관심이 있으면 확인을 해보자.

[Maven Repo](https://mvnrepository.com/)

아래처럼 검색 창에 치고 마젠타로 그려놓은 버젼 숫자를 클릭해자.

{% include figure image_path="/assets/images/sf1-4.png" alt="sf4" %}

들어간 사이트에서 그려진 그레이들의 의존성 코드를 복사 해서 build.gradle에 넣어주면 된다.

{% include figure image_path="/assets/images/sf1-5.png" alt="sf5" %}

맨 왼쪽에 build.gradle을 선택하여서 implementaion으로 시작하는 스프링 컨텍스트 의존성 코드를 넣어주고 오른쪽 Gradle을 클릭하고

새로 고침(Reload) 버튼을 누르면 다시 프로젝트가 빌드가 되면서 스프링 컨텍스트를 쓸 준비가 되었다.

{% include figure image_path="/assets/images/sf1-6.png" alt="sf6"%}

프로젝트 디렉토리 밑에 다음과 같이 패키지와 자바 클래스를 만들어보자.

{% include figure image_path="/assets/images/sf1-7.png" alt="sf7"%}

Main 클래스를 다음과 같이 구성하였다. 

```
package com.dan.practice.demos.myapp;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.FileSystemXmlApplicationContext;

public class App {
    public static void main(String[] args) {
        ApplicationContext ctx;
    }
}
```

ApplicationContext를 클릭하고 Cmd + B를 누르면 소스 코드를 보여준다.

다음은 ApplicationContext 인터페이스가 어떤 다른 인터페이스를 확장하는지 보여주는 부분이다.
```
public interface ApplicationContext extends EnvironmentCapable, ListableBeanFactory, HierarchicalBeanFactory,
		MessageSource, ApplicationEventPublisher, ResourcePatternResolver
```

각각의 확장시키는 인터페이스들 마다 Cmd + B를 누르면 주석에 어떠한 역할을 하는지 알려준다.

EnvironmentCapable은 Environment라는 인터페이스를 노출시켜준다. 

Environment 인터페이스는 사용자가 만든 환경설정 파일에 따라서 앱의 환경을 설정 시켜주는데 만약에 없을 경우에

기본 환경설정 파일을 적용 시킨다.

Environment는 PropertyResolver를 확장하는데 PropertyResolver는 property key가 맞는지 아닌지를 알려준다.

이와 같이 스프링은 명령적(imperative)이 아닌 선언적(declarative)으로 사용되도록 설계되어있다.

실제 사용자는 Environment나 PropertyResolver를 알지 못하더라도 스프링 프레임워크가 지원하는 기능들을 충분히 이용할 수 있다.

인터페이스간의 설계를 철저히 Single Responsibility를 준수하여서 각각의 인터페이스가 어떤 역할을 하는지

정확하게 명시되어 있다.

ListableBeanFactory는 Bean 팩토리에 Bean 정의(Bean definition)이 들어갔는지 확인해준다.

HierarchicalBean는 BeanFacotry 간에 상속관계를 나타내게 도와준다.

이 두가지 인터페이스는 BeanFactory를 확장한다. 

BeanFactory는 빈을 만드는 역할을 하는 인터페이스고 Bean은 우리가 만들 설정에 의해서 생성된 앱의 객체이다.


MessageSource는 Strategy 패턴을 이용하여서 I18N(국제화)를 도와준다.

Strategy 패턴은 행동들의 덩어리들을 객체로 만들어서 필요할 때 기존의 context 객체랑 바꿀 수 있도록 하는 디자인을 말한다.

ApplicationEventPublisher는 event publication 기능들을 캡슐화 시켜준다.

ResourcePatternResolver는 pseudo(가짜) url 접두사를 만들어서 classpath를 이용할 수 있도록 도와준다.



지금까지 Gradle 설정, ApplicationContext에 대한 간단한 설명을 하였다.

스프링의 환경설정은 크게 두가지로 xml과 annotation이 있다.

다음에는 xml을 어떻게 사용하는지 써보겠다.



