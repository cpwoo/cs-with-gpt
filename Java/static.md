# static 의 의미와 잘못 사용될 경우 발생할 수 있는 문제에 대해 설명하시오.

## 한 줄 요약: static 키워드는 클래스 레벨의 멤버를 정의할 때 매우 유용하지만, 잘못 사용될 경우 여러 문제가 발생하며, 특히 스레드 안정성, 메모리 관리, 초기화 순서 문제를 주의해야 한다.

## static 키워드의 의미
- Static 변수 (클래스 변수)
    - `static` 으로 선언된 변수는 클래스의 모든 인스턴스가 공유하는 변수다.
    - 클래스 로딩 시 초기화되며, 모든 인스턴스가 동일한 값을 참조한다.
    ``` Java
    public class Example {
        public static int counter = 0;
    }
    ```
- Static 메소드 (클래스 메소드)
    - `static` 으로 선언된 메소드는 클래스의 인스턴스에 의존하지 않는다.
    - 클래스 이름을 통해 직접 호출할 수 있다.
    - 인스턴스 변수나 메소드에 접근할 수 없다(인스턴스가 없기 때문)
    ```Java
    public class Example {
        public static void printMessage() {
            System.out.println("Hello, World!");
        }
    }
    ```
- Static 블록
    - `static` 블록은 클래스가 메모리에 로드될 때 한 번 실행된다.
    - 주로 `static` 변수를 초기화하는 데 사용된다.
    ```Java
    public class Example {
        public static int counter;

        static {
            counter = 100;
        }
    }
    ```

## static 의 잘못된 사용으로 인한 문제
- 공유 자원 문제 (Thread Safety)
    - `static` 변수는 모든 인스턴스가 공유하기 때문에 여러 스레드가 동시에 접근하면 데이터 불일치나 경합 상태가 발생할 수 있다.
    ```Java
    public class Counter {
        public static int count = 0;

        public static void increment() {
            count++;
        }
    }
    ```
    - 여러 스레드가 `Counter.increment()` 를 호출하면, `count` 의 값이 정확하지 않을 수 있다. 이를 방지하려면 동기화(synchronization)를 사용해야 한다.
- 메모리 누수 (Memory Leak)
    - `static` 변수는 프로그램이 종료될 때까지 메모리에 유지된다. 따라서 불필요하게 많은 메모리를 차지할 수 있다.
    ``` Java
    public class Example {
        public static List<String> list = new ArrayList<>();
    }
    ```
    - 이 경우, `list` 는 프로그램이 종료될 때까지 메모리에 유지된다.
- 초기화 순서 문제 (Initialization Order)
    - 여러 클래스 간에 `static` 변수나 메소드에 의존성이 있을 경우, 초기화 순서에 따라 의도치 않은 동작이 발생할 수 있다.
    ``` Java
    public class A {
        public static int value = B.getValue();
    }

    public class B {
        public static int getValue() {
            return 10;
        }
    }
    ```
    - 이 경우, 클래스 A 와 B 의 초기화 순서에 따라 `A.value` 의 값이 다르게 초기화 될 수 있다.
- 과도한 사용 (Overuse)
    - `static` 멤버를 과도하게 사용하면 객체 지향 프로그래밍의 장점을 잃게 된다. 재사용성, 유연성, 다형성 등이 감소한다.
    ``` Java
    public class MathUtils {
        public static int add(int a, int b) {
            return a + b;
        }
    }
    ```
    - 이러한 유틸리티 클래스는 유용하지만, 모든 기능을 `static` 메소드로 정의하면 객체 지향 설계의 이점을 활용하지 못하게 된다.
