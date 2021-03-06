---
title:  "성공적인 시스템 설계를 위한 원칙들 - 1"
header:
  teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"
categories: 
  - architecture
tags:
  - 분산시스템 설계
---

  페이스북(현:메타)에서 인프라 설계를 맡았던 Maheshda의 포스팅을 다루어 본다.
  
  Mahesdha는 크게 설계의 원칙을 7개로 나누었다 : 고객, 프로젝트 관리, 디자인, 코드 리뷰, 전략, 측정, 연구.
  
  회사 경영에서 다루는 고객 서비스, 총무, 인사, 회계, 마케팅, 품질 관리, 기술과 굉장히 유사하다.
  
  이 페이지에서는 고객 부분만을 다룬다.
  
  서비스의 가장 중요한 목적은 고객 만족도의 최우선화이다.
  
  많은 스타트업 회사들의 경영진들이 가장 두려워하는 사건은 갑자기 폭발적으로 증가하는 사용자 수다.
  
  폭발적으로 유저수가 늘어난 경우에는 트래픽의 양이라던지 프로세싱 시간이던지 다양한 이유로 고객이 예상하는
  
  서비스질을 맞추지 못하게 된다.
  
  고객들은 기본적으로 한번 불만족스러운 서비스를 받는 경우 앞으로도 계속 안좋은 서비스를 받을 것이라고
  
  생각하는 경향이 크다.
  
  사람의 심리라는 것이 리스크를 지는 것을 굉장히 꺼려하기 때문에 고정적으로 이 불만족스러운 서비스를 받은
  
  고객들은 돌아오지 않을 것이다.
  
  문제는 확장성(scalability)를 또 극단적으로 치중하게 분산시스템을 설계하면 안된다는 것이다.
  
  시스템이나 서비스는 예상되는 적정량의 사용자 수가 있다.
  
  사용자 수를 너무 크게 잡고 설계를 하는 경우에는 회사에서는 안쓰이는 서비스의 자원만큼 기회 비용의 손해를 보게 된다.
  
  그래서 서비스의 핵심 품질과 기술에 엔지니어들이 집중할 수 있도록 고객 수를 적정순으로 조절하는 것이 중요하다.
  
  문제는 이제 고객이 있는 현장(field)과 데스크간의 견해차이가 자연스럽게 나게 되는데.
  
  대부분은 데스크의 직원들이 현장의 상황을 알 수 없기에 벌어지는 일이다.
  
  엔지니어라도 가능한 단순한 글의 형태로라도 고객이 어떤 것을 요구하는지 알 수 있도록 하여야한다.
  
  예상 사용자 수나 서비스와 시스템간의 적절성은 숙련된 직원이라면 관리가 가능하지만
  
  가장 큰 문제는 이제 고객 조차도 자신이 무엇을 원하는지 모르거나 고객이 구두로나 글로 요구를 할 수 없는 경우이다.
  
  말을 하지 않아도 타인이 자신의 마음을 알아주면 좋겠다는 사람들의 심리로 인해서 일어나는 것인데
  
  이런 것들을 알아내기위해서 쿠키나 트래킹등이 발달하게 된것 이라고 본다.
  
  행동심리학만큼 확실하게 증거가 나오면서 고객의 요구를 알 수 있는 방법이 드물기 때문이다.
  
  요약:
  
  1. 고객 만족도가 항상 최우선이다.
  
  2. 실제 이용자수와 이용자의 수준(평균적인 서비스 이해도)을 조절 할 수 있도록 해라.
  
  다음 장에서는 프로젝트 관리 부분을 알아보도록 하겠다.
  
  3. 현장과 데스크간의 입장차(괴리감)을 줄여라

  4. 고객이 간접적으로 요구하거나 말하지 못하는 것을 찾아서 해결해 주어라.
 
 
출처: 

https://maheshba.bitbucket.io/blog/2021/10/19/42Things.html