# enum 은 무엇이고 어떻게 사용하는가? 그리고 enum 이 가지는 특징에 대해 설명하시오.

## 한 줄 요약: enum 은 상수들의 집합을 타입 안전하게 정의하고, 이 상수들이 고유한 속성 및 메소드를 가질 수 있다. 이는 코드의 가독성과 유지보수성을 높이고, 실수를 줄이는 데 도움이 된다.

## Enum 의 정의와 사용법
- `enum` 은 열거형 데이터 타입을 정의하는 데 사용된다.
- 열거형은 고정된 상수 집합을 나타내며, 주로 특정 범주에 속하는 값을 나타낼 때 사용된다.
- 예를 들어, 요일, 월, 카드의 슈트(suit) 등을 나타낼 수 있다.
- `enum`을 정의하는 기본적인 문법은 다음과 같다.
```Java
public enum Day {
    SUNDAY, MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY
}
```
- `enum` 을 사용하는 방법은 다음과 같다.
``` Java
public class EnumExample {
    public static void main(String[] args) {
        Day today = Day.MONDAY;

        switch (today) {
            case MONDAY:
                System.out.println("Today is Monday");
                break;
            case TUESDAY:
                System.out.println("Today is Tuesday");
                break;
            default:
                System.out.println("Some other day");
                break;
        }

        for (Day day: Day.values()) {
            System.out.println(day);
        }
    }
}
```

## Enum 의 특징
- 타입 안정성(Type Safety): 열거형을 사용하면 잘못된 값을 사용할 가능성이 줄어들어 코드의 안정성을 높인다. 예를 들어 `Day` 타입에는 `SUNDAY` 부터 `SATURDAY` 까지의 값만 포함되므로 다른 값이 할당되는 것을 방지할 수 있다.
- 상수 집합(Constant Set): 열거형은 명명된 상수들의 집합으로, 코드의 가독성을 높이고 유지보수를 용이하게 한다.
- 메소드 포함 가능: 열거형은 메소드를 포함할 수 있다. 예를 들어, 열거형에 생성자, 필드, 메소드를 추가할 수 있다.
- 내부에 인스턴스 변수와 메소드 사용 가능: 각 열거형 상수는 고유한 속성이나 동작을 가질 수 있다.
```Java
public enum Planet {
    MERCURY(3.303e+23, 2.4397e6),
    VENUS(4.869e+24, 6.0518e6),
    EARTH(5.976e+24, 6.37814e6),
    MARS(6.421e+23, 3.3972e6),
    JUPITER(1.9e+27, 7.1492e7),
    SATURN(5.688e+26, 6.0268e7),
    URANUS(8.686e+25, 2.5559e7),
    NEPTUNE(1.024e+26, 2.4746e7);

    private final double mass;   // kg
    private final double radius; // meters

    Planet(double mass, double radius) {
        this.mass = mass;
        this.radius = radius;
    }

    public double getMass() {
        return mass;
    }

    public double getRadius() {
        return radius;
    }

    // 표면 중력 계산
    public double surfaceGravity() {
        final double G = 6.67300E-11;
        return G * mass / (radius * radius);
    }
}

public class TestPlanet {
    public static void main(String[] args) {
        for (Planet p : Planet.values()) {
            System.out.printf("Surface gravity on %s is %f%n", p, p.surfaceGravity());
        }
    }
}
```
- 추상 메소드 정의 가능: 열거형 내에 추상 메소드를 정의하고, 각 열거형 상수마다 이를 구현할 수 있다.
```Java
public enum Operation {
    PLUS {
        double apply(double x, double y) { return x + y; }
    },
    MINUS {
        double apply(double x, double y) { return x - y; }
    },
    TIMES {
        double apply(double x, double y) { return x * y; }
    },
    DIVIDE {
        double apply(double x, double y) { return x / y; }
    };

    abstract double apply(double x, double y);
}
```
