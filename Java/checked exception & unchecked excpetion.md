# Checked Exception 과 Unchecked Exception 의 차이점은 무엇인가? 언제 각각을 사용해야 하나? 각 예외의 처리 방식에 대해 설명하시오.

## 한 줄 요약: Checked Exception 은 컴파일 시점에 확인되어 반드시 처리해야 하는 예외고, Unchecked Exception 은 런타임 시점에 발생하여 선택적으로 처리하는 프로그래밍 오류다.

## Checked Exception
- `Checked Exception` 은 컴파일 시점에 확인되는 예외다.
- 컴파일러가 해당 예외가 처리되었는지 확인한다. 처리되지 않으면 컴파일 오류가 발생한다.
- 이 예외는 주로 외부 시스템과의 상호 작용에서 발생할 수 있는 오류를 나타낸다.
- 특징은 다음과 같다.
    - 컴파일 타임 검사: 컴파일러가 예외 처리 여부를 확인한다.
    - 메소드 서명에 포함: 메소드에서 발생할 수 있는 Checked Exception 은 메소드 서명에 `throws` 키워드를 사용하여 명시해야 한다.
    - 예외 처리 강제: 해당 예외를 처리하지 않으면 컴파일 오류가 발생한다.
- 사용 예: 파일 입출력(IOException), 데이터베이스 연결(SQLException), 네트워크 통신(MalformedURLException)
- 처리 방식은 다음과 같다.
    - try-catch 블록 사용: 예외를 직접 처리하는 방식
    ```Java
    try {
        FileReader file = new FileReader("file.txt");
    } catch (IOException e) {
        e.printStackTrace();
    }
    ```
    - throws 키워드 사용: 예외를 호출한 메소드로 전파하는 방식
    ```Java
    public void readFile() throws IOException {
        FileReader file = new FileReader("file.txt");
    }
    ```

## Unchecked Exception
- `Unchecked Exception` 은 런타임 시점에 발생하는 예외다.
- 컴파일러가 예외 처리 여부를 확인하지 않는다.
- 주로 프로그래머의 실수로 인한 예외를 나타내며, `RuntimeException` 을 상속받는 예외들이다.
- 특징은 다음과 같다.
    - 런타임 타임 검사: 컴파일러가 예외 처리 여부를 확인하지 않는다.
    - 메소드 서명에 포함하지 않음: 메소드 서명에 `throws` 키워드를 사용하여 명시할 필요가 없다.
    - 예외 처리 선택적: 예외를 처리하지 않아도 컴파일 오류가 발생하지 않는다.
- 사용 예: 잘못된 인덱스 접근(ArrayIndexOutBoundsException), null 참조(NullPointerException), 부적절한 형 변환(ClassCastException)
- 처리 방식은 다음과 같다.
    - try-catch 블록 사용: 예외를 직접 처리하는 방식
    ```Java
    try {
        int[] arr = new int[5];
        System.out.println(arr[10]);
    } catch (ArrayIndexOutOfBoundsException e) {
        e.printStackTrace();
    }
    ```
    - throw 키워드 사용: 필요에 따라 예외를 명시적으로 던지는 방식
    ```Java
    public void checkValue(int value) {
        if (value < 0) {
            throw new IllegalArgumentException("Value cannot be negative");
        }
    }
    ```

## 언제 각각을 사용해야 하는가?
- Checked Exception
    - 외부 환경과의 상호 작용 시 예상할 수 있는 오류를 처리할 때 사용한다.
    - 예를 들어, 파일을 읽거나 쓰는 작업, 네트워크 연결, 데이터베이스 접근 등 외부 시스템과 통신하는 경우다.
    - 이런 상황에서는 예외를 명시적으로 처리하여 프로그램의 안정성을 높이는 것이 중요하다.
- Unchecked Exception
    - 프로그래머의 실수로 인해 발생할 수 있는 논리적 오류를 나타낼 때 사용한다.
    - 예를 들어, 배열의 잘못된 인덱스 접근, null 객체 참조, 부적절한 형 변환 등이다.
    - 이런 예외는 주로 코드의 버그를 나타내며, 반드시 예외 처리를 강제할 필요는 없다.
    - 대신, 코드를 수정하여 예외가 발생하지 않도록 하는 것이 더 중요하다.
