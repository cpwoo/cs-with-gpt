# 오버로딩과 오버라이딩의 차이점에 대해 설명하시오.

## 한 줄 요약: 오버로딩은 주로 편의성을 위한 것이고, 오버라이딩은 상속받은 기능을 변경하거나 확장하기 위한 것이다.

## 오버로딩 (Overloading)
- 오버로딩은 같은 이름의 메서드를 여러 개 정의하되, 매개변수의 타입이나 개수를 다르게 하는 것을 말한다.
- 이는 메소드 이름의 재사용성을 높여 코드를 더 간결하고 이해하기 쉽게 만든다.
- 특징은 다음과 같다.
    - 메소드 이름이 동일하다.
    - 매개변수의 타입, 개수 또는 순서가 달라야 한다.
    - 반환 타입은 상관없다. 반환 타입만 다르고 매개변수가 동일하면 오버로딩이 아니다.
    - 접근 제어자와 예외는 상관없다.
```Java
class MathUtils {
    // 정수의 합
    int add(int a, int b) {
        return a + b;
    }
    
    // 세 정수의 합 (오버로딩 O)
    int add(int a, int b, int c) {
        return a + b + c;
    }
    
    // 실수의 합 (오버로딩 X)
    double add(double a, double b) {
        return a + b;
    }
}
```

## 오버라이딩 (Overriding)
- 오버라이딩은 상속 관계에 있는 클래스에서 부모 클래스의 메소드를 재정의하는 것을 말한다.
- 이는 부모 클래스의 메소드를 자식 클래스에 맞게 변경할 때 사용된다.
- 특징은 다음과 같다.
    - 메소드 이름, 매개변수, 반환 타입이 모두 동일해야 한다.
    - 접근 제어자는 부모 클래스의 메소드보다 더 좁은 범위로 설정할 수 없다.
    - 부모 클래스의 메소드가 예외를 던지면, 자식 클래스의 메소드는 그 예외 또는 그 예외의 하위 예외를 던질 수 있다.
    - @Override 어노테이션을 사용해 오버라이딩을 명확히 할 수 있다. (선택 사항이지만 권장됨)
``` Java
class Animal {
    void sound() {
        System.out.println("Animal makes a sound");
    }
}

class Dog extends Animal {
    @Override
    void sound() {
        System.out.println("Dog barks");
    }
}
```

## 주요 차이점 요약
| 구문 | 오버로딩 (Overloading) | 오버라이딩 (Overriding) |
|:---:|:---:|:---:|
| 정의 |같은 이름의 메소드를 여러 개 정의, 매개변수 다름 | 상속받은 메소드를 재정의 |
| 클래스 관계 | 동일 클래스 내에서 발생 | 상속 관계에 있는 클래스 간 발생 |
| 매개변수 | 타입, 개수, 순서가 달라야 함 | 동일해야 함 |
| 반환 타입 | 상관없음 | 동일해야 함 |
| 접근 제어자 | 상관없음 | 부모 메소드보다 좁은 범위로 설정 불가 |
| 예외 처리 | 상관없음 | 부모 메소드의 예외 또는 하위 예외만 던질 수 있음 |