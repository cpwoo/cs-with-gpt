# Connection 을 close 하지 않으면 발생하는 문제는 무엇인가? 그리고 이 문제를 방지하기 위한 방법은 무엇이 있는지 설명하시오.

## 한 줄 요약: 리소스 누수, 메모리 누수, 데드락, 보안 취약점이 발생할 수 있으며, 이를 방지하기 위해 try-with-resource 와 연결 풀링을 사용해야 한다.

## 문제점
- 리소스 누수(Resource Leak)
    - DB 연결, 파일 핸들, 소켓 등 다양한 자원이 닫히지 않고 남아 있으면 시스템 리소스가 고갈된다.
    - 리소스가 부족하면 새로운 연결을 생성할 수 없게 되어 애플리케이션의 성능이 저하되거나 장애가 발생할 수 있다.
- 메모리 누수(Memory Leak)
    - 닫히지 않은 연결이 계속해서 메모리를 점유하기 때문에, 시간이 지나면서 메모리가 점차 부족해진다.
    - 메모리가 부족해지면 애플리케이션이 불안정해지거나, 심한 경우 시스템 전체가 영향을 받을 수 있다.
- 데드락(Deadlock)
    - 열린 채로 남아 있는 연결이 트랜잭션을 차지하고 있는 경우, 다른 트랜잭션들이 대기 상태에 빠질 수 있다.
    - 이러한 상황이 지속되면 데드락이 발생하여 시스템이 멈출 수 있다.
- 보안 취약점
    - 닫히지 않은 네트워크 연결은 악의적인 사용자가 시스템에 접근하는 통로가 될 수 있다.
    - 공격자가 열린 연결을 통해 시스템에 침투하거나 데이터를 가로챌 가능성이 증가한다.

## 방지 방법
- 적절한 자원 해제
    - 데이터베이스 연결, 파일 핸들, 네트워크 소켓 등을 사용한 후 반드시 닫아야 한다.
    - 일반적으로 `try-finally` 블록이나 `try-with-resource` 를 사용하여 자원을 해제한다.
    ```Java
    // try-finally 예시
    Connection conn = null;
    try {
        conn = DriverManager.getConnection(url, user, password);
        // 데이터베이스 작업 수행
    } catch (SQLException e) {
        e.printStackTrace();
    } finally {
        if (conn != null) {
            try {
                conn.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
    ```
    ```Java
    // try-with-resources 예시
    try (Connection conn = DriverManager.getConnection(url, user, password)) {
        // 데이터베이스 작업 수행
    } catch (SQLException e) {
        e.printStackTrace();
    }
    ```
- 연결 풀링(Connection Pooling)
    - DB 연결을 효율적으로 관리하기 위해 연결 풀링을 사용한다.
    - 연결 풀은 일정 수의 연결을 미리 생성해 두고 필요할 때마다 재사용하기 때문에 연결을 반복적으로 생성/해제하는 오버헤드를 줄일 수 있다.
    - 대표적인 연결 풀 라이브러리는 HikariCP, Apache DBCP, C3P0 등이 있다.
    ```Java
    // HikariCP 예시
    HikariConfig config = new HikariConfig();
    config.setJdbcUrl("jdbc:mysql://localhost:3306/yourdatabase");
    config.setUsername("yourusername");
    config.setPassword("yourpassword");

    HikariDataSource dataSource = new HikariDataSource(config);

    try (Connection conn = dataSource.getConnection()) {
        // 데이터베이스 작업 수행
    } catch (SQLException e) {
        e.printStackTrace();
    }
    ```
- 자동화된 자원 관리
    - 자원 해제를 자동화하기 위해 try-with-resources 구조를 적극 활용한다.
    - Java 7 이상에서 사용할 수 있는 이 구조는 `AutoCloseable` 을 구현한 모든 자원에 대해 자동으로 close 메소드를 호출한다.
- 테스트와 모니터링
    - 자원이 제대로 해제되고 있는지 정기적으로 테스트하고 모니터링한다.
    - 리소스 사용 현황을 로그로 기록하고 분석하여, 비정상적인 리소스 사용 패턴을 감지한다.
    ```Java
    // 로깅 예시 (Log4j2 사용)
    Logger logger = LogManager.getLogger(YourClass.class);

    try (Connection conn = DriverManager.getConnection(url, user, password)) {
        // 데이터베이스 작업 수행
    } catch (SQLException e) {
        logger.error("데이터베이스 연결 오류", e);
    }
    ```
