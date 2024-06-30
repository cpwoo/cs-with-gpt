# Java 의 volatile 키워드는 무엇이고, 언제 사용해야 하는지 설명하시오.

## 한 줄 요약: 여러 스레드가 공유할 때 그 변수의 최신 값을 읽고 쓰게 하는 데 사용되고, JVM 과 프로세서가 최적화를 위해 변수 값을 캐시하는 것을 방지한다.

## Java 메모리 모델 (Java Memory Model, JMM)
- Java 메모리 모델은 여러 스레드가 메모리에 접근하고 데이터를 읽고 쓰는 방법을 정의한다.
- Java 프로그램에서 각 스레드는 자신의 로컬 캐시에 변수를 저장할 수 있다.
- 이러한 캐시는 성능을 향상시키지만, 여러 스레드가 동일한 변수에 접근할 때 문제가 발생할 수 있다.
- `volatile` 키워드는 이런 경우에 필요한 동기화를 보장한다.

## volatile 키워드의 기능
- 가시성 (Visibility) 보장
    - `volatile` 변수의 값을 변경하면, 해당 값은 모든 스레드에게 즉시 가시적이다.
    - 즉, 한 스레드가 `volatile` 변수를 수정하면 다른 스레드는 즉시 그 변경된 값을 볼 수 있다.
- 명령 재배치 방지 (Prevents Instruction Reordering)
    - 컴파일러와 프로세서는 코드 실행 순서를 최적화하기 위해 명령을 재배치할 수 있다.
    - 하지만 `volatile` 키워드를 사용하면, 그 변수에 대한 읽기 및 쓰기 명령은 재배치되지 않는다.
    - 이는 변수의 상태가 일관성을 유지하도록 보장한다.

## volatile 키워드의 사용 예
- 플래그 변수: 다수의 스레드가 상태 플래그를 확인하고 업데이트하는 경우
- 상태 체크: 스레드가 어떤 조건을 기다리다가 특정 조건이 만족될 때 작업을 수행하는 경우
```Java
public class VolatileExample {
    private volatile boolean running = true;

    public void stop() {
        running = false;
    }

    public void run() {
        while (running) {
            // 작업 수행
        }
        System.out.println("Stopped");
    }

    public static void main(String[] args) {
        VolatileExample example = new VolatileExample();
        new Thread(example::run).start();

        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }

        example.stop();
    }
}
```

## volatile 키워드를 사용하지 말아야 할 때
- 복합 연산: `volatile` 변수에 대한 복합 연산(ex. `i++`)은 원자적이지 않다. 이런 경우 `synchronized` 블록이나 `java.util.concurrent.atomic` 패키지의 원자적 클래스(ex. `AtomicInteger`)를 사용해야 한다.
- 심각한 성능 문제: `volatile` 변수는 성능에 영향을 줄 수 있으므로, 필요하지 않은 경우에는 사용을 피하는 것이 좋다.
