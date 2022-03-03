---
title:  "그레이들"
header:
teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"
categories:
- BackEnd
tags:
- BackEnd
---
 [Gradle Doc](https://docs.gradle.org/current/userguide/what_is_gradle.html#five_things)
 
  그레이들은(Gradle)은 Ant와 Maven과 같은  프로젝트 빌드 자동화 도구이다.

그레이들의 특징으로는 5가지가 있다.

높은 퍼포먼스:
 1. 그레이들은 불필요한 작업을 피하기 위해서 지정된 필수적인 작업에서만 동작을 한다. 
 2. 빌드 캐시를 이용하여서 빌드 속도를 더 빠르게 할 수 있다.

JVM 기반
 그레이들은 JVM 위에서 작동하기 때문에 JDK(Java Development Kit)가 필요하다.
 JVM 위에서 작동하지만 다른 native 앱의 빌드 또한 지원한다.

컨벤션
  그레이들은 Maven의 컨벤션을 준수하면서 최대한 간결하게 만들려고 노력하였다.
  이 컨벤션은 사용자의 취향에 맞게 커스터마이징이 가능하다.

확장성
  작업의 타입이나 빌드 모델에 따라서 그레이들을 확장할 수 있다. 

IDE 지원
  안드로이드 스튜디오, 인텔리제이, 이클립스, NetBean등의 IDE를 지원한다.

인사이트
  Build scans는 빌드 시에 일어나는 에러나 이슈들을 자세하게 알려준다.


추가적으로 중요한 특징들: 

  1. 현재 그레이들은 의존성 관리 리포(repository)를 Maven과 Ivy 계열만을 지원한다.

  의존성 관리 리포는 사용자가 간편하게 미리 만들어진 일반적인 프로젝트의 의존성을 주입 할 수 있도록 도와주는 저장소이다.
  
  2. 그레이들의 기본 단위는 작업(task)이다.
  
  그레이들 코어 모델은 Directed Acyclic Graphs(DAGs) 구조를 이용하여서 올바른 순서로 빠르게 빌드를 할 수 있도록 한다.

  {% include figure image_path="/assets/images/DAGs.png" alt="DAGs" caption="DAGs" %}

  Task는 3가지 요소로 구분되어 질 수 있다.
  Action: 소스 파일 컴파일이나 파일 복사와 같은 다 작은 단위의 작업
  Input: 변수, 파일, 디렉토리와 같은 Action이 쓰는 자원
  Output: Action이 수정이나 생성하는 파일, 디렉토

  Task를 강조하는 이유는 프로젝트 단위 테스트를 하는 경우 자주 빌드를 돌려주어야 하는 경우가 있는데

  그레이들은 테스트를 돌리는 부분만 따로 빌드를 돌릴 수 있게 도와줄 수 있다.

  그리하여 테스트를 돌릴 때에 빌드 시간을 많이 아낄 수 있다.

  3. 빌드 페이즈(phase)
  Initializaion -> Configuration -> Execution
  
  빌드 환경을 조성 -> 작업 그래프(DAG)를 만들고 환경설정을 조정 -> 실행
   
  좋은 빌드 스크립트는 명령적(imperative)이기 보다 선언적(declarative)이어야 한다.

  즉, 사용자가 스크립트의 동작 원리를 이해하지 않고도 이용이 가능하게 하여야 하기 때문에

  Configuration과 Execution 페이즈를 나누어 놓았다.

  4. 커스터마이징
  
  Action, Task, 환경 설정, 플러그인등 다양한 커스터마이징을 그레이들은 지원

  5. 빌드 스크립트
  
  빌드 스크립트는 실행코드 덩어리처럼 보인다. 그렇지만 공식 문서에서는 빌드 스크립트를 가능한한

  선언적(declarative)으로 쓰는 것을 권장한다. 그레이들 API에 맞게 스크립트를 짜는 것이

  대부분의 경우 정답이다. 그러나 빌드 스크립트를 직접 손대서 커스터마이징을 해야할 때가 있는데

  그건 다음에 다루어 보도록 하겠다.
   
  


