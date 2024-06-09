# Java 의 어노테이션이란 무엇이며, 주로 어떤 상황에서 작용되는지 설명하시오.

## 한 줄 요약: 어노테이션은 메타데이터를 코드에 추가할 수 있는 방법을 제공한다.

## 적용 상황
- 어노테이션은 클래스, 메소드, 변수, 매개변수, 패키지 등에 작용할 수 있다.
- 컴파일러 지시자
    - 컴파일러에게 코드 작성 의도를 전달하거나, 경고를 무시하게 하거나, 특정 오류를 체크하게 할 수 있다.
    - ex: `@Override` (메소드가 슈퍼클래스의 메소드를 오버라이드하고 있음을 명시)
- 런타임 처리
    - 런타임 시 특정 동작을 수행하도록 코드에 힌트를 제공할 수 있다.
    - 리플렉션(reflection)을 사용하여 런타임에 어노테이션을 읽고 처리할 수 있다.
    - ex: `@Deprecated` (더 이상 사용되지 않는 코드를 표시하고 경고를 발생)
- 코드 문서화
    - 자동으로 문서를 생성할 때, 주석과 함께 어노테이션을 사용하여 문서에 추가 정보를 제공할 수 있다.
    - ex: `@author`, `@version`
- 프레임워크 및 라이브러리 지원
    - 많은 프레임워크(ex: 스프링, 하이버네이트 등)에서는 어노테이션을 사용하여 설정을 단순화하고, 특정 동작을 트리거한다.
    - ex: 스프링의 `@Autowired` (의존성 주입을 자동으로 처리)

## 주요 어노테이션 예시
- @Override: 메소드가 슈퍼클래스의 메소드를 오버라이드하고 있음을 나타낸다.
```Java
@Override
public void someMethod() {
    // Implementation here
}
```
- @Deprecated: 더 이상 사용되지 않는 요소임을 나타내며, 사용 시 경고를 발생시킨다.
```Java
@Deprecated
public void oldMethod() {
    // Old implementation
}
```
- @SuppressWarnings: 특정 컴파일러 경고를 무시하도록 한다.
``` Java
@SuppressWarnings("unchecked")
public void someMethod() {
    // Implementation here
}
```
- @Retention: 어노테이션의 유지 정책을 정의한다. (소스, 클래스, 런타임)
```Java
@Retention(RetentionPolicy.RUNTIME)
public @interface MyAnnotation {
    // Elements here
}
```
- @Target: 어노테이션이 적용될 수 있는 요소(클래스, 메소드, 필드 등)을 정의한다.
``` Java
@Target(ElementType.METHOD)
public @interface MyAnnotation {
    // Elements here
}
```

## 상황별 사용 예
- 테스트 프레임워크: Junit의 `@Test` 어노테이션은 해당 메소드가 테스트 메소드임을 나타낸다.
``` Java
@Test
public void testMethod() {
    // Test code here
}
```
- 의존성 주입: 스프링의 `@Autowired` 는 스프링 컨테이너가 필요한 의존성을 자동으로 주입하도록 한다.
```Java
@Autowired
private SomeService someService;
```
- ORM 매핑: 하이버네이트의 `@Entity` 와 `@Table` 어노테이션은 클래스가 데이터베이스의 테이블과 매핑됨을 나타낸다.
```Java
@Entity
@Table(name = "users")
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(name = "username")
    private String username;

    // Getters and setters
}
```
