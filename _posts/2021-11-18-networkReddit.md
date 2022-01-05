---
title:  "HTTP와 사업성"
header:
  teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"
categories: 
  - Network
tags:
  - 잡설
---

CERN에서 근무하던 팀 버너스 리가 어떻게 하면 좀 논문을 언제 어디서든 간편하게 읽을까하고 만든 것이 HTTP(Hyper Text Transfer Protocal)의 시초이다.

HTTP는 어플리케이션 레이어에 있는 많은 프로토콜중 하나로서 단순하게 글짜와 이미지를 언제 어디서든 볼 수 있도록 만드는데 최적화 되어있다.

즉 이용자가 20세기 말에 컴퓨터라는 비싼 도구를 쓰는 연구를 하는 학자들을 위한 프로토콜이였다.

이런 특징으로 HTTP는 사용자가 누구인지 상관하지 않고 그저 사용자가 요청하는 저장된 글이나 이미지를 넘겨주는 것만 중점적으로 했다.

기존의 네트워크 프로토콜들의 대부분이 보안이 취약하거나 권한 생성 및 이임 기능을 따로 만들어야했던 이유이기도 하다.

이렇게 사용자가 누군지 알 필요가 없다고 하여 HTTP의 특성 중 하나를 Stateless라고 한다.

Stateless의 장점은 그만큼 사용자가 누구인지 알필요가 없기 때문에 네트워크간에 주고 받는 신호, 패킷의 사이즈가 작아지게 된다.

Stateless에 대해서 더 자세히 알고 싶으면 그 유명한 RESTful Api 논문을 읽어 보는 것을 추천한다.


HTTP의 장점은 가능한한 많은 유저가 언제 어디서든 오류가 없는 정보를 열람 할 수 있도록 하는 것 이다.

재밌는 점은 인터넷 웹페이지의 홈페이지를 추구하는 레딧은 이것을 반쯤 거부 하고 있다.

정확하게 레딧은 정확한 데이터는 넘겨주지만 내가 보았던 포스트나 페이지를 다시 찾는 것은 거의 불가능에 가깝다.

이건 시스템 설계자의 사업적인 의도가 깔려 있는 것으로 보인다.

네트워크 서비스 설계자들의 지상 최대의 목표는 언제나 이용자가 쓸 수 있는 시간을 모두 그 사이트에 이용하도록 하는 것이다.

HTTP 프로토콜의 장점인 '언제나' 같은 글과 이미지를 열람 가능하다는 점은 이 비즈니스 모델에 부합하지 않다.

레딧 설계자가 원하는 것은 언제나 사용자가 컴퓨터로든 핸드폰으로든 레딧 웹페이지를 확인하고 이용하길 바라기에

게시물 검색 기능을 못쓸정도로 방치해 두었다.

구글이 어떤 식으로든 자신들의 검색 엔진을 통해서 다른 페이지로 넘어가게한 설계와는 정반대의 성향이다.


즉, 기술이라는 것은 현재 주어진 상황과 목적에 따라서 선택을 하고 그 장점 또한 일부러 반감 시켜야한다는 것이다.

레딧의 검색 기능의 방치를 잘했다거나 못했다고 주장하는 것이 아니다.

'그' 시스템 설계자는 자신의 상황에 부합하게 기술을 이용했을 뿐이다.

개발자들이 기존에 있는 기술들을 뜯어고치는 것도 같은 맥락이다.

맞다트린 문제가 새로운 기술을 만드는 것보다 기존에 있는 것을 수정하는 것이 더 효율적이기 때문이다.

그에 대한 예로서 쿠키 기능이나 HTTPS, SSL이 있다.




[^posts]: Footnote test.