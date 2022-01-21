---
title:  "Java 동시성"
header:
  teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"
categories: 
  - Java
tags:
  - Java
---
   CPU에서 코어와 쓰레드 수가 늘어남에 따라 소프트웨어에서도 동시성을 지원하게 되었다.
  
   프로세스와 쓰레드
   
  프로세스는 메모리에 올라간 프로그램을 뜻하며 현재 컴퓨터 상에서 돌아갈 준비가 되어있는 인스턴스를 뜻한다.
  
  컴퓨터 운영체제가 프로세스를 만들 때 여러 컴퓨터 자원을 할당해주는데 이 때 비용이 발생하게 된다.
  
  쓰레드는 프로세스 내에서 작업을 위한 실행부분만을 담당하는 부분으로 프로세스 내에서 여러개의 쓰레드가
  
  만들어지고 실행될 수 있다. 쓰레드의 핵심은 생성에 프로세스 보다 비용이 적게 든다는 부분이다.
  
  운영체제가 프로세스들을 관리하는 것처럼 프로세스가 프로세스 내에서 만들어진 쓰레드들을 관리하게 된다.
  
  병렬처리(Parallelism)는 두개 이상의 작업을 돌리는 것으로 프로세스와 쓰레드 둘다 병렬처리가 가능하다.
  
   동시성 설명에 앞서 일반적으로 하나의 쓰레드로 자바를 실행할때 어떤 일이 일어나는지 알아보자.
   
  1. 개발자가 java Main을 실행시킨다.
  2. Java Virutal Machine(JVM)이 실행된다.
  3. JVM이 전달인자(argument)를 확인하면서 사용자가 Main 클래스에 main함수 실행요청을 했는지 본다.
  4. Main 클래스가 로드 되면서 할당된 지정 쓰레드가 생성된다.
  5. JVM의 바이트코드 인터프리터가 메인 쓰레드에서 실행된다.
  6. 메인 쓰레드에 인터프리터가 Main Class에 main 함수를 읽고 바이트코드 사이즈로 실행한다.

 여기서 중요한 것은 JVM이 쓰레드의 생성과 실행을 관리한다는 점이다.
 
 JVM이 생성한 메인 쓰레드는 운영체제에서 자원 할당을 요청하고 받은 자원의 관리 및 이용권한을 얻게 된다.
 
  자바의 메모리 관리 방식처럼 최근의 자바 버젼들에서는 동시성에서도 가능한 개발자가 신경을 덜 쓰도록 지원을 해준다.
  
 쓰레드의 생성은 개발자가 지정해주어야 하지만 다쓰인 쓰레드는 알아서 처리되도록 자바가 지원을 해준다.
 
  쓰레드에는 생명주기를 가르키는 상태들이 있다.
  
  1. New: 막 쓰레드가 생성되고 start() 함수가 실행되지 않은 상태이다.
  2. Runnable: 쓰레드가 돌아가거나 운영체제에 의해서 자원 할당이 되어서 돌아갈 준비가 된 상태이다.
  3. Blocked: 락을 아직 못받은 상태이다. synchronized된 함수나 블록에 들어가기 위해서는 락이 필요하다.
  4. Waiting: Object.wait()이나  Thread.join()에 의해서 아직 쓰레드가 기다리는 중이다.
  5. Timed-Waited: Thread.sleep(), Object.wait(), Thread.join()으로 일정시간 동안 쓰레드가 기다리고 있는 상태이다.
  6. Terminated: 주어진 업무를 쓰레드가 완료하고 run() 함수에 의해서 정상적으로 종료되거나 예외처리로 삭제된 상태이다.


  일반적인 경우에 모든 쓰레드는 고유의 스택 메모리를 가지고 있고 하나의 힙 메모리를 공유한다.
 
 각각의 인스턴스들이 특정 자원(e.g 메모리 주소)을 공유할 권한이 있냐 아니냐를 두고 이것을 인스턴스가 볼 수 있냐 아니냐로 표현을 하는데 이걸
 
 가시성(Visibility)라고 한다.
 
  쓰레드들은 기본 설정으로 서로가 가지고 있는 레퍼런스를 볼 수 있다고 표현한다.
  
  프로세스들을 쓰는 대신에 쓰레드들을 쓰는 이유가 공유 메모리 자원 공유에서 통신 비용이 쓰레드가 훨씬 싸기 때문이기에
  
  쓰레드들이 서로 자원 공유를 할 수 있도록 기본 설정을 한 것 이다.
  
  두개 이상의 쓰레드가 공유자원을 작업하면서 서로 충돌을 일으킬 수 있는데 이에 대한걸 동시성 안전이라고 한다.
  
   동시성 안전은 데드락과 같이 쓰레드들이 서로 일을 못하도록 교착상태로 들어가거나 값이 잘못 수정되는 것을 방지하기 위함이다.
   
  쓰레드의 교착상태가 일어나면서 제대로 작동을 안하면 컴퓨터 운영체제가 강제적으로 이 쓰레드를 제거할 수 있다.
  
  잘못된 값이 저장될 수 있는 경우에는 제외(Exclusion)과 보호(protecting)상태를 부여해주어야 한다.
  
  쓰레드가 공유자원에 접근할 때 다음의 절차를 걸친다.
  
  1. 쓰레드가 객체 수정이 필요 할 때 객체를 비지속적인(inconsistent) 상태로 잠시 만들어 둔다.
  2. 쓰레드가 모니터를 획득하여 일시적인 제외적 접근을 한다고 표시한다.
  3. 쓰레드가 객체를 수정하고 객체럴 지속적(consistent) 상태로 만들고 나간다.
  4. 쓰레드가 모니터를 놓아준다.
  
  모니터를 획득하는 것으로 다른 쓰레드가 객체에 접근하는 것을 막는 것은 아니다. 모니터는 다른 쓰레드가
  
  락을 추가적으로 획득하는 것을 방지하기 위한 장치이다.
  자바에서는 모니터를 획득하는 함수에 Synchronized 키워드를 붙인다.
  
  Synchronized와 다르게 해당 값이 자주 바뀔 수 있다고 명시해주는 키워드인 volatile이 있다.

   
  
출처: 
