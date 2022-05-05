---
title:  "레이스 컨디션 - 4"
header:
  teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"
categories: 
  - os
tags:
  - Concurrency
---

   세마포어의 높은 자원 이용량과 모니터의 컴퓨터 언어적 한계성을 극복하기 위한 해결법이
  
 "메시지 패싱"이다.
 
  메시지 패싱은 두가지 기본적인 함수: send와 receive를 사용한다.
  
 - sender와 receiver는 서로 메시지를 주고 받으면서 통신을 한다.
 
 - 어떤 컴퓨터 언어에서도 구현이 가능하고 세마포어처럼 많은 자원을 사용하지는 않지만 통신을 기반으로 하는 해결법이라서 컴퓨터 네트워크에 영향을 많이 받는다.
 
 - sender와 receiver는 네트워크에서 통신을 주고 받으며 만약에 메시지가 잃어버려질 경우를 고려해서 special acknowledgement message라는 특별 승인 메시지를 receiver가 sender에게 보내도록 되어있다.(TCP/IP와 흡사하다)
 
 - 만약에 sender가 보낸 통신이 순서가 뒤바뀌거나 빠지거나 중복된 통신을 보내는 경우들을 방지하기 위해서 각각의 통신에 연속적 숫자(consecutive number)를 부여하여서 순서에 맞게 receiver가 받도록 도와준다.
   
   
 - 네트워크 기반인 메시지 패싱은 해당 송신자와 수신자가 우리가 원하는 개체들인지 확인하기 위해서 authentication(인증) 작업을 한다.
 - 라우터와 여러다른 ISP(Internet Service Provier), 인터넷 서비스 공급자 들을 거쳐서 통신이 되는 가정하에서 만들어진 방법이기에 한 컴퓨터 내에서 이용되는 경우에 여러 불필요한 작업들을 실행하여서 효율성이 조금 떨어질 수 있다.
 
 
 
 - 메시지 패싱에서 sender와 receiver 사이에 생산자-소비자 문제가 생길 수 있다. 이를 해결하기 위해서 mailbox라는 데이터 구조를 버퍼로서 이용을 하여 통신들을 대기시키거나 통제한다.
 
 - 버퍼로서 락을 걸어주는 mailbox를 사용하지 않고 즉각적으로 sender와 receiver가 통신을 알아서 주고 받도록 하는 방법을 랑데뷰(rendevous)라고 한다.
  
  
  마지막으로 알아볼 동기화 기법은 배리어(barrier)이다.
 
 - 배리어는 두 개체 사이가 아닌 세개 이상의 개체들간의 동기화를 고려하여서 만들어진 기법이다.
 
 - 다수의 프로세스들이 동시에 작업에 들어갈 경우 각각의 작업들을 페이스라고 하는 단계들로 나누어서 진행을 시키다가 공유자원에 들어가거나 다른 프로세스의 작업 결과물이 필요한 구간을 배리어로 지정시켜서 다른 프로세스가 작업을 끝날 때까지 기다리게 만드는 것 이다.
 
 책에서 나오는 열역학을 이용한 예제보다 아래의 영상이 더 쉽고 자세하게 설명을 하여서 아래 링크를 공유한다.

출처: 

Modern Operating Systems 4th Edition by Andrew Tanenbaum

https://www.youtube.com/watch?v=M1e9nmmD3II
  
  
