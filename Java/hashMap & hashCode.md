# Java 에서 HashMap 과 hashCode 메소드의 관련성에 대해 설명하시오.

## 한 줄 요약: hashCode 메소드는 해시 테이블에서 해싱을 구현하는 데 핵심적인 역할을 한다.

## HashMap 의 구조와 작동 방식
- `HashMap` 은 내부적으로 배열과 연결 리스트 또는 트리를 사용하여 데이터를 저장한다.
- 해시 테이블에서 각 요소는 "버킷(bucket)" 에 저장되는데, 버킷은 배열의 한 칸에 대응한다.
- 각 키-값 쌍은 버킷에 저장되며, 해시 충돌이 발생하면 해당 버킷에 여러 요소가 연결 리스트나 트리 형태로 저장된다.

## 해싱과 hashCode 메소드
- 해싱은 객체를 고유의 정수 값(해시 코드)으로 변환하는 과정을 의미한다.
- Java 에서 모든 객체는 `Object` 클래스로부터 상속받은 `hashCode` 메소드를 가지고 있다.
- 이 메소드는 객체의 메모리 주소를 기반으로 해시 코드를 생성한다.
- `HashMap` 은 다음과 같은 단계로 해서 해시 코드를 사용한다.
    - 해시 코드 계산: 키 객체의 `hashCode` 메소드를 호출하여 해시 코드를 계산한다.
    - 인덱스 계산: 해시 코드를 배열의 인덱스로 변환한다. 일반적으로 이는 `hashCode % 배열의 크기`와 같은 연산을 통해 수행된다.
    - 데이터 저장: 계산된 인덱스에 해당하는 버킷에 데이터를 저장한다.

## 해시 충돌 처리
- 같은 인덱스로 해시 코딩된 서로 다른 키-값 쌍들이 있을 수 있다.
- 이를 해시 충돌이라고 하며, `HashMap` 은 이를 해결하기 위해 두 가지 방법을 사용한다.
    - 연결 리스트: 초기 버전에서는 충돌이 발생한 버킷에 연결 리스트로 데이터를 저장한다.
    - 트리: Java 8 이후로는 충돌이 많이 발생하면 연결 리스트 대신 트리를 사용하여 성능을 향상시킨다.

## equals 메소드와의 관계
- `HashMap` 에서 키 객체의 동일성을 비교할 때는 `equals` 메소드를 사용한다.
- 따라서 `hashCode` 와 `equals` 메소드는 다음과 같은 규칙을 따라야 한다.
    - 두 객체가 `equals` 메소드로 같다고 판단되면, 두 객체의 `hashCode` 값도 같아야 한다.
    - 두 객체가 `equals` 메소드로 다르다고 판단되는 경우, `hashCode` 값이 다를 필요는 없지만, 해시 충돌을 최소화하기 위해서는 가능한 다르게 구현하는 것이 좋다.

## 예제 코드
```Java
import java.util.HashMap;

public class HashMapExample {
    public static void main(String[] args) {
        HashMap<Key, String> map = new HashMap<>();

        Key key1 = new Key(1);
        Key key2 = new Key(2);
        Key key3 = new Key(1); // key1과 동일한 해시 코드와 equals 결과를 가질 것

        map.put(key1, "Value1");
        map.put(key2, "Value2");
        map.put(key3, "Value3");

        System.out.println(map.get(key1)); // Value3 출력
        System.out.println(map.get(key2)); // Value2 출력
    }
}

class Key {
    private int id;

    public Key(int id) {
        this.id = id;
    }

    @Override
    public int hashCode() {
        return id; // 단순화된 해시 코드 계산
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (obj == null || getClass() != obj.getClass()) return false;
        Key key = (Key) obj;
        return id == key.id;
    }
}
```
