# 람다표현식이란 무엇이고 어떤 상황에서 사용하면 좋을지 설명하시오.

## 한 줄 요약: 익명 함수를 간결하게 표현한 것으로, 코드의 가독성과 병렬 처리를 보다 쉽게 할 수 있다.

## 람다 표현식의 문법
```Java
(parameters) -> expression
(parameters) -> { statements; }
```
- (parameters): 인자를 정의하는 부분이다. 매개변수가 하나인 경우 괄호를 생략할 수 있다.
- ->: 람다 연산자(또는 화살표 연산자)로, 매개변수와 함수 본문을 구분한다.
- expression or {statements;}: 함수의 본문으로, 단일 표현식의 경우 중괄호를 생략할 수 있으며, 여러 문장일 경우 중괄호로 묶어야 한다.

## 예시
- 기본 형태의 람다 표현식
```Java
(int x, int y) -> x + y
```
- 매개변수가 하나일 때 괄호 생략
```Java
a -> a * 2
```
- 문장이 여러 개일 경우 중괄호 사용
```Java
(int x) -> {
    System.out.println(x);
    return x * 2;
}
```

## 사용 상황
- 컬렉션 프레임워크와 스트림 API
    - 람다 표현식은 주로 컬렉션 프레임워크와 스트림 API 에서 많이 사용된다.
    - 자바 8에서 도입된 스트림 API 는 컬렉션 데이터를 처리하는 데 있어서 람다 표현식과 매우 잘 맞는다.
    ```Java
    List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
    List<Integer> doubled = numbers.stream().map(n -> n * 2).collect(Collectors.toList());
    ```
- 함수형 인터페이스와의 활용
    - 자바에서 람다 표현식을 사용하려면 함수형 인터페이스(Functional Interface)가 필요하다.
    - 함수형 인터페이스는 단 하나의 추상 메소드만을 가지는 인터페이스다.
    - 여러 가지 기본 함수형 인터페이스가 제공된다.(ex. `Predicate`, `Function`, `Consumer`, `Supplier`)
    ```Java
    Runnable runnable = () -> System.out.println("Hello, Lambda!");
    new Thread(runnable).start();
    ```
- 이벤트 처리
    - 이벤트 기반 프로그래밍에서 자주 사용되는 익명 클래스의 대체로 람다 표현식을 사용할 수 있다.
    ```Java
    button.setOnAction(event -> System.out.println("Button clicked!"));
    ```

## 장점
- 간결함: 코드가 짧고 명확해진다. 익명 클래스를 사용할 때의 장황함을 제거한다.
- 가독성: 코드의 의도가 명확하게 드러나므로 가독성이 향상된다.
- 효율성: 병렬 처리 및 컬렉션 처리에서 효율적인 코드를 작성할 수 있다.

## 단점 및 주의사항
- 디버깅의 어려움: 익명 함수이기 때문에 디버깅 시 호출 스택을 추적하기 어려울 수 있다.
- 익명 함수의 특성: 코드의 복잡도가 증가하면 람다 표현식이 오히려 이해하기 어려워질 수 있다. 이 경우 명시적인 메소드 참조나 익명 클래스가 더 나을 수 있다.
- 함수형 인터페이스 요구: 람다 표현식은 함수형 인터페이스가 필요하므로 기존의 많은 코드 구조를 변경해야 할 수도 있다.
