# Java 의 Synchronized 키워드는 무엇이며, 어디에 사용되며, 어떻게 동작하는지 설명하시오.

## 한 줄 요약: 스레드 간 동기화를 위해 사용되는 키워드다. 경쟁 상태를 방지할 수 있다.

## synchronized 키워드의 사용
- 메소드에 사용: `synchronized` 키워드를 메소드 선언부에 붙이면, 해당 메소드는 한 번에 하나의 스레드만 실행할 수 있다.
```Java
public synchronized void synchronizedMethod() {
    // 이 메서드의 내용은 한 번에 하나의 스레드만 접근 가능합니다.
}
```
- 블록에 사용: `synchronized` 키워드를 특정 블록에 적용할 수 있다. 이 경우, 특정 객체를 잠금(lock)으로 사용하여 해당 블록이 한 번에 하나의 스레드만 실행되도록 한다.
```Java
public void method() {
    synchronized(this) {
        // 이 블록의 내용은 한 번에 하나의 스레드만 접근 가능합니다.
    }
}
```

## synchronized 의 동작 방식
- 모니터 락
    - `synchronized` 키워드는 모니터 락(monitor lock)을 사용한다.
    - 자바 객체는 각자 모니터 락을 가지고 있으며, `synchronized` 메소드나 블록에 진입하려는 스레드는 먼저 해당 객체의 모니터 락을 획득해야 한다.
- 락 획득과 해제
    - 스레드가 `synchronized` 메소드나 블록에 진입할 때 객체의 모니터 락을 획득한다.
    - 메소드나 블록을 벗어나면 모니터 락을 해제한다.
    - 다른 스레드는 모니터 락이 해제될 때까지 대기한다.

## synchronized 의 사용 예시
```Java
class Counter {
    private int count = 0;

    public synchronized void increment() {
        count++;
    }

    public synchronized int getCount() {
        return count;
    }
}

public class Main {
    public static void main(String[] args) {
        Counter counter = new Counter();

        // 여러 스레드가 동시에 counter 객체의 메서드를 호출할 경우
        Runnable task = () -> {
            for (int i = 0; i < 1000; i++) {
                counter.increment();
            }
        };

        Thread thread1 = new Thread(task);
        Thread thread2 = new Thread(task);

        thread1.start();
        thread2.start();

        try {
            thread1.join();
            thread2.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        // count 값은 항상 2000이 되어야 합니다.
        System.out.println(counter.getCount()); // 출력: 2000
    }
}
```

## 주의사항
- 성능 문제: 과도한 동기화는 성능 저하를 초래할 수 있다. 불필요하게 큰 범위의 코드에 `synchronized` 를 사용하면 병목 현상이 발생할 수 있다.
- 데드락: 여러 개의 락을 사용하는 경우 잘못된 락 순서로 인해 데드락(deadlock)이 발생할 수 있다.
- 대안: 경우에 따라 `java.util.concurrent` 패키지의 `ReentrantLock` 이나 고수준 동시성 유틸리티를 사용하는 것이 더 나은 선택이 될 수 있다.
