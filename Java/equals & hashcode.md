# equals 와 hashCode 메소드를 오버라이딩하는 이유에 대해 설명하시오.

## 한 줄 요약: 주로 객체의 동등성 비교와 효율적인 해시 기반 컬렉션의 동작을 보장하기 위함

## equals 메소드
- 객체 동등성 비교
    - 기본적으로 `Object` 클래스의 `equals` 메소드는 동일한 객체 참조인지 비교한다. 즉, `==` 연산자와 동일하게 작동한다.
    - 하지만, 우리가 원하는 것은 객체의 내용이 같은지를 비교하는 것이다.
    - 예를 들어, 두 `Person` 객체가 이름과 나이가 같다면 같은 객체로 간주하고 싶을 때, `equals` 메서드를 오버라이딩하여 아래와 같이 구현할 수 있다.
    ``` Java
    public class Person {
        private String name;
        private int age;

        // constructor, getters, and setters

        @Override
        public boolean equals(Object o) {
            if (this == o) return true;
            if (o == null || getClass() != o.getClass()) return false;
            Person person = (Person) o;
            return age == person.age && Objects.equals(name, person.name);
        }
    }
    ```
- 일관된 동작 보장
    - `equals` 메소드를 오버라이딩할 때, `hashCode` 메소드도 일관성 있게 오버라이딩해야 한다.
    - 두 객체가 `equals` 메소드를 통해 같다면, 같은 `hashCode` 값을 가져야 한다.

## hashCode 메소드
- 해시 기반 컬렉션에서의 사용
    - `hashCode` 메소드는 객체를 빠르게 찾기 위해 해시 코드를 생성한다.
    - 해시 기반 컬렉션(`HashMap`, `HashSet` 등)은 이 해시 코드를 사용하여 객체를 저장하고 검색한다.
    - 예를 들어, `Person` 객체를 `HashSet` 에 저장하고 싶다면, `hashCode` 메소드를 적절히 오버라이딩해야 한다.
    ```Java
    public class Person {
        private String name;
        private int age;

        // constructor, getters, and setters

        @Override
        public int hashCode() {
            return Objects.hash(name, age);
        }

        @Override
        public boolean equals(Object o) {
            if (this == o) return true;
            if (o == null || getClass() != o.getClass()) return false;
            Person person = (Person) o;
            return age == person.age && Objects.equals(name, person.name);
        }
    }
    ```
- 해시 충돌 최소화
    - 효율적인 해시 기반 컬렉션을 위해 해시 충돌을 최소화하는 것이 중요하다.
    - 잘 설계된 `hashCode` 메소드는 다양한 객체에 대해 가능한 고르게 해시 값을 분포시켜야 한다.

## equals 와 hashCode 를 오버라이딩할 때의 주요 원칙
- 일관성
    - 객체의 `equals` 메서드 결과가 변하지 않으면, `hashCode` 메소드의 결과도 변하지 않아야 한다.
- equals 계약
    - 반사성: `x.equals(x)` 는 항상 `true` 여야 한다.
    - 대칭성: `x.equals(y)` 가 `true` 라면, `y.equals(x)` 도 `true` 여야 한다.
    - 추이성: `x.equals(y)` 가 `true` 이고, `y.equals(z)` 가 `true` 라면, `x.equals(z)` 도 `true` 여야 한다.
    - 일관성: 객체가 변하지 않는다면 `x.equals(y)` 가 계속해서 같은 결과를 반환해야 한다.
    - `null` 에 대한 비교: `x.equals(null)` 은 항상 `false` 여야 한다.
- hashCode 계약
    - 일관성: 객체의 프로퍼티가 변경되지 않으면, 같은 객체는 항상 같은 해시 코드를 반환해야 한다.
    - 동등 객체: `x.equals(y)` 가 `true` 이면, `x.hashCode()` 와 `y.hashCode()` 는 같아야 한다.
    - 서로 다른 객체: `x.equals(y)` 가 `false` 일지라도, `x.hashCode()` 와 `y.hashCode()` 가 반드시 다른 값을 가질 필요는 없다. 그러나 해시 충돌을 줄이기 위해 다른 값을 반환하는 것이 좋다.
