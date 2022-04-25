---
title:  "자바 Concurrency - Thread"
header:
teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"
categories:
  - java
tags:
  - BackEnd
---

자바에서 쓰레드를 쓰기 위해서는 run() 메소드를 오버라이드 해야하는데 크게 두가지가 방법이 있다.

1. Thread 클래스를 extend로 상속받는다. 
2. Runnable 클래스를 implement로 구현한다.

쓰레드가 실행될 때 start()메소드가 JVM에게 run() 메소드를 끌어오라고(hook up) 요청합니다.

이를 위해서 run() 메소드를 오버라이드 해주어야 합니다.

쓰레드는 run() 메소드가 반환(return)과 함께 끝날 때 까지 계속 실행될 수 있지만

스케쥴러에 의해서 중지(suspend)되었다가 다시 재실행(resume)될 수 있습니다.

run() 메소드가 반환되지 않으면 무한으로 쓰레드는 실행되게 되고

run() 메소드가 반환이 되면 쓰레드는 소멸되게 됩니다.

join() 메소드는 run() 되고 있는 쓰레드를 다른 쓰레드의 작업이 끝날 때까지 기다리게

할 수 있습니다.

일반적으로는 extend보다 implement를 선호합니다. 상속은 하나의 상위 클래스만 받을 수 지만

implement는 여러개를 할 수 있기 때문입니다.

쓰레드는 JVM의 GC(Garbage Collector)에 의해 지워질 수 있습니다.

자바에서 쓰레드는 크게 두개로 나뉠 수 있습니다.

1. 유저 쓰레드(User Thread)
2. 데몬 쓰레드(Daemon Thread)

JVM에 실행될 때 한개의 유저 쓰레드를 가지고 있습니다.

이 첫 유저 쓰레드를 "Main Thread"라고도 합니다.

유저 쓰레드와 데몬 쓰레드는 작업을 마칠 때 차이를 보입니다.

유저 쓰레드는 "Main Thread"보다 더 길게 살아남을 수 있습니다.

그러나 데몬 쓰레드는 유저 쓰레드가 작업을 종료할 때 전부 소멸됩니다.

JVM은 유저 쓰레드가 다 소멸되고 남은 쓰레드가 데몬 쓰레드일 때 종료됩니다.

자바의 데몬 쓰레드 기능들(ForkJoinPool, Timer)은 java.util.concurrent 패키지에 있습니다.



유저 쓰레드와 데몬 쓰레드의 차이점을 보여주는 예제는 다음과 같습니다.

예제에서는 파라미터가 없이 쓰레드를 생성할 경우에 유저 쓰레드를 생성하도록 하고

파라미터가 있는 경우 데몬 쓰레드를 생성하도록 하였습니다.

위의 말들이 맞다면 데몬 쓰레드들은 "Main Thread"의 종료와 함께 사라지고

유저 쓰레드는 살아남아 있어야합니다. 

테스트를 위한 메인 클래스는 다음과 같습니다

```java
package UserOrDaemonThread;

public class TestThread {

    public static void main(String[] args) {
        System.out.println("Entering main()");

        final Boolean daemonThread = args.length > 0;

        UserOrDaemonThread thr =
                new UserOrDaemonThread(daemonThread);
        thr.start();

        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {}
        System.out.println("Levaing main()");
    }
}
```

유저 데몬 쓰레드 구분을 위한 클래스는 다음과 같습니다

