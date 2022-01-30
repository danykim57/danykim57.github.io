---
title:  "테스트환경의 중요성 - Jekyll"
header:
teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"
categories:
  - BackEnd
tags:
  - BackEnd
---
  
 이 블로그는 Ruby의 Jekyll이라는 정적 사이트 생성자를 이용한 minimalmistake 테마에서 돌아가고 있다.
 
블로그를 개설한지 한 3개월간은 아무 이상없이 운영이 되었다.

예전 글들을 보면 최소한의 퇴고 과정도 없이 바로 업로드를 시킨 포스트가 대다수라서 굉장히 너저분한 것을

알 수 있다. 약 3개월간 글을 쓰는 습관을 길들이기 위해서 글을 쓰는 작업의 상당부분을 차지하는

퇴고과정 자체를 없애버린 결과물들이다. 



 kramdown, 프로젝트 디렉토리 적응 등 여러가지 이유를 핑계로 Jekyll 문서화에서 추천하는
 
로컬 컴퓨터 내의 Jekyll 실행환경을 적용시키지 않았다.

 글을 쓰는 것이 어느정도 적응이 된 나는 블로그 정리를 해주고 싶었고 그에 알맞는 기능이
 
사이드바 기능이였다.

 그 결과, 블로그 왼쪽 사이드바에 네비게이션 기능을 넣어주고 싶어서 여러 YAML 파일과 html 파일을 손보다가 
 
블로그가 배포 실패에 들어가는 현상이 일어났었다.

{% include figure image_path="/assets/images/buildFailed.png" alt="buildFailed.png" caption="무수한 배포 실패의 향연" %}

어딘가 수정이 잘못된 것 이다.

문제는 GitHub Action에서는 배포의 성공여부와 굉장히 짤막하게 원인을 알려주기 때문에

실질적인 문제해결에는 크게 도움이 되지 않는다.

또한 로컬에서 Jekyll을 돌리지 않고 GitHub에서 계속 커밋 푸쉬를 이용하여서 오류가 해결되었는지 확인하다가

문제는 전혀 개선되지 않고 2,3일이 날아가버렸다.

Ruby, Kramdown, Liquid등의 새로운 언어들을 제대로 이해가 안된 상태에서

뭔가 오류를 잡아보려는 시도들이 정말 마구잡이로 구글 검색에서 나오는대로 시도를 하는 것이기 때문에 헛발질만 계속 차고 있었다.

결국은 Jekyll 문서화에서 말하는 것처럼 로컬에서 블로그를 실행할 수 있는 환경이 되어있어야

더 빠르고 정확한 피드백이 들어온다는 것이 정답이다.

[Jekyll QuickStart](https://jekyllrb.com/docs/)

초기에 로컬 환경을 처리하지 않았던 가장 큰 이유가 내가 arm64 기반의 맥북을 쓰고 있기에

분명 다른 예기치 않은 문제들이 일어날 것을 예감했기 때문이다.

새로운 칩기반의 기계 환경에 Ruby, Jekyll에 대한 이해도가 없는 상황에서

그 중에서 맥환경에 기존에 깔려 있는 Ruby 버젼과 arm64간의 충돌이 굉장히 골치 아팠는데

이건 다음 블로그 포스트를 보고 해결 할 수 있었다.

[Mac에서 Gem::FilePermissionError 에러 발생시 해결 방법](https://so-es-immer.tistory.com/entry/%EA%B9%83%ED%97%88%EB%B8%8C-%EB%B8%94%EB%A1%9C%EA%B7%B8-%EB%A7%8C%EB%93%9C%EB%8A%94%EB%8D%B0-%EB%A3%A8%EB%B9%84-%ED%8D%BC%EB%AF%B8%EC%85%98-%EC%97%86%EB%8A%94-%EC%97%90%EB%9F%AC-%ED%95%B4%EA%B2%B0)

제대로 환경 설정을 하고 나서야 이제 제대로된 오류 코드를 볼 수 있게 되었다.

{% include figure image_path="/assets/images/jekyllURLError.png" alt="jekyllURLError.png" caption="nav 파일의 url 에러" %}

좀 더 명확한 오류 코드를 보면서 Git 히스토리를 확인하면서 한단계씩 접근을 하여서

사이드바의 카테고리 기능을 활용할 수 있게 되었다.

결론: 가능하면 기술부채는 좀 잡아가면서 하자. 힘들것 같다고 피하면 언젠가는 또 다시 되돌아온다.