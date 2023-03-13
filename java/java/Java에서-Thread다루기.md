# Java에서 스레드(Thread) 다루기

스레드(Thread)란 프로세스 내에세 생성되는 흐름의 단위이다. <br>
Java에서도 이 스레드를 잘 다루는 것이 중요한데, 이를 정리해 보았다.

-----

### 스레드를 다루는 간단한 예제

두 개의 스레드를 생성하여 숫자를 출력하는 간단한 예제이다. <br>
하나의 스레드는 1부터 10까지의 숫자를 출력하고, 다른 하나의 스레드는 11부터 20까지의 숫자를 출력한다. <br>
두 스레드는 동시에 실행되며, 숫자를 출력할 때는 0.5초의 딜레이를 준다.

```java
public class ThreadExample {
    public static void main(String[] args) {
        // 첫 번째 스레드 생성
        Thread t1 = new Thread(() -> {
            for (int i = 1; i <= 10; i++) {
                System.out.println("Thread 1: " + i);
                try {
                    Thread.sleep(500); // 0.5초 대기
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        });
        
        // 두 번째 스레드 생성
        Thread t2 = new Thread(() -> {
            for (int i = 11; i <= 20; i++) {
                System.out.println("Thread 2: " + i);
                try {
                    Thread.sleep(500); // 0.5초 대기
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        });
        
        // 두 스레드 시작 (두 개의 스레드가 동시에 실행: 딜레이를 주었기에)
        t1.start();
        t2.start();
    }
}
```

+ 위 예제를 통해 알 수 있는내용

  + Java에서 Thread 클래스를 이용하여 스레드를 생성할 수 있다.
  + 스레드를 동시에 실행시키려면 Thread.sleep() 메소드를 이용하여 딜레이를 줄 수 있다.
  + 멀티스레딩의 이점
  > 멀티스레딩을 이용하면 여러 작업을 동시에 처리할 수 있다. <br>
  > (위 예제에서는 두 개의 스레드가 각각 다른 범위의 숫자를 출력하며, 동시에 실행되는 것을 확인할 수 있다.)
      
<br>

+ Tip) 동시에 실행되는 것처럼 보인다는 의미?

> Java에서 멀티스레딩을 이용하여 작업을 처리할 때, 두 개 이상의 스레드가 동시에 실행되는 것처럼 보일 수 있다. 
> 이것은 사실은 CPU 코어가 하나이기 때문에 실제로 동시에 실행되는 것은 아니지만, 각 스레드가 번갈아 가며 실행되는 것으로 보이게 되는 것이다.
>
> 예를 들어, 위에서 제시한 예제에서는 Thread.sleep() 메소드를 이용하여 각 스레드가 일정 시간 동안 대기하도록 했다. 
> 이렇게 하면 첫 번째 스레드가 Thread.sleep()을 호출하여 대기하는 동안, 두 번째 스레드가 실행되는 것으로 보이게 된다. 
> 이는 두 번째 스레드도 Thread.sleep()을 호출하여 대기하는 동안, 다시 첫 번째 스레드가 실행되는 것으로 보이게 되는 것이라 설명 할 수 있다.

## 실무에서 적용할 때?

우리가 Java로 웹 페이지를 만들거나, 혹은 REST API를 짤때, 멀티스레딩 환경을 고려한다. <br>
REST API 개발에서도 여러 클라이언트로부터 동시에 요청이 들어올 수 있기 때문에, 각 요청을 처리하기 위해 별도의 스레드를 할당하는 것이 필요하다. <br>
하지만, 보통 Spring framework(기본적으로 멀티스레딩을 지원하며, 스레드 안전성(Thread safety)을 보장)를 이용하여 개발하기에 <br>
따라서 개발자가 별도로 스레드를 구현할 필요는 없긴하다.

<b>하지만 Spring에서도 멀티스레딩으로 인한 문제가 발생할 수 있는 경우가 있다.</b><br>
데이터베이스와의 연결이나 파일 시스템 등의 외부 리소스에 대한 접근이 필요한 경우에는, 스레드 안전성을 보장하기 위해 동기화나 스레드 풀을 이용하여 작업을 처리해야 한다. <br>

### 스레드 풀

앞서 말했 듯, 스레드 풀을 이용하여 멀티스레드 환경에서의 작업 처리를 관리할 수 있다. <br>
스레드 풀은 미리 정해진 개수의 스레드를 생성하여 작업을 처리하며, 작업 처리가 끝나면 다음 작업을 처리할 수 있는 스레드를 할당한다. <br>
이를 통해 스레드 생성 및 소멸로 인한 오버헤드를 줄이고, 효율적인 작업 처리를 가능하게 한다. <br>

### 스레드 풀 예시

```java
/*
스프링에서는 스레드 풀을 구현하기 위해 java.util.concurrent 패키지에서 제공하는 ThreadPoolExecutor 클래스를 이용
ThreadPoolExecutor 클래스는 스레드 풀의 크기, 작업 큐, 스레드 생성 방식 등을 설정
*/
@Service
public class MyService {

    private ThreadPoolTaskExecutor executor;

    public MyService() {
        executor = new ThreadPoolTaskExecutor();
        executor.setCorePoolSize(5); // 스레드 풀의 크기를 5로 설정
        executor.setMaxPoolSize(10); // 스레드 풀의 최대 크기를 10으로 설정
        executor.setQueueCapacity(25); // 작업 큐의 크기를 25로 설정
        executor.initialize();
    }

    @Async // 비동기 작업 처리를 위한 어노테이션
    public void doAsyncTask() {
        // 비동기 작업 처리
    }
}
```

이런식으로 비동기 작업을 효율적으로 처리할 수 있다. 