```java
package UserOrDaemonThread;

import java.util.Random;
import static java.lang.Math.abs;

public class UserOrDaemonThread extends Thread {
    private final String threadType;

    private final int MAX_ITERATIONS = 100_000_000;

    public UserOrDaemonThread(Boolean daemonThread) {
        if (daemonThread) {
            setDaemon(true);
            threadType = "daemon";
        } else
            threadType = "user";
    }

    private int computeGCD(int number1, int number2) {
        if (number2 == 0) return number1;
        return computeGCD(number2, number1 % number2);
    }

    @Override
    public void run() {
        final  String threadString =
                " with "
                + threadType
                + " thread id "
                + Thread.currentThread();
        System.out.println("Entering run()"
                            + threadString);

        Random random = new Random();

        try {
            for (int i = 0; i < MAX_ITERATIONS; ++i) {
                int number1 = abs(random.nextInt());
                int number2 = abs(random.nextInt());

                if ((i % 10000000) == 0)
                    System.out.println("In run()"
                            + threadString
                            + " the GCD of "
                            + number1
                            + " and "
                            + number2
                            + " is "
                            + computeGCD(number1, number2));
            }
        }
        finally {
            System.out.println("Leaving run() "
                                + threadString);
        }
    }

    public static void main(String[] args) {
        System.out.println("Entering main()");

        final Boolean daemonThread = args.length > 0;

        UserOrDaemonThread thr =
                new UserOrDaemonThread(daemonThread);
        thr.start();
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
        }

        System.out.println("Leaving main()");
    }
}
```

메인 함수를 실행하면 결과값이 다음과 같이 나옵니다.

```
Entering main()
Entering run() with user thread id Thread[Thread-0,5,main]
In run() with user thread id Thread[Thread-0,5,main] the GCD of 379866564 and 1687456990 is 2
In run() with user thread id Thread[Thread-0,5,main] the GCD of 1058901458 and 1756953267 is 1
In run() with user thread id Thread[Thread-0,5,main] the GCD of 1157541804 and 1904073919 is 1
In run() with user thread id Thread[Thread-0,5,main] the GCD of 2017039934 and 668780872 is 2
Levaing main()
In run() with user thread id Thread[Thread-0,5,main] the GCD of 1460909541 and 944213939 is 1
In run() with user thread id Thread[Thread-0,5,main] the GCD of 88972959 and 1454604750 is 3
In run() with user thread id Thread[Thread-0,5,main] the GCD of 840441747 and 1540313653 is 1
In run() with user thread id Thread[Thread-0,5,main] the GCD of 589499215 and 1230152771 is 1
In run() with user thread id Thread[Thread-0,5,main] the GCD of 1635799227 and 1459874504 is 1
In run() with user thread id Thread[Thread-0,5,main] the GCD of 12475471 and 271601495 is 1
Leaving run()  with user thread id Thread[Thread-0,5,main]
```

메인 함수가 종료하더라도 유저 쓰레드는 생존해있는 것을 확인할 수 있습니다.

상단의 IntelliJ IDEA에서 Run -> Edit Configurations에서 커맨드라인 파라미터 값을 다음과 같이 설정해줍니다.

{% include figure image_path="/assets/images/java/jc-1.png" alt="jc-1" %}

다시 메인 메소드를 실행해보면 다음과 같이 출력이 바뀝니다.

```
Entering main()
Entering run() with daemon thread id Thread[Thread-0,5,main]
In run() with daemon thread id Thread[Thread-0,5,main] the GCD of 1647033049 and 1769970559 is 1
In run() with daemon thread id Thread[Thread-0,5,main] the GCD of 705890512 and 1972161377 is 1
In run() with daemon thread id Thread[Thread-0,5,main] the GCD of 2104307692 and 1975961494 is 2
In run() with daemon thread id Thread[Thread-0,5,main] the GCD of 297271339 and 2040471584 is 1
In run() with daemon thread id Thread[Thread-0,5,main] the GCD of 1018251663 and 1271358718 is 1
Levaing main()

Process finished with exit code 0
```

메인 메소드의 종료와 함께 데몬 쓰레드들이 같이 종료되는 것을 확인할 수 있습니다.

아래 예제는 implement로 위의 예제를 구현해놓은 것 입니다.

```java
package UserOrDaemonRunnable;

public class TestRunnable {
    public static void main(String[] args) {
        System.out.println("Entering main()");

        final Boolean daemonThread = args.length > 0;

        GCDRunnable runnableCommand = new GCDRunnable(daemonThread ? "daemon" :"user");

        Thread thr = new Thread(runnableCommand);

        if (daemonThread) thr.setDaemon(true);

        thr.start();

        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {}
        System.out.println("Leaving main()");
    }
}

```

