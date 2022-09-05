---
title:  스프링 시큐리티 H2 Console 문제
header:
teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"
categories:
  - spring
tags:
  - Spring
---

스프링 시큐리티를 이용할 경우에 혀용되지 않은 요청은 사용이 불가능하다.

H2 Console이 이 경우에 해당한다.

인메모리로 테스트용으로 H2를 이용하여서 H2 Console을 사용해야할 경우에 다음과 같은 설정을 추가로 넣어주어야 한다.

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.csrf().disable()
                //h2console 허용 파트 **프로덕션 때는 꼭 뺴야함
                .antMatchers("/console/**")
                .permitAll()
                .anyRequest()
                .authenticated()
                .and()
                .headers()
                .frameOptions()
                .disable();
        //여기까지 h2console 관련
    }
}
```

각각 csrf를 끄고 console이 들어간 요청에 대한 접근허가와

헤더 보안과 Xframe 옵션을 끄는 항목들이다.

여기서 .headers()를 안꺼도 h2-console은 사용이 가능하다. 

여기서 http.csrf()는 CORS(Cross Origin Resource Sharing)을 할 때

리소스의 출처의 확인의 취약점을 이용한 csrf(Cross Site Request Forgery) 관련 보안 기능이다.
