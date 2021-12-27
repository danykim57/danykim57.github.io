---
title:  "Spring IoC"
header:
  teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"
categories: 
  - Spring
tags:
  - Spring
---
  
  기존에는 자바 어플리케이션을 개발하던 개발자들이 모든 객체들을 생성하고 의존관계를 연결해주었다.
  
  자바의 서블릿 라이브러리와 EJB(Enterprise Java Bean)가 나오면서 개발자가 객체에 대한 제어권을
  
  라이브러리와 컨테이너에게 넘겨주는 형태가 되었다. (항상 사용자가 자율적으로 무언가를 조정하는 것보다
  
  도구의 개발자가 제어권을 좀 더 가져가는 것이 좀 더 안정적이다)
  
  Spring에서 제어역전(Inversion of Control) 컨테이너가 객체를 생성하고 의존성을 부여해주는 역할을 한다.
  
  제어역전(IoC)과 의존주입(DI)는 동의어라고 봐도 무방하다.
  
  org.springframework.beans와 org.springframework.context가 스프링 IoC 컨테이너의 핵심이다.
  
  컨테이너에는 BeanFactory가 웹어플리케이션의 환경구성과 기본 기능을 지원한다. 
  
  ApplicationContext는 기업용 기능을 추가적으로 지원한다.
  
  IoC 컨테이너에 의해서 관리되는 객체들을 빈(Bean)이라고 한다.
  
  수정중...
  
  
  
  
  

   
  
[^posts]: Footnote test.
