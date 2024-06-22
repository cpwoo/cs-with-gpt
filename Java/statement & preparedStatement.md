# Statement 와 PreparedStatement 의 차이점과 어떤 상황에서 어떤 것을 사용하는 것이 좋은지 설명하시오.

## 한 줄 요약: DB 에 SQL 문을 실행하기 위해 사용되는 두 가지 인터페이스이며, Statement 는 간단한 일회성 쿼리에, PreparedStatement 는 반복적 쿼리 실행에 적합하다.

## Statement
- `Statement` 는 SQL 쿼리를 실행하기 위해 사용되는 인터페이스다.
- 일반적으로 `Statement` 객체는 다음과 같은 절차로 사용된다.
    - `Statement` 객체 생성
        ```Java
        Statement stmt = connection.createStatement();
        ```
    - SQL 쿼리 실행
        ```Java
        ResultSet rs = stmt.executeQuery("SELECT * FROM users");
        ```
- 특징
    - SQL 쿼리 반복: `Statement` 객체는 매번 쿼리를 컴파일하고 실행한다. 동일한 쿼리를 여러 번 실행해야 하는 경우, 성능이 저하될 수 있다.
    - SQL 인젝션 위험: 사용자가 입력한 값을 직접 쿼리에 포함시키면 SQL 인젝션 공격에 취약하다.
    - 파라미터 지원 부족: SQL 쿼리에 파라미터를 바인딩할 수 있는 기능이 없다. 모든 값을 쿼리 문자열에 직접 삽입해야 한다.
- 사용 예
    ```Java
    Statement stmt = connection.createStatement();
    ResultSet rs = stmt.executeQuery("SELECT * FROM users WHERE age > 30");
    ```

## PreparedStatement
- `PreparedStatement` 는 미리 컴파일된 SQL 문을 실행하기 위한 인터페이스다.
- `PreparedStatement` 는 다음과 같은 절차로 사용된다.
    - `PreparedStatement` 객체 생성
        ```Java
        PreparedStatement pstmt = connection.prepareStatement("SELECT * FROM users WHERE age > ?");
        ```
    - 파라미터 설정
        ```Java
        PreparedStatement pstmt = connection.prepareStatement("SELECT * FROM users WHERE age > ?");
        ```
    - SQL 쿼리 실행
        ```Java
        ResultSet rs = pstmt.executeQuery();
        ```
- 특징
    - 미리 컴파일된 쿼리: SQL 문이 미리 컴파일되므로, 동일한 쿼리를 여러 번 실행해도 성능이 향상된다.
    - SQL 인젝션 방지: 파라미터를 바인딩할 때 자동으로 이스케이핑이 되므로, SQL 인젝션 공격을 방지할 수 있다.
    - 가독성 및 유지보수성: 파라미터를 바인딩할 수 있어 코드의 가독성과 유지보수성이 향상된다.
    - 재사용 기능: 동일한 SQL 문을 여러 번 실행해야 할 때 유용한다.
- 사용 예
```Java
PreparedStatement pstmt = connection.prepareStatement("SELECT * FROM users WHERE age > ?");
pstmt.setInt(1, 30);
ResultSet rs = pstmt.executeQuery();
```

## 언제 어떤 것을 사용할 것인가?
- Statement 사용 시기
    - 단순한 SQL 문: 쿼리가 단순하고, 한두 번만 실행될 경우 `Statement` 를 사용해도 무방하다.
    - 동적 쿼리: 쿼리 자체가 동적으로 구성되어야 하는 경우, 즉 쿼리 문자열이 런타임에 구성되는 경우 유용할 수 있다.
- PreparedStatement 사용 시기
    - 반복 실행되는 쿼리: 동일한 SQL 쿼리를 여러 번 실행해야 하는 경우 성능상 유리하다.
    - 파라미터화된 쿼리: 사용자 입력을 포함하는 쿼리를 실행할 때는 반드시 `PreparedStatement` 를 사용하여 SQL 인젝션을 방지해야 한다.
    - 성능 최적화: 대규모 DB 애플리케이션에서는 쿼리 컴파일 오버헤드를 줄이기 위해 `PreparedStatement` 를 사용하는 것이 좋다.
