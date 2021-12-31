---
title:  "거래시스템 - 개요"
header:
  teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"
categories: 
  - 설계
tags:
  - 설계
---

출처: link: https://web.archive.org/web/20110219163418/http://howtohft.wordpress.com/2011/02/15/building-a-trading-system-general-considerations/
     link: https://web.archive.org/web/20110219163448/http://howtohft.wordpress.com/2011/02/15/how-to-build-a-fast-limit-order-book/
     
   와우, 룬스케이프등 MMORPG에서 많이 적용되면서 가장 유저들에게 친숙한 경매장 시스템의 베이스인 거래시스템에
  
 대해서 이번 포스트에서 말해보고 실제로 자바와 스프링 프레임워크로 단순한 거래시스템의 기능 중 하나인 주문 대장을 만들어보는 것을 다음 포스팅에서 해볼려고 한다.
 
 이 글에서는 10년전 위의 글들의 저자인 wkselph가 거래시스템을 만들때 주의점을 정리해본다.
 
 첫번째 문단에서 wkselph는 SOLID 디자인 원칙의 인터페이스 분리 원칙(Interfacee Segregation Principle)에 대해서 강조한다.
 
 거래시스템의 큰 두개의 축들은 실제 비즈니스 로직(알고리즘)파트와 빠르게 거래 데이터를 주고 받게 해주는 인터페이스가 중요하다.
 
 여기서 인터페이스 분리 원칙을 통해서 거래 시스템이 변화하는 환경과 상황에 빠르게 수정될 수 있도록 해주는 것이 중요하다.
 
   인터페이스 분리 원칙이 지켜지기가 힘든 이유는 어디까지 인터페이스로 보아야하고 어디까지 실제 구현으로 보아야하는지 구분하기가
   
 힘들기 때문이다. 예를 들어서 거래시스템은 빠른 빈도수의 거래량이 처리가 되어야한다. 사용자 수가 몇명이고 얼마나 사용자가 거래시스템에
 
 자주 요청을 하느냐에 따라서 실제 처리량은 달라지겠지만 거래시스템이 최대한 거래를 처리를 해주기 위해서는 동시성이 충족이 되어야한다.
 
   앞의 포스트에서 언급했듯이 동시성은 결국 다수의 개체들이 데이터베이스에 있는 공유자원을 조회하거나 수정을 할 때에 문제가 되지 않도록
   
 접근 순서를 정확하게 배정해주는 것이 관건인데 이것을 인터페이스를 제작할때 확실하게 접근 순서를 규명해주지 않는다면 이 인터페이스를
 
 기반으로 만들어지는 모든 구현체들이 동시성이 제대로 처리가 안되는 상황이 연출될 것 이다.
 
   고로 동시성 문제를 매번 구현에서 처리해주는 것이 아니라 인터페이스에서 정리가 되게 하여서 거래시스템의 사용량이 폭증하더라도
   
 빠르게 대처가 가능하도록 해주는 것이 좋은 시스템 디자인이다.
 
   
   
   거래시스템의 기능중 하나인 한계 주문 대장(Limit Order Book)에 대해서 이야기 해보자.
   
 한계 주문 대장은 사용자가 원하는 수량, 가격에 해당 상품이 재고와 가격이 맞으면 거래를 진행시켜주는 형태의
 
 주문 대장이다. 주식 시장 어플레케이션에서 사용자가 주식의 수량, 가격을 입력하고 신청을 넣으면 
 
 수량과 가격이 맞는 대로 자동으로 매수를 해주는 형태이다.
 
   한계 주문 대장의 기능은 크게 3가지로 신청, 취소, 실행이 있다.
   
 대량의 요청들을 처리하기 위해서 위 3가지 기능이 가능한 O(1)로 만들게 하는 것이 중요하다.
 
 성능을 위해서 데이터 구조를 해쉬테이블로 만드는 것이 권장되고 해쉬테이블을 쓰기 위해서
 
 각각의 주문들이 고유의 id 번호를 가지게 하는 것이 필요하다.
 
 클래스들은 다음과 같은 멤버 변수들을 가진다.
 
     Order
      int idNumber;
      bool buyOrSell;
      int shares;
      int limit;
      int entryTime;
      int eventTime;
      Order *nextOrder;
      Order *prevOrder;
      Limit *parentLimit;

    Limit  // representing a single limit price
      int limitPrice;
      int size;
      int totalVolume;
      Limit *parent;
      Limit *leftChild;
      Limit *rightChild;
      Order *headOrder;
      Order *tailOrder;

    Book
      Limit *buyTree;
      Limit *sellTree;
      Limit *lowestSell;
      Limit *highestBuy;
      
   출처: link: https://web.archive.org/web/20110219163448/http://howtohft.wordpress.com/2011/02/15/how-to-build-a-fast-limit-order-book/
 
 Limit 객체들은 트리 구조(O(log N))를 취하도록 인터페이스를 짜준다. Order 객체들이 양방향 링크드 리스트(O(N))로 연결되게 하여서 순서를 잡아준다. 
 
 Limit 클래스의 매수 매도트리들을 각 대장(Book)별로 따로 만들어서 관리해준다.
 
 Limit의 해쉬테이블 구조를 트리로 만드는 방법으로 위에 설명하였지만 이외에도 대부분의 값이 0으로 이루어진 스파스 어레이(Sparse Array)로 구현하는 방법도 있다.
 
 트리로 해쉬테이블을 구현할 경우 신청 기능의 성능이 O(N)이 나오지만 취소와 실행이 O(1)이 나오게 구현할 수 있다.
 
 스파스 어레이로 해쉬테이블을 구현할 경우 신청 기능의 성능이 O(1)이 나오지만 취소와 실행이 O(N)이 나온다.
 
 
 
   자바와 같이 메모리 정리 기능(Garbage Collector)가 돌아갈 경우에 배치 할당(Batch allocation)을 사용하여서
   
 메모리 정리 기능을 끄고 거래 처리 시간을 더 늘리는 방법도 있다.
   
  

   
  
[^posts]: Footnote test.
