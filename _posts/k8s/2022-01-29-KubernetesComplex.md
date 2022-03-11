---
title:  "쿠버네티스는 왜 복잡한가"
header:
teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"
categories:
- BackEnd 
tags:
  - BackEnd
---
  스터디 중에 쿠버네티스를 왜 이용하는지에 대해서 말하는 시간이 있었다.

***쿠버네티스에서 오는 이점들***

- **앱 배포의 단순화**
- **더 효율적인 하드웨어 자원의 이용**
- **헬스체킹과 자가복원**
- **자동 스케일링**
- **앱 개의 단순화**

여기서 의문이 드는 점은 헬스체킹 부분이다.

실제로 쿠버네티스를 이용하는 스터디원에 따르면 쿠버네티스는 오류가 생길 경우에

확실하게 오류가 생겼다는 표시를 안해주는 경우가 있다.

예를 들어서, 자바 앱들은 오류가 생길경우에 멈춰 버린다. 그렇지만 쿠버네티스는

오류가 생기면 자가복원을 하기 위한 준비상태에서 사람이 조치를 취하지 않으면 서비스가 겉으로만 멀쩡하고

안에서는 하나도 안돌아가는 현상이 벌어진다.

이런 일이 일어나는 이유는 밑의 출처의 저자에 따르면 쿠버네티스가 가지고 있는 디자인 설계에 의해서 

일어나는 현상이다.

저자에 따르면 쿠버네티스는 다목적 클러스터 OS 커널로서 개발되었다.

좋은 운영체제가 가져야할 요소들에는 자원의 효율적 분배, 다른 시스템에서도 돌아가는 휴대성, 새로운 컴퓨터 자원의

추가에도 작동하는 일반성, 모든 컴퓨터 자원을 관리할 수 있는 전체성, 적은 비용에 최대의 효율을 보이는 성능이 있다.

이런 요소들과 함께 분산 시스템과 클라우드 서비스를 굉장히 자주 이용해야하는 구글 회사의 환경에서

심어지고 길러진 것이 쿠버네티스이다.

그렇다면 이런 의문이 들 수 있다.

"구글이라는 좋은 회사에서 분산시스템 환경을 많이 경험하는 개발자들이 만든 기술이 좋은 운영체제의 5요소를 다갖추면 좋은 것 아니냐?"

쿠버네티스의 특징이 장점이 될지 단점이 될지는 상황에 따라서 다르다.

장점이 되는 경우를 보자

1. 내가 하는 프로젝트가 클라우드 서비스를 이용하여야 하고 다른 기술스택으로 이루어진 앱들이 돌아가야만 한다.
2. 사업의 성공여부와 예측되는 서비스 이용량이 불분명하여서 스케일링이 필수적이다.
3. 많은 수의 머신들의 네트워크 설정을 매번 수동으로 다루는 것이 고역이다.

기존의 모노리틱 앱들을 엮어서 마이크로서비스로 전환하거나

분산시스템인 클라우드 서비스를 이용하여야만 하는 경우들에는 쿠버네티스를 활용하는 것이 최선이다.

쿠버네티스는 기존에 모노리틱 설계로 된 앱들도 쉽게 마이크로서비스로 전환하게 도와주고

클라우드 서비스 기능들을 이용하기 쉽게 도와준다.

그러나 단점이 되는 경우를 보자

1. 로드밸런서와 같은 기능이 필요하지 않고 분산 시스템을 이용할 필요가 없다.
2. 기술 스택이 고정적이면서 앱이 돌아가는 환경이 작은 수의 머신에서 돌아간다.
3. 서비스의 트래픽이 늘일이 없거나 아무리 많이 늘어도 스케일링을 할필요가 없다.

이럴 경우에는 쿠버네티스를 쓰는 것은 좋지 않다.

왜냐하면

1. 쿠버네티스는 초기 학습 진입 장벽이 존재한다.

진입 장벽이 높지는 않지만 터미널, 커맨드 프롬프트 같이 콘솔에서 작업을 하는 것이

익숙하지 않은 사람에게는 고역이다.

쿠버네티스가 가지고 있는 코어 개념들을 잘 이해하여야 이것을 잘 이용할 수 있는데

초심자가 하기에는 힘든 부분이다.

2. 쿠버네티스의 오류 출력이 그렇게 친절하지는 않다.

개인적으로 스프링 프레임워크를 정말 좋아하는 것이 오류 출력이 정말 상세하게 되어있다.

쿠버네티스는 오류 출력이 굳이 스프링과 비교를 하자면 그리 좋지 않다.

3. 쿠버네티스가 오류가 생겨도 알 수 없는 경우가 있다.

쿠버네티스는 선언적(declarative)인 설계를 가졌다.

그래서 간편하게 명령어 한줄로 많은 것이 가능하지만

반대로 그 과정을 우리는 거의 알 수 가 없다.

나온 결과가 내가 의도한 것이라면 우리는 과정을 알필요가 없이 좋은 결과를 얻을 수 있지만

그렇지 않는 경우가 헬스체크와 자가복구 기능에서 일어난다.

쿠버네티스의 클러스터는 자동으로 입력된 수치만큼 필요한 자원을 가져다가 쓰는데

모종의 이유들(자원을 다쓰거나, 리포에 있는 이미지가 이전과 다르던가, 필요한 이미지를 찾을 수 없는 경우들)로

시스템이 기능을 하지 않으면 자가복구를 위해 시스템이 기다리게 되는데 이 때

겉으로는 그냥 평소처럼 돌아가는 것 처럼 보인다.

그래서 개발자나 운영자가 실제로 문제자체를 인식하지 못하는 경우가 발생한다.

쿠버네티스의 특징에 따른 장단점을 조금 살펴보았다.

그래도 정말 간단하게 여러 앱을 운영할 수 있는 좋은 앱이라는 점은 변함이 없다.

클라우드 서비스를 이용하지 않더라도 로컬에서 node.js가 있어야만 돌아가는 앱을

쿠버네티스로 간단하게 pod을 생성하여서 자바스크립트 앱을 실행시킬 수 있다.

지금까지 쿠버네티스에 대한 글에 대한 포스팅이였다.


 출처: [Two Reasons why Kubernetes is so complex](https://buttondown.email/nelhage/archive/two-reasons-kubernetes-is-so-complex/)