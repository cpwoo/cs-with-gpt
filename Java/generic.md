# 제네릭이란 무엇이며, Java 에서 제네릭을 사용하는 이유에 대해 설명하시오.

## 한 줄 요약: 제네릭은 타입을 파라미터화하는 기법이다. 데이터 타입을 나중에 지정할 수 있다. 코드의 재사용성을 높이고, 타입 안정성을 강화하며, 코드의 가독성을 향상시킬 수 있다.

## 제네릭의 정의와 개념
- 제네릭의 기본 개념
    - 제네릭은 데이터 타입을 일반화(generalize)하여 여러 타입을 하나의 클래스나 메소드에서 처리할 수 있도록 한다.
    - 예를 들어, 동일한 기능을 가진 클래스가 Integer 타입의 데이터를 처리하도록 만들었다면, 다른 데이터 타입(Double, String 등)을 처리하는 새로운 클래스를 또 만들어야 한다.
    - 제네릭을 사용하면 이러한 반복 작업을 줄일 수 있다.
- 제네릭의 타입 파라미터
    - 제네릭은 타입 파라미터를 사용하여 다양한 데이터 타입을 하나의 코드 구조로 처리한다.
    - 타입 파라미터는 보통 대문자 한 글자로 표현하며, 가장 일반적으로 사용되는 타입 파라미터는 다음과 같다.
        - `E`: 요소(Element)
        - `K`: 키(Key)
        - `N`: 숫자(Number)
        - `T`: 타입(Type)
        - `V`: 값(Value)
    ```Java
    public class Box<T> {
        private T item;

        public void setItem(T item) {
            this.item = item;
        }

        public T getItem() {
            return item;
        }
    }
    ```

## Java 에서 제네릭을 사용하는 이유
- 코드의 재사용성 증가
    - 제네릭을 사용하면 동일한 기능을 가진 클래스를 여러 타입에 대해 각각 작성할 필요가 없다.
    - 하나의 제네릭 클래스나 메소드로 여러 타입을 처리할 수 있기 때문에 코드의 재사용성이 크게 증가한다.
    ``` Java
    Box<Integer> integerBox = new Box<>();
    Box<String> stringBox = new Box<>();
    ```
- 타입 안정성 강화
    - 제네릭을 사용하면 컴파일 시점에 타입 검사가 이루어지므로, 런타임에 발생할 수 있는 타입 오류를 사전에 방지할 수 있다.
    - 제네릭을 사용하지 않으면 객체를 삽입하고 꺼낼 때 명시적 타입 캐스팅이 필요하고, 이는 타입 오류를 발생시킬 수 있다.
    ```Java
    Box<String> stringBox = new Box<>();
    stringBox.setItem("Hello");

    // 컴파일 타임에 오류를 잡아줌
    // stringBox.setItem(123); // 오류: 타입 불일치
    ```
- 코드의 가독성 향상
    - 제네릭을 사용하면 코드에서 사용되는 데이터 타입이 명확하게 드러나므로, 코드의 가독성이 향상된다.
    - 이를 통해, 코드의 유지보수가 용이해지고, 협업 시 다른 개발자가 코드를 이해하기 쉬워진다.
    ```Java
    public class Pair<K, V> {
        private K key;
        private V value;

        public Pair(K key, V value) {
            this.key = key;
            this.value = value;
        }

        public K getKey() {
            return key;
        }

        public V getValue() {
            return value;
        }
    }
    ```