```java
package UserOrDaemonRunnable;

import static java.lang.Math.abs;

import java.util.Random;

public class GCDRunnable
        extends Random
        implements Runnable {

    final private String threadType;
    private final int MAX_ITERATIONS = 100000000;

    public GCDRunnable(String threadType) {
        this.threadType = threadType;
    }

    private int computeGCD(int number1,
                           int number2) {
        if (number2 == 0)
            return number1;
        return computeGCD(number2,
                number1 % number2);
    }

    public void run() {
        final String threadString =
                " with "
                        + threadType
                        + " thread id "
                        + Thread.currentThread();

        System.out.println("Entering run()"
                + threadString);

        try {
            // Iterate for the give number of times.
            for (int i = 0; i < MAX_ITERATIONS; ++i) {
                // Generate two random numbers (nextInt() obtained
                // from Random superclass).
                int number1 = abs(nextInt());
                int number2 = abs(nextInt());

                // Print results every 10 million iterations.
                if ((i % 10000000) == 0)
                    System.out.println("In run()"
                            + threadString
                            + " the GCD of "
                            + number1
                            + " and "
                            + number2
                            + " is "
                            + computeGCD(number1,
                            number2));
            }
        }
        finally {
            System.out.println("Leaving run() "
                    + threadString);
        }
    }
}
```

결과는 extend를 활용한 예제와 똑같이 나옵니다.

Java에서는 Executor 프레임워크를 제공합니다.

쓰레드를 asychronous(비동기)하게 쓸 수 있게 도와주는 도구가 Executor입니다.

비동기식으로 병렬처리를 하려면 각각의 작업들이 독립적이여서

간섭이 없어야 한다는 조건이 있습니다.

Executor를 활용하여 task로 작업들을 나누어서 처리하는 쓰레드의 예제는 다음과 같습니다.

```java
package UserOrDaemonExecutor;

import static java.lang.Math.abs;

import java.util.Random;

public class GCDRunnable
        extends Random
        implements Runnable {

    final private String threadType;
    private final int MAX_ITERATIONS = 100000000;

    public GCDRunnable(String threadType) {
        this.threadType = threadType;
    }

    private int computeGCD(int number1,
                           int number2) {
        if (number2 == 0)
            return number1;
        return computeGCD(number2,
                number1 % number2);
    }

    public void run() {
        final String threadString =
                " with "
                        + threadType
                        + " thread id "
                        + Thread.currentThread();

        System.out.println("Entering run()"
                + threadString);

        try {
            // Iterate for the give number of times.
            for (int i = 0; i < MAX_ITERATIONS; ++i) {
                // Generate two random numbers (nextInt() obtained
                // from Random superclass).
                int number1 = abs(nextInt());
                int number2 = abs(nextInt());

                // Print results every 10 million iterations.
                if ((i % 10000000) == 0)
                    System.out.println("In run()"
                            + threadString
                            + " the GCD of "
                            + number1
                            + " and "
                            + number2
                            + " is "
                            + computeGCD(number1,
                            number2));
            }
        }
        finally {
            System.out.println("Leaving run() "
                    + threadString);
        }
    }
}
```

```java
package UserOrDaemonExecutor;

import java.util.concurrent.Executor;
import java.util.concurrent.Executors;
import java.util.concurrent.ThreadFactory;

public class TestExecutor {

    public static void main(String[] args) {
        System.out.println("Entering main()");

        final Boolean daemonThread = args.length > 0;

        GCDRunnable runnableCommand =
                new GCDRunnable(daemonThread ? "daemon" : "user");
        final int POOL_SIZE = 2;

        final ThreadFactory threadFactory =
                new ThreadFactory() {
                    @Override
                    public Thread newThread(Runnable r) {
                        Thread thr = new Thread(r);
                        if (daemonThread)
                            thr.setDaemon(true);
                        return thr;
                    }
                };

        final Executor executor =
                Executors.newFixedThreadPool(POOL_SIZE,
                        threadFactory);
        for (int i = 0; i < POOL_SIZE; i++)
            executor.execute(runnableCommand);

        try {
            Thread.sleep(2000);
        } catch (InterruptedException e) {
        }
        System.out.println("Leaving main()");
    }
}

```

