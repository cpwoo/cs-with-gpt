# Java 에서 직렬화란 무엇이며, 왜 사용되는지 설명하시오.

# 한 줄 요약: 객체의 상태를 바이트 스트림으로 변환하는 과정이며, 바이트스트림은 DB 또는 네트워크를 통해 전송되거나 저장될 수 있다.

## 직렬화를 사용하는 이유
- 데이터 전송: 네트워크를 통해 객체를 전송할 때, 객체를 직렬화하여 바이트 스트림으로 변환한 후 전송한다. 수신 측에서는 역직렬화하여 객체로 복원할 수 있다.
- 영구 저장: 객체의 상태를 파일이나 DB 에 저장할 수 있다. 직렬화를 사용하면 객체의 상태를 쉽게 저장하고 나중에 복원할 수 있다.
- 캐싱: 객체를 직렬화하여 캐시에 저장하고 필요할 때 역직렬화하여 사용함으로써 성능을 향상시킬 수 있다.

## 직렬화와 역직렬화 사용 방법
- Java 에서 객체를 직렬화하려면 `java.io.Serializable` 인터페이스를 구현해야 한다.
- 또한, `ObjectOutputStream` 과 `ObjectInputStream` 클래스를 사용하여 객체를 직렬화 및 역직렬화할 수 있다.
```Java
import java.io.Serializable;

public class Person implements Serializable {
    private static final long serialVersionUID = 1L; // 버전 관리용 ID
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return "Person{name='" + name + "', age=" + age + "}";
    }
}
```
```Java
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.ObjectOutputStream;

public class SerializeExample {
    public static void main(String[] args) {
        Person person = new Person("John Doe", 30);

        try (FileOutputStream fileOut = new FileOutputStream("person.ser");
             ObjectOutputStream out = new ObjectOutputStream(fileOut)) {
            out.writeObject(person);
            System.out.println("직렬화 완료: " + person);
        } catch (IOException i) {
            i.printStackTrace();
        }
    }
}
```
```Java
import java.io.FileInputStream;
import java.io.IOException;
import java.io.ObjectInputStream;

public class DeserializeExample {
    public static void main(String[] args) {
        Person person = null;

        try (FileInputStream fileIn = new FileInputStream("person.ser");
             ObjectInputStream in = new ObjectInputStream(fileIn)) {
            person = (Person) in.readObject();
            System.out.println("역직렬화 완료: " + person);
        } catch (IOException i) {
            i.printStackTrace();
        } catch (ClassNotFoundException c) {
            System.out.println("Person 클래스 찾을 수 없음");
            c.printStackTrace();
        }
    }
}
```

## 중요한 고려사항
- serialVersionUID
    - 클래스가 변경되더라도 직렬화된 객체의 호환성을 유지하기 위해 `serialVersionUID` 를 명시적으로 정의하는 것이 좋다.
    - 정의하지 않으면 컴파일러가 자동 생성하지만, 클래스 구조 변경 시 호환성 문제가 발생할 수 있다.
- transient 키워드
    - 직렬화하지 않으려는 필드는 `transient` 키워드를 사용한다.
    - 예를 들어, 비밀번호 같은 민감한 정보는 직렬화 대상에서 제외할 수 있다.
- 직렬화 성능
    - 대규모 객체 그래프를 직렬화/역직렬화하는 것은 성능에 영향을 미칠 수 있다.
    - 필요한 경우 효율적인 직렬화 기법(ex. Protobuf, Kryo ...)을 고려할 수 있다.

## transient 사용 예제
```Java
import java.io.Serializable;

public class User implements Serializable {
    private static final long serialVersionUID = 1L;
    private String username;
    private transient String password;

    public User(String username, String password) {
        this.username = username;
        this.password = password;
    }

    @Override
    public String toString() {
        return "User{username='" + username + "', password='" + password + "'}";
    }
}
```
