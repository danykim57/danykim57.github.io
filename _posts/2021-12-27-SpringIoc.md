---
title:  "Spring IoC"
header:
  teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"
categories: 
  - Spring
tags:
  - Spring
---

  출처: link: https://docs.spring.io/spring-framework/docs/current/reference/html/core.html
  
  기존에는 자바 어플리케이션을 개발하던 개발자들이 모든 객체들을 생성하고 의존관계를 연결해주었다.
  
  자바의 서블릿 라이브러리와 EJB(Enterprise Java Bean)가 나오면서 개발자가 객체에 대한 제어권을
  
  라이브러리와 컨테이너에게 넘겨주는 형태가 되었다. (항상 사용자가 자율적으로 무언가를 조정하는 것보다
  
  도구의 개발자가 제어권을 좀 더 가져가는 것이 좀 더 안정적이다)
  
  Spring에서 제어역전(Inversion of Control) 컨테이너가 객체를 생성하고 의존성을 부여해주는 역할을 한다.
  
  org.springframework.beans와 org.springframework.context가 스프링 IoC 컨테이너의 핵심이다.
  
  컨테이너에는 BeanFactory가 웹어플리케이션의 환경구성과 기본 기능을 지원한다. 
  
  ApplicationContext는 기업용 기능을 추가적으로 지원한다.
  
  IoC 컨테이너에 의해서 관리되는 객체들을 빈(Bean)이라고 한다.
  
 
 
 
  org.springframework.context.ApplicationContext는 Spring IoC 컨테이너를 위한 인터페이스이다.
  
  앞에서 말한바와 같이 Spring IoC 컨테이너는 웹어플리케이션의 환경구성을 가능하게 해주는데
  
  웹어플리케이션 환경구성은 3가지: XML, 자바 어노테이션, 자바 코드이 있다.
  
  
  제어역전(IoC)을 구현할 때 의존 검색(Dependency Lookup)과 의존 주입(Dependency Injection)이 있다.
  
  IoC 컨테이너가 지원하는 API를 이용하여서 빈 저장소에서 검색을 하는 것이 의존 검색이다.
  
  사용자가 빈 설정파일에 의존관계를 추가로 입력해주면 IoC 컨테이너가 자동으로 객체간의 의존성을 조정해주는 것이
  
  의존 주입(Dependency Injection)이다. 
  
  그 외에 좀 더 다양한 방식의 IoC 컨테이너 운영 방식은 맨 위의 Spring Doc에서 찾아볼 수 있다.
  
  
  
  
  
  

   
  
[^posts]: Footnote test.
