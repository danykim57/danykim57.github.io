---
title:  "List와 Vector 차이점"
header:
teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"
categories:
  - BackEnd
tags:
  - c++
---

공통점
- C++ STL에서 제공하는 List(Linked List의 List)와 Vector(동적 배열)는 둘다 동적으로 메모리 할당이 가능하다.

차이점
- 메모리 할당
  - 메모리 할당시 List는 값을 넣을 때마다 새로운 Node를 추가해주면서 메모리를 할당한다.
  - Vector는 특정 사이즈의 연속적인 메모리를 할당하고 그 이상의 값들이 추가되면 할당된 메모리의 2배의 값으로 늘려준다(Bubbling).
  - 그러므로 Vector는 매번 메모리 할당을 하지 않기에 push_back이 List보다 빠르다
  - 추가적인 메모리 할당을 최소화 하기 위해서 Constructor에서 vector의 사이즈를 정해주거나 resize() 함수를 사용하는 방법이 있다.
  - vector에서 재할당이 일어나는 경우에는 이전의 메모리의 값들을 전부 새루운 메모리에 넣어주기 때문에 비용이 크다.
- 메모리 해제
  - Vector는 데이터 구조내의 값을 지우더라도 메모리를 해제하지 않는다. clear() 함수를 이용하더라도 메모리 할당은 여전히 그대로이다.
  - List는 Node 객체를 지울 때마다 객체에 할당된 메모리 할당을 해제해준다.
  - Vector는 뒤에서 부터 값을 해제할 수 있는데 pop_back의 속도가 List보다 빠르다
  - 그러나 데이터의 중간에 있는 값을 삭제할 때는 List가 빠르다. (삼성 기출 문제에서 고려할 대상 중 하나)
  - 중간에 있는 값을 자주 삭제하는 경우에는 Vector보다 List를 고려하는 것이 맞다.
- 메모리 사용량
  - Vector는 연속적인 주소에 할당이 된다. List와는 다르게 다른 주소 변수값을 가지고 있을 필요가 없다.
  - 따라서, vector가 더 적은 메모리를 사용한다. 
- 데이터 접근
  - List는 노드를 방문해주면서 원하는 값을 가지고 있는지 확인하여야하기 때문에 O(N)이 걸린다.
  - Vector는 index를 통한 접근을 지원하기에 O(1)로 접근을 할 수 있다.

출처:
 - (https://jaehogame.tistory.com/entry/STL-Vector%EC%99%80-List-%EC%B0%A8%EC%9D%B4%EC%A0%90)
 - (https://imksh.com/13)
