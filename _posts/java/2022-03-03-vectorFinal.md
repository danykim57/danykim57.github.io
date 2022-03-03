---
title:  "Vector 클래스의 실패"
header:
teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"
categories:
 - java
tags:
 - BackEnd
---
 
 자바에서 Vector 클래스가 버려진데에는 크게 2가지 이유가 있다.

 - final의 남용


final 키워드를 쓰는 경우에는 2가지가 있다.

 1) 디자인적으로 변경을 못하게 해야하는 이유.

         클래스, 메소드, argument, 변수 등 디자인적으로 변경, 상속이 문제를 일으킬 때 final로 변경을 불가하게 한다.

 2) 컴파일러에서 method call이 아닌 method body를 통째로 실행시켜서 method call 오버헤드를 안겪고 싶을 때.

        method call은 arguments를 스택에 넣고 다시 method 코드로 돌아와서 코드를 실행시킨다.       
        그리고 다시 스택으로 돌아가서 사용하였던 메모리를 정리하고 return 값을 처리한다. 이때 스택으로 왔다갔다하는       
        오버헤드 비용이 발생한다. 메소드 바디가 긴 경우에 퍼포먼스가 전혀 안나오기 때문에 권장하지 않는 사용 방법이다.


    Vector의 함수들은 전부 final이다. 클라이언트 프로그래머가 자체적으로 이 함수들의 오버라이드가 불가능하다.               
    
    Vector 함수의 대표적인 서브 클래스는 Stack이다.               
    
    Stack을 쓸 때 Vector의 final 함수들로 인해서 활용이 너무나도 제한적이다. 



 - final 남용에 의한 synchronized의 무용

       addElement()과 elementAt()은 synchronized 키워드를 사용한다.
   
       동시성에서 final을 사용하게 되면 상당히 큰 퍼모먼스 오버헤드가 발생한다.


이러한 이유들로 Vector는 ArrayList로 대체되었다.


- 부록 
      Hashtable 클래스는 Vector 클래스와 정반대 상황이다.
      
      Hashtable은 final 메소드가 없다.
      
      이 클래스 둘이 서로 다른 개발자에 의해서 디자인 되었다고 추론할 수 있는 좋은 증거이다.
      
      Hashtable은 HashMap으로 대체되었다.
      
      Hashtable의 문제는 싱글 쓰레드 경우에도 synchronization 체크를 매번 함으로서
      
      퍼포먼스 문제가 있다.


참조: Bruce Eckel, "Thinking in Java"
