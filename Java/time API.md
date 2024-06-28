# Java 의 새로운 시간 API(java.time.*)와 기존의 java.util.Date 와의 차이점에 대해 설명하시오.

## 한 줄 요약: 더 직관적이고 사용하기 쉬우며, 많은 문제를 해결하고 다양한 기능을 제공한다.

## 설계 철학 및 사용성
- 불변성
    - `java.time`: 이 API 의 대부분의 클래스는 불변(immutable)이다. 즉, 한 번 생성한 객체의 상태를 변경할 수 없다. 이는 멀티스레딩 환경에서 안전하게 사용할 수 있도록 한다.
    - `java.util.Date` 및 `Calendar`: 이 클래스들은 가변(mutable)이다. 객체의 상태를 변경할 수 있어, 멀티스레드 환경에서 동기화 문제를 일으킬 수 있다.
- 클래스 이름 및 구조
    - `java.time`: 명확하고 직관적인 이름을 가진 클래스들로 구성된다. 예를 들어, `LocalDate`, `LocalTime`, `LocalDateTime`, `ZonedDateTime` 등은 각각 날짜, 시간, 날짜와 시간, 시간대가 포함된 날짜와 시간을 표현한다.
    - `java.util.Date` 및 `Calendar`: 클래스 이름이 덜 직관적이고, 날짜와 시간을 모두 포함하는 단일 클래스로 설계되어 사용이 복잡해진다.

## 시간대 및 오프셋
- 시간대 처리
    - `java.time`: `ZoneId`, `ZoneOffset`, `ZonedDateTime` 등 다양한 클래스가 시간대를 효과적으로 처리할 수 있도록 한다.
    - `java.util.Date` 및 `Calendar`: 시간대를 직접 처리하는 데 제한적이다. `Calendar` 는 시간대 정보를 포함하지만, 사용이 복잡하고 실수가 발생하기 쉽다.
- 오프셋 관리
    - `java.time`: `OffsetDateTime` 과 같은 클래스가 UTC 오프셋을 명시적으로 관리할 수 있다.
    - `java.util.Date`: UTC 오프셋을 관리하는 명확한 방법이 없다.

## 날짜와 시간 조작
- 조작 메소드
    - `java.time`: 날짜와 시간을 조작하는 다양한 메소드를 제공한다. 예를 들어, `plusDays`, `minusMonths`, `withYear` 등의 메소드로 간편하게 날짜와 시간을 조작할 수 있다.
    - `java.util.Date` 및 `Calendar`: 날짜와 시간을 조작하는 메소드가 제한적이고, 사용이 불편하다. `Calendar` 는 메소드 호출 후 다시 설정을 해야 하는 불편함이 있다.
- 포맷팅 및 파싱
    - `java.time`: `DateTimeFormatter` 클래스를 사용해 날짜와 시간을 간편하게 포맷팅하고 파싱할 수 있다.
    - `java.util.Date`: `SimpleDateFormat` 클래스를 사용해야 하며, 쓰레드 안전하지 않아 멀티스레딩 환경에서 사용이 위험하다.

## 인간 친화적
- API 의 직관성
    - `java.time`: API 가 직관적이고, 메소드와 클래스의 이름이 명확하게 설계되어 있어 코드의 가독성이 높다.
    - `java.util.Date` 및 `Calendar`: 가독성이 낮고, 명확하지 않은 메소드와 상수들이 많이 포함되어 있다.

## 기능성
- 추가 기능
    - `java.time`: 기간(`Period`), 지속 시간(`Duration`), 타임라인 포인트(`Instant`) 등 다양한 시간 개념을 지원한다.
    - `java.util.Date` 및 `Calendar`: 이러한 추가적인 시간 개념을 직접 지원하지 않으며, 사용자 정의 구현이 필요하다.
 