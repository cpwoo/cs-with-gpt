# ArrayList 는 어떻게 동작하며, 어떤 상황에서 사용하면 좋을지 설명하시오.

## 한 줄 요약: ArrayList 는 동적 배열을 구현한 클래스다.

## 동작 원리
- 내부 구조
    - 내부적으로 배열을 사용하여 데이터를 저장한다.
    - 초기에는 일정 크기의 배열이 생성되고, 데이터가 추가되면서 배열의 크기가 자동으로 조정된다.
- 동적 크기 조정
    - 추가(add)
        - 배열이 가득 차면, `ArrayList` 는 현재 크기의 약 1.5배 정도 되는 새로운 배열을 생성하고, 기존 배열의 데이터를 새로운 배열로 복사한다.
        - 이는 자주 발생하지 않지만, 발생할 때 성능의 영향을 줄 수 있다.
    - 제거(remove)
        - 요소를 제거하면, 뒤에 있는 요소들이 앞으로 이동하여 빈 공간을 메운다.
        - 이는 최악의 경우 O(n) 의 시간이 걸린다.
- 인덱스 기반 접근
    - `ArrayList` 는 인덱스를 기반으로 빠른 읽기와 쓰기 (O(1)) 가 가능하다.
    - 이는 내부적으로 배열을 사용하기 때문에 가능한 일이다.
- 탐색
    - 특정 요소를 찾는 데 O(n) 의 시간이 걸린다.
    - 이는 `ArrayList` 가 순차적으로 요소를 검색하기 때문이다.

## 사용 상황
- 데이터의 빈번한 추가 및 삭제가 없는 경우
    - `ArrayList` 는 배열 기반이기 때문에, 데이터를 추가하거나 삭제할 때 성능이 저하될 수 있다.
    - 그러나 데이터의 추가 및 삭제가 드물고, 주로 읽기 및 쓰기가 주로 이루어지는 상황에서는 매우 효율적이다.
- 순차적인 접근이 필요한 경우
    - `ArrayList` 는 인덱스를 통한 순차적 접근이 매우 빠르기 때문에, 인덱스를 기반으로 한 순차적 접근이 빈번한 경우 적합하다.
- 크기가 가변적인 컬렉션이 필요한 경우
    - 프로그램 실행 중에 데이터의 크기가 동적으로 변하는 경우, `ArrayList` 는 유연하게 크기를 조절할 수 있다.
    - 이는 고정된 크기의 배열보다 더 나은 선택이 될 수 있다.

## 예제 코드
다음은 `ArrayList` 의 기본적인 사용 예제다.
```Java
import java.util.ArrayList;

public class ArrayListExample {
    public static void main(String[] args) {
        // ArrayList 선언 및 생성
        ArrayList<String> fruits = new ArrayList<>();

        // 요소 추가
        fruits.add("Apple");
        fruits.add("Banana");
        fruits.add("Cherry");

        // 요소 접근
        System.out.println("First fruit: " + fruits.get(0));  // Output: First fruit: Apple

        // 요소 삭제
        fruits.remove("Banana");

        // 크기 확인
        System.out.println("Size: " + fruits.size());  // Output: Size: 2

        // 요소 순회
        for (String fruit : fruits) {
            System.out.println(fruit);
        }
        // Output:
        // Apple
        // Cherry
    }
}
```

## 주의사항 및 최적화
- 초기 용량 설정
    - 초기 용량을 저장하여, 예상되는 데이터의 크기만큼 배열을 미리 할당할 수 있다.
    - 이를 통해 배열 확장을 최소화하여 성능을 향상시킬 수 있다.
    ```Java
    ArrayList<String> largeList = new ArrayList<>(1000);
    ```
- 동기화
    - `ArrayList` 는 비동기화된 컬렉션이다.
    - 멀티스레드 환경에서는 `Collections.synchronizedList` 메서드를 사용하여 동기화된 리스트로 감쌀 필요가 있다.
    ```Java
    List<String> synchronizedList = Collections.synchronizedList(new ArrayList<>());
    ```