GCDRunnable 클래스는 이전의 GCDRunnable과 동일합니다.

TestExecutor가 새로운 메인 메소드를 위한 클래스 입니다.

메인 쓰레드가 잠들고 나가지는 것은 동일 하지만

쓰레드를 생성할 때 ThreadFactory를 이용하고

Executor를 이용하여서 ThreadPool을 만들어서 쓰레드를

실행시켜준다는 점이 다릅니다.



커맨드인 파라미터가 비어있는 상태로 실행한 결과는 다음과 같습니다. 

```
Entering main()
Entering run() with user thread id Thread[Thread-0,5,main]
Entering run() with user thread id Thread[Thread-1,5,main]
In run() with user thread id Thread[Thread-1,5,main] the GCD of 632115600 and 1038445836 is 12
In run() with user thread id Thread[Thread-0,5,main] the GCD of 825482824 and 668347068 is 4
Leaving main()
In run() with user thread id Thread[Thread-0,5,main] the GCD of 1380296290 and 1502173602 is 2
In run() with user thread id Thread[Thread-1,5,main] the GCD of 1662358346 and 1847498689 is 1
In run() with user thread id Thread[Thread-1,5,main] the GCD of 740273353 and 1228302612 is 1
In run() with user thread id Thread[Thread-0,5,main] the GCD of 351532176 and 1487880375 is 3
In run() with user thread id Thread[Thread-1,5,main] the GCD of 1723019102 and 362343790 is 2
In run() with user thread id Thread[Thread-0,5,main] the GCD of 694978419 and 993020006 is 1
In run() with user thread id Thread[Thread-1,5,main] the GCD of 1442552283 and 1154753895 is 3
In run() with user thread id Thread[Thread-0,5,main] the GCD of 606153628 and 650111945 is 1
In run() with user thread id Thread[Thread-1,5,main] the GCD of 1827265844 and 528339357 is 1
In run() with user thread id Thread[Thread-0,5,main] the GCD of 1536776091 and 58711545 is 9
In run() with user thread id Thread[Thread-0,5,main] the GCD of 1775870813 and 965065987 is 1
In run() with user thread id Thread[Thread-1,5,main] the GCD of 249596906 and 1625295435 is 1
In run() with user thread id Thread[Thread-0,5,main] the GCD of 149130522 and 214593147 is 9
In run() with user thread id Thread[Thread-0,5,main] the GCD of 163884985 and 1216526338 is 1
In run() with user thread id Thread[Thread-1,5,main] the GCD of 1764651476 and 469579403 is 1
In run() with user thread id Thread[Thread-1,5,main] the GCD of 1496405822 and 1414084649 is 1
In run() with user thread id Thread[Thread-0,5,main] the GCD of 797798704 and 1337083465 is 1
In run() with user thread id Thread[Thread-1,5,main] the GCD of 1981382844 and 1430014676 is 4
Leaving run()  with user thread id Thread[Thread-0,5,main]
Leaving run()  with user thread id Thread[Thread-1,5,main]
```

다른 위의 예제처럼 커맨드인 파라미터를 넣어주고 실행시킨 결과는 다음과 같습니다.

```
Entering main()
Entering run() with daemon thread id Thread[Thread-1,5,main]
Entering run() with daemon thread id Thread[Thread-0,5,main]
In run() with daemon thread id Thread[Thread-0,5,main] the GCD of 912402164 and 1151238628 is 4
In run() with daemon thread id Thread[Thread-1,5,main] the GCD of 108116349 and 344079237 is 3
Leaving main()
```

ThreadFactory와 Executor를 이용하여도 유저 쓰레드와 데몬 쓰레드가 똑같이 움직인다는 것을

확인할 수 있었습니다.
