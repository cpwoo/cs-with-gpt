# Java 에서 스레드란 무엇이고 왜 사용하는지 설명하시오.

## 한 줄 요약: 스레드를 사용하면 병렬 처리를 통해 프로그램의 효율성과 성능을 높일 수 있다.

## 스레드의 기본 개념
- 스레드는 운영체제의 프로세스 내에서 독립적으로 실행되는 경량 실행 단위다.
- 하나의 프로세스는 여러 개의 스레드를 포함할 수 있으며, 이들 스레드는 프로세스의 자원을 공유한다.
- Java 에서는 `Thread` 클래스와 `Runnable` 인터페이스를 통해 스레드를 생성하고 관리할 수 있다.

## 스레드를 사용하는 이유
- 병렬 처리 및 성능 향상
    - 멀티스레딩을 사용하면 여러 작업을 동시에 수행할 수 있다.
    - 예를 들어, 웹 서버는 각 클라이언트의 요청을 별도의 스레드로 처리함으로써 동시에 여러 요청을 처리할 수 있다.
- 응답성 개선
    - 사용자 인터페이스(UI) 애플리케이션에서는 긴 작업을 별도의 스레드에서 실행함으로써 주 스레드가 응답성을 유지하도록 할 수 있다.
    - 예를 들어, 파일을 읽거나 네트워크 요청을 처리하는 동안 UI 가 멈추지 않게 할 수 있다.
- 자원 효율성
    - 스레드는 프로세스 내의 자원을 공유하기 때문에, 여러 프로세스를 사용하는 것보다 메모리와 같은 시스템 자원을 더 효율적으로 사용할 수 있다.

## Java 에서 스레드 생성 및 실행
- `Thread` 클래스 상속
    - `Thread` 클래스를 상속받아 새로운 클래스를 정의하고, `run` 메소드를 오버라이드한다.
    - `run` 메소드 안에 스레드가 실행할 코드를 작성한다.
    ```Java
    class MyThread extends Thread {
        public void run() {
            // 스레드가 수행할 작업
            for (int i = 0; i < 10; i++) {
                System.out.println("Thread: " + i);
            }
        }
    }

    public class Main {
        public static void main(String[] args) {
            MyThread thread = new MyThread();
            thread.start();  // 스레드 시작
        }
    }
    ```
- `Runnable` 인터페이스 구현
    - `Runnable` 인터페이스를 구현한 클래스를 정의하고, `run` 메소드를 오버라이드한다.
    - 그런 다음, `Thread` 객체를 생성할 때 `Runnable` 객체를 전달한다.
    ```Java
    class MyRunnable implements Runnable {
        public void run() {
            // 스레드가 수행할 작업
            for (int i = 0; i < 10; i++) {
                System.out.println("Runnable: " + i);
            }
        }
    }

    public class Main {
        public static void main(String[] args) {
            MyRunnable runnable = new MyRunnable();
            Thread thread = new Thread(runnable);
            thread.start();  // 스레드 시작
        }
    }
    ```

## 스레드의 상태
스레드는 다음과 같은 여러 상태를 가질 수 있다.
- NEW: 스레드가 생성되었지만 아직 시작되지 않은 상태
- RUNNABLE: 실행 중이거나 실행될 준비가 된 상태
- BLOCKED: 스레드가 잠금이 풀리기를 기다리는 상태
- WAITING: 스레드가 다른 스레드의 특정 작업이 완료되기를 기다리는 상태
- TIMED_WAITING: 지정된 시간 동안 기다리는 상태
- TERMINATED: 스레드가 실행을 마친 상태

## 동기화와 스레드 안전
- 멀티스레딩 환경에서는 여러 스레드가 공유 자원에 동시에 접근할 때 발생할 수 있는 경쟁 조건(race condition)을 피하기 위해 동기화가 필요하다.
- Java 에서는 `synchronized` 키워드를 사용하여 임계 구역을 설정하고, 객체의 모니터 락을 사용하여 동기화를 처리한다.
```Java
class Counter {
    private int count = 0;

    public synchronized void increment() {
        count++;
    }

    public int getCount() {
        return count;
    }
}

public class Main {
    public static void main(String[] args) {
        Counter counter = new Counter();
        Thread t1 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                counter.increment();
            }
        });
        Thread t2 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                counter.increment();
            }
        });

        t1.start();
        t2.start();
        
        try {
            t1.join();
            t2.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("Final count: " + counter.getCount());
    }
}
```

## 스레드 관리 및 고급 기능
- Executors 프레임워크: `java.util.concurrent` 패키지에 포함된 Executor 프레임워크는 스레드 풀을 사용하여 생성을 관리하고 작업을 할당하는 더 높은 수준의 API 를 제공한다.
```Java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class Main {
    public static void main(String[] args) {
        ExecutorService executor = Executors.newFixedThreadPool(2);
        executor.submit(() -> {
            for (int i = 0; i < 10; i++) {
                System.out.println("Task 1: " + i);
            }
        });
        executor.submit(() -> {
            for (int i = 0; i < 10; i++) {
                System.out.println("Task 2: " + i);
            }
        });
        executor.shutdown();
    }
}
```
- Lock 및 Condition 객체: 보다 정교한 동기화 제어를 위해 `ReentrantLock` 및 `Condition` 객체를 사용할 수 있다.
