# Java 의 소켓 프로그래밍이란 무엇이며, 어떤 상황에서 주로 사용되는지 설명하시오.

## 한 줄 요약: 네트워크를 통해 다른 컴퓨터와 데이터를 주고받기 위해 소켓을 활용하는 프로그래밍 기법. 네트워크 통신을 효율적으로 처리할 수 있는 방법을 제공한다.

## 소켓 프로그래밍의 기본 개념
- 소켓(Socket)
    - 소켓은 네트워크 상에서 통신을 위한 끝점을 의미한다. 
    - 소켓을 통해 컴퓨터는 네트워크 상에서 다른 컴퓨터와 데이터를 송수신할 수 있다.
    - 소켓은 IP 주소와 포트 번호를 조합하여 네트워크 상에서 유일한 식별자를 갖는다.
- 클라이언트-서버 모델
    - 소켓 프로그래밍에서 일반적으로 클라이언트-서버 모델을 사용한다.
    - 서버는 특정 포트에서 클라이언트의 요청을 기다리고, 클라이언트는 서버에 연결을 요청한다.
    - 서버는 다수의 클라이언트 요청을 처리할 수 있다.

## 주요 클래스와 인터페이스
- ServerSocket 클래스
    - 서버 소켓을 생성하여 특정 포트에서 클라이언트의 연결 요청을 기다린다.
    - `accept()` 메소드를 사용하여 클라이언트의 연결 요청을 처리한다.
    ```Java
    ServerSocket serverSocket = new ServerSocket(12345);
    Socket clientSocket = serverSocket.accept(); // 클라이언트 연결 수락
    ```
- Socket 클래스
    - 클라이언트와 서버 간의 통신을 위해 사용된다.
    - 클라이언트 측에서 서버에 연결 요청을 보낼 때 사용된다.
    - 서버 측에서는 `ServerSocket` 이 클라이언트의 연결을 수락하면 `Socket` 객체를 생성한다.
    ```Java
    Socket socket = new Socket("127.0.0.1", 12345); // 클라이언트가 서버에 연결 요청
    ```
- InputStream 과 OutputStream
    - 소켓을 통해 데이터를 주고받기 위해 사용된다.
    - `getInputStream()` 과 `getOutputStream()` 메소드를 사용하여 입력 스트림과 출력 스트림을 얻을 수 있다.
    ```Java
    InputStream input = socket.getInputStream();
    OutputStream output = socket.getOutputStream();
    ```

## 소켓 프로그래밍의 단계
- 서버 측 작업
    - `ServerSocket` 객체를 생성하여 특정 포트에서 클라이언트 연결을 대기한다.
    - 클라이언트 연결 요청을 수락하고, `Socket` 객체를 생성한다.
    ```Java
    ServerSocket serverSocket = new ServerSocket(12345);
    Socket clientSocket = serverSocket.accept();
    InputStream input = clientSocket.getInputStream();
    OutputStream output = clientSocket.getOutputStream();
    // 데이터 송수신
    ```
- 클라이언트 측 작업
    - `Socket` 객체를 생성하여 서버에 연결을 요청한다.
    - 서버와 연결이 성립되면, `InputStream` 과 `OutputStream` 을 사용하여 데이터를 주고받는다.
    ```Java
    Socket socket = new Socket("127.0.0.1", 12345);
    InputStream input = socket.getInputStream();
    OutputStream output = socket.getOutputStream();
    // 데이터 송수신
    ```

## 주요 사용 상황
- 네트워크 애플리케이션
    - 채팅 프로그램, 메신저, 온라인 게임 등에서 사용된다.
    - 이러한 애플리케이션은 클라이언트-서버 모델을 기반으로 다수의 클라이언트와 서버 간의 실시간 통신이 필요하다.
- 웹 서버와 클라이언트
    - HTTP 프로토콜을 사용하여 웹 페이지를 제공하는 웹 서버와 이를 요청하는 웹 브라우저(클라이언트) 간의 통신에서도 소켓이 사용된다.
- 데이터 전송
    - 파일 전송, 데이터 스트리밍, 원격 제어 등 대용량 데이터를 전송하거나 지속적인 데이터 통신이 필요한 경우 소켓 프로그래밍이 필요하다.
- 분산 시스템
    - 분산 시스템에서는 여러 노드 간의 통신이 필요하며, 이를 위해 소켓 프로그래밍이 사용된다.
    - 예를 들어, DB 나 클라우드 컴퓨팅 환경에서 노드 간 데이터 동기화 및 통신에 소켓이 사용된다.

## 예제 코드
```Java
import java.io.*;
import java.net.*;

// 서버 측 코드
public class SimpleServer {
    public static void main(String[] args) {
        try (ServerSocket serverSocket = new ServerSocket(12345)) {
            System.out.println("Server is listening on port 12345");
            Socket socket = serverSocket.accept();
            System.out.println("Client connected");

            BufferedReader in = new BufferedReader(new InputStreamReader(socket.getInputStream()));
            PrintWriter out = new PrintWriter(socket.getOutputStream(), true);

            String message;
            while ((message = in.readLine()) != null) {
                System.out.println("Received: " + message);
                out.println("Echo: " + message);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
```Java
import java.io.*;
import java.net.*;

// 클라이언트 측 코드
public class SimpleClient {
    public static void main(String[] args) {
        try (Socket socket = new Socket("localhost", 12345)) {
            BufferedReader in = new BufferedReader(new InputStreamReader(socket.getInputStream()));
            PrintWriter out = new PrintWriter(socket.getOutputStream(), true);
            BufferedReader consoleIn = new BufferedReader(new InputStreamReader(System.in));

            String userInput;
            while ((userInput = consoleIn.readLine()) != null) {
                out.println(userInput);
                System.out.println("Server response: " + in.readLine());
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
