---
title:  "스프링 기초 시리즈 - LifeCycle Call Back"
header:
teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"
categories:
  - spring
tags:
  - Spring
---
  
  @Transaction을 쓴 Service Method Transactional이 어떤 식으로 스프링에서 돌아가는지
  
디버그 모드로 탐색을 해보았다.

  필요한 클래스들
  
App 클래스

```
package com.dan.practice.demos.myapp;

import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class App {
    public static void main(String[] args) {
        ApplicationContext ctx = new AnnotationConfigApplicationContext(AppConfig.class);
        OrderService service = ctx.getBean(OrderService.class);
        service.placeOrder();
    }
}
```

AppConfig 클래스 

```
package com.dan.practice.demos.myapp;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.jdbc.datasource.DataSourceTransactionManager;
import org.springframework.jdbc.datasource.embedded.EmbeddedDatabaseBuilder;
import org.springframework.jdbc.datasource.embedded.EmbeddedDatabaseType;
import org.springframework.transaction.PlatformTransactionManager;
import org.springframework.transaction.annotation.EnableTransactionManagement;

@Configuration
@ComponentScan("com.dan.practice.demos.myapp")
@EnableTransactionManagement
public class AppConfig {

    @Bean
    public PlatformTransactionManager transactionManager() {
        DataSourceTransactionManager transactionManager = new DataSourceTransactionManager();
        transactionManager.setDataSource(new EmbeddedDatabaseBuilder().setType(EmbeddedDatabaseType.H2).build());
        return transactionManager;
    }

}
```

OrderService 클래스

```
package com.dan.practice.demos.myapp;

import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

@Service
public class OrderService {
    @Transactional
    public void placeOrder() {
        //This places an order
        System.out.println("Placing Order...");
    }
}
```

H2 DB를 사용하여야 하기 때문에 build.gradle에 의존성이 추가 되어야한다.

```
dependencies {
    implementation 'org.springframework:spring-context:5.3.16'
    implementation 'org.springframework:spring-jdbc:5.3.16'
    implementation 'com.h2database:h2:2.1.210'
    testImplementation 'org.junit.jupiter:junit-jupiter-api:5.8.1'
    testRuntimeOnly 'org.junit.jupiter:junit-jupiter-engine:5.8.1'
}
```

App 클래스의 10번 째 줄인 service.placeOrder();에 breakpoint를 걸고 디버그 모드를 돌리면서

step into(F7)와 step over(F8)을 눌러가면서 실제로 @Transactional을 받은 메소드가 어떻게 행동하는지 보자.

service는 다음과 같이 SPRINGCGLIB에 의해서 생성된다.

{% include figure image_path="/assets/images/sf9/sf9-1.png" alt="sf9-1"%}

service.placeOrder() 함수가 실행되기 전에 @Transactional에 의해서 CglibAopProxy 클래스의 intercept함수를 요쳥한다.

intercept 함수는 다음과 같다.
```
@Override
@Nullable
public Object intercept(Object proxy, Method method, Object[] args, MethodProxy methodProxy) throws Throwable {
	Object oldProxy = null;
	boolean setProxyContext = false;
	Object target = null;
	TargetSource targetSource = this.advised.getTargetSource();
	try {
		if (this.advised.exposeProxy) {
			// Make invocation available if necessary.
			oldProxy = AopContext.setCurrentProxy(proxy);
			setProxyContext = true;
		}
		// Get as late as possible to minimize the time we "own" the target, in case it comes from a pool...
		target = targetSource.getTarget();
		Class<?> targetClass = (target != null ? target.getClass() : null);
		List<Object> chain = this.advised.getInterceptorsAndDynamicInterceptionAdvice(method, targetClass);
		Object retVal;
		// Check whether we only have one InvokerInterceptor: that is,
		// no real advice, but just reflective invocation of the target.
		if (chain.isEmpty() && CglibMethodInvocation.isMethodProxyCompatible(method)) {
			// We can skip creating a MethodInvocation: just invoke the target directly.
			// Note that the final invoker must be an InvokerInterceptor, so we know
			// it does nothing but a reflective operation on the target, and no hot
			// swapping or fancy proxying.
			Object[] argsToUse = AopProxyUtils.adaptArgumentsIfNecessary(method, args);
			try {
				retVal = methodProxy.invoke(target, argsToUse);
			}
			catch (CodeGenerationException ex) {
				CglibMethodInvocation.logFastClassGenerationFailure(method);
				retVal = AopUtils.invokeJoinpointUsingReflection(target, method, argsToUse);
			}
		}
		else {
			// We need to create a method invocation...
			retVal = new CglibMethodInvocation(proxy, target, method, args, targetClass, chain, methodProxy).proceed();
		}
		retVal = processReturnType(proxy, target, method, retVal);
		return retVal;
	}
	finally {
		if (target != null && !targetSource.isStatic()) {
			targetSource.releaseTarget(target);
		}
		if (setProxyContext) {
			// Restore old proxy.
			AopContext.setCurrentProxy(oldProxy);
		}
	}
}

```

여기서 TargetSource targetSource = this.advised.getTargetSource();를 타고

AdvisedSupport 클래스의 객체에게 TargetSource를 반환해 달라고 요청을 한다.

이 경우의 advised는 protected final AdvisedSupport advised;이다.

TargetSource에서 TargetClass를 찾는데 이경우 TargetClass는 OrderService이다.

List<Object> chain = this.advised.getInterceptorsAndDynamicInterceptionAdvice(method, targetClass);에서

<br>

인터셉터와 다이나믹 인터셉션어드바이져들을 불러온다.

{% include figure image_path="/assets/images/sf9/sf9-2.png" alt="sf9-2"%}

이미지와 같이 chain에 TransactionInterceptor가 들어가 있다.

이제 chain에 있는 TransactionInterceptor의 invoke() 메소드로 invokeWithinTransaction함수를 리턴한다.

chain이 비어져 있지 않기에 if문을 생략하고 else 문으로 들어가서 

retVal = new CglibMethodInvocation(proxy, target, method, args, targetClass, chain, methodProxy).proceed();을 실행하고 retVal를 리턴한다.

대략 이부분 까지가 @Transactional가 붙어있는 메소드를 실행시킬 경우에 일어나는 과정이다.

Step Over를 안쓰면 빈 생성에 관련된 부분들도 볼 수 있지만

이번에는 Proxy가 어떻게 일어나는지 디버깅모드로 확인하는 작업이기에 이정도로 마치겠다.











