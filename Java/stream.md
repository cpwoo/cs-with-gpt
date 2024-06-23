# Java 의 Stream API 의 목적과 주요 기능에 대해 설명하시오.

## 한 줄 요약: 컬렉션 및 배열의 처리와 변환을 효율적이고 간결하게 수행할 수 있다.

## 목적
- 데이터 처리의 간결성
    - 컬렉션 데이터를 처리하는 과정을 간단하고 읽기 쉽게 만든다.
    - for-loop 등의 반복 구조를 대체하여 더 직관적인 코드 작성이 가능하다.
- 함수형 프로그래밍 지원
    - 람다 표현식과 함께 사용하여 함수형 프로그래밍 패러다임을 지원한다.
    - 함수형 스타일의 코딩을 통해 부작용이 없는(pure) 함수 작성과 불변성(immutability) 유지가 용이하다.
- 병렬 처리의 용이성
    - 데이터 병렬 처리를 쉽게 구현할 수 있다.
    - Stream API 는 내부적으로 병렬 처리를 지원하여 멀티코어 프로세서의 성능을 활용할 수 있다.
- 성능 최적화
    - 게으른 연산(lazy evaluation)을 통해 필요한 시점에만 연산을 수행하여 성능을 최적화한다.
    - 필요 없는 연산을 줄임으로써 불필요한 자원 낭비를 막는다.

## 주요 기능
- 스트림 생성
    - 컬렉션: `Collection.stream()` 또는 `Collection.parallelStream()`
    - 배열: `Arrays.stream(T[] array)`
    - 정적 메소드: `Stream.of(T... values)`, `Stream.iterate(T send, UnaryOperator<T> f)`, `Stream.generate(Supplier<T> s)`
- 중간 연산 (Intermediate Operations): 중간 연산은 스트림을 변환하며, 결과는 또 다른 스트림이다. 중간 연산은 지연 평가된다.
    - `filter(Predicate<? super T> predicate)`: 조건에 맞는 요소만을 포함하는 스트림을 반환한다.
    - `map(Function<? super T, ? extends R> mapper)`: 각 요소를 주어진 함수에 적용한 결과를 포함하는 스트림을 반환한다.
    - `flatMap(Function<? super T, ? extends Stream<? extends R>> mapper)`: 스트림의 각 요소를 다른 스트림으로 매핑한 후, 모든 스트림을 하나의 스트림으로 병합한다.
    - `distinct()`: 중복된 요소를 제거한 스트림을 반환한다.
    - `sorted()` 및 `sorted(Comparator<? super T> comparator)`: 스트림의 요소를 정렬한다.
    - `peek(Consumer<? super T> action)`: 스트림의 각 요소를 소모하며, 요소를 변경하지 않고, 주어진 작업을 수행한다.
- 종료 연산 (Terminal Operations): 종료 연산은 스트림을 소모하며, 연산 결과를 생성한다.
    - `forEach(Consumer<? super T> action)`: 각 요소에 주어진 동작을 수행한다.
    - `toArray()`: 스트림의 요소를 배열로 반환한다.
    - `reduce(BinaryOperator<T> accumulator)` 및 `reduce(T identity, BinaryOperator<T> accumulator)`: 모든 요소를 결합하여 단일 결과를 생성한다.
    - `collect(Collector<? super T, A, R> collector)`: 스트림의 요소를 지정된 컬렉터에 의해 수집한다. ex. `Collectors.toList()`, `Collectors.toSet()`, `Collectors.toMap()`
    - `min(Comparator<? super T> comparator)` 및 `max(Comparator<? super T> comparator)`: 최소 또는 최대 요소를 찾는다.
    - `count()`: 스트림의 요소 개수를 반환한다.
    - `anyMatch(Predicate<? super T> predicate)`, `allMatch(Predicate<? super T> predicate)`, `noneMatch(Predicate<? super T> predicate)`: 스트림의 요소가 주어진 조건과 일치하는지 검사한다.

## 예제 코드
```Java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class StreamExample {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "David", "Eve");

        // 중간 연산과 종료 연산 예제
        List<String> filteredNames = names.stream()
                                          .filter(name -> name.startsWith("A") || name.startsWith("E"))
                                          .map(String::toUpperCase)
                                          .sorted()
                                          .collect(Collectors.toList());

        System.out.println(filteredNames); // [ALICE, EVE]

        // 병렬 처리 예제
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
        int sumOfEvenNumbers = numbers.parallelStream()
                                      .filter(n -> n % 2 == 0)
                                      .mapToInt(Integer::intValue)
                                      .sum();

        System.out.println(sumOfEvenNumbers); // 30
    }
}
```
