---
title:  "자바/메이븐의 부풀려진 의존성"
header:
  teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"
categories: 
  - Java, Mavern, dependency
tags:
  - Java
---
      
  자바 프로젝트는 여러개의 자바 소스 파일들과 환경설정 파일들로 이루어져있다.

메이븐은 이 각각의 소스파일들간의 의존성을 pom.xml이라고 하는 파일에 저장하여서

의존성 문제를 해결해주는 빌드 도구이다.

pom.xml은 xml 파일로서 각각의 파일들의 의존성을 트리구조로 표현한다.

서로 다른 소스 파일들의 의존성을 하나하나 테스트 하면서 자바 프로젝트를 개발하는 것은

너무나도 피곤한 일이다. 이 의존성들을 자동으로 테스트하고 추천해주는 도구인 Dependantbot는

Github에서 개발자들이 많이 쓰는 봇으로 이 번거로운 작업을 피할 수 있는 방법들 중 하나이다.

Soto-Valero, Durieux, Baudry의 연구에 따르면 Github에서 구한 약 31,000 개의 메이븐 의존성 트리들 중에서

무려 89%가 불필요한 의존 관계를 가지고 있는 부풀려진 의존성(bloated dependency)를 가지고 있다고 한다.

이 부풀려진 의존성으로 개발자가 의존성 환경설정을 다시 업데이트 하여야 했던 경우가 전체 중에 약 22%를 차지하였다.

부풀려진 의존성의 원인들은 여러가지가 있다. 개개인 개발자들이 수정시키면서 pom.xml이 수정이 안된 경우, Dependantbot이

쓰이지 않는 의존성을 넣은 경우, 일시적(transitive)으로 쓰이는 의존성이 더이상 사용되지 않음에도 수정되지 않은 경우, 

원래 의존성이 부풀려진 환경설정을 그대로 가져와서 쓰는 경우등이 있다.



연구에서 DepClean이라는 오픈소스 도구를 이용하여서 얼마나 의존성 트리가 부풀려져 있는지 확인하였다.

DepClean의 깃허브 주소는 아래와 같다.

출처:

https://github.com/castor-software/depclean

DepClean은 필요없는 의존성을 줄여주는 debloating 도구로서 메이븐 빌드의 성능, 메모리 사용 및 컴퓨터 자원 사용의

효율성을 위해서 사용하는 것을 권장한다.


출처: 

link: https://arxiv.org/abs/2105.14226

link: https://arxiv.org/pdf/2105.14226.pdf