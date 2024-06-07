# Java 에서 Exception 이란 무엇이며, 언제 발생하는지 설명하시오.

## 한 줄 요약: Exception 은 프로그램 실행 중에 발생할 수 있는 예외적인 상황을 나타내는 객체다.
- 이러한 예외는 프로그램이 정상적으로 실행되지 못하게 하는 상황을 의미하며, 이를 처리하지 않으면 프로그램이 비정상적으로 종료될 수 있다.
- 예외는 다양한 이유로 발생할 수 있으며, Java 에서는 이러한 예외를 체계적으로 처리하기 위해 예외 클래스를 제공한다.

## 예외의 종류
- Checked Exception (검사 예외)
    - Checked Exception 은 컴파일 타임에 체크되는 예외로, 예외 처리 코드가 반드시 필요하다.
    - 예외 처리 코드를 작성하지 않으면 컴파일 오류가 발생한다.
    - 이러한 예외는 보통 외부 자원(파일, 네트워크 등)과의 입출력 작업에서 발생한다.
    - 대표적인 예로는 `IOException`, `SQLException` 등이 있다.
    ```Java
    public void readFile(String fileName) {
        try {
            FileReader fileReader = new FileReader(fileName);
            BufferedReader bufferedReader = new BufferedReader(fileReader);
            String line;
            while ((line = bufferedReader.readLine()) != null) {
                System.out.println(line);
            }
            bufferedReader.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    ```
- Unchecked Exception (비검사 예외)
    - Unchecked Exception 은 런타임에 발생하는 예외로, 컴파일러가 예외 처리를 강제하지 않는다.
    - 주로 프로그래밍 오류(예: 배열 범위 초과, null 참조 등)에서 발생한다.
    - 대표적인 예로는 `NullPointerException`, `ArrayIndexOutOfBoundsException`, `ArithmeticException` 등이 있다.
    ```Java
    public void divide(int a, int b) {
        try {
            int result = a / b;
            System.out.println("Result: " + result);
        } catch (ArithmeticException e) {
            System.out.println("Cannot divide by zero.");
        }
    }
    ```

## 예외 처리
Java 에서는 예외를 처리하기 위해 `try`, `catch`, `finally`, `throw`, `throws` 키워드를 사용한다.
- try-catch-finally
    - `try`: 예외가 발생할 수 있는 코드를 감싼다.
    - `catch`: 예외가 발생했을 때 처리할 코드를 작성한다.
    - `finally`: 예외 발생 여부와 상관없이 항상 실행되는 코드를 작성한다.
    ```Java
    try {
        // 예외가 발생할 수 있는 코드
    } catch (ExceptionType e) {
        // 예외 처리 코드
    } finally {
        // 항상 실행되는 코드
    }
    ```
- throw
    - `throw` 키워드를 사용하여 명시적으로 예외를 발생시킬 수 있다.
    ```Java
    public void checkAge(int age) {
        if (age < 18) {
            throw new IllegalArgumentException("Age must be at least 18.");
        }
    }
    ```
- throws
    - `throws` 키워드는 메소드 선언부에 사용되며 해당 메소드에서 발생할 수 있는 예외를 호출자에게 전달할 수 있음을 명시한다.
    ```Java
    public void readFile(String fileName) throws IOException {
        FileReader fileReader = new FileReader(fileName);
        // ...
    }
    ```

## 예외 계층 구조
Java 의 예외는 `java.lang.Throwable` 클래스를 상속받는다. `Throwable` 클래스의 두 하위 클래스는 `Error` 와 `Exception` 이다.
- Error: JVM 의 오류로, 애플리케이션에서 복구할 수 없는 심각한 문제를 나타낸다. 예를 들어 `OutOfMemoryError` 등이 있다.
- Exception: 애플리케이션에서 발생할 수 있는 예외 상황을 나타낸다. 다시, `Exception` 은 `RuntimeException` 과 그 외의 예외로 나타낸다.
