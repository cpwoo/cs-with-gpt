# Blocking IO 와 Non-Blocking IO 의 차이는 무엇인가? 각각의 장단점에 대해 설명하시오.

## 한 줄 요약: Blocking IO 는 단순하지만 확장성에 한계가 있고, Non-Blocking IO 는 효율적이고 확장성이 뛰어나지만, 구현과 디버깅이 어렵다.

## Blocking IO
Blocking IO 에서는 IO 작업이 완료될 때까지 현재 스레드가 대기한다.
```Java
import java.io.*;
import java.net.*;

public class BlockingIOExample {
    public static void main(String[] args) {
        try (Socket socket = new Socket("example.com", 80);
             PrintWriter out = new PrintWriter(socket.getOutputStream(), true);
             BufferedReader in = new BufferedReader(new InputStreamReader(socket.getInputStream()))) {
            
            // 요청 전송
            out.println("GET / HTTP/1.1\r\nHost: example.com\r\n\r\n");
            
            // 응답 수신 (대기)
            String responseLine;
            while ((responseLine = in.readLine()) != null) {
                System.out.println(responseLine);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

## Non-Blocking IO
Non-Blocking IO 에서는 IO 작업이 완료될 때까지 대기하지 않고, 다른 작업을 수행할 수 있다. Java 에서는 `java.nio` 패키지를 사용하여 구현한다.
```Java
import java.io.IOException;
import java.net.InetSocketAddress;
import java.nio.ByteBuffer;
import java.nio.channels.SelectionKey;
import java.nio.channels.Selector;
import java.nio.channels.SocketChannel;
import java.util.Iterator;
import java.util.Set;

public class NonBlockingIOExample {
    public static void main(String[] args) {
        try {
            Selector selector = Selector.open();
            SocketChannel socketChannel = SocketChannel.open();
            socketChannel.configureBlocking(false);
            socketChannel.connect(new InetSocketAddress("example.com", 80));
            socketChannel.register(selector, SelectionKey.OP_CONNECT);

            while (true) {
                selector.select();
                Set<SelectionKey> selectedKeys = selector.selectedKeys();
                Iterator<SelectionKey> keyIterator = selectedKeys.iterator();

                while (keyIterator.hasNext()) {
                    SelectionKey key = keyIterator.next();

                    if (key.isConnectable()) {
                        handleConnect(key);
                    } else if (key.isReadable()) {
                        handleRead(key);
                    }

                    keyIterator.remove();
                }
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    private static void handleConnect(SelectionKey key) throws IOException {
        SocketChannel socketChannel = (SocketChannel) key.channel();
        if (socketChannel.finishConnect()) {
            ByteBuffer buffer = ByteBuffer.allocate(256);
            buffer.put("GET / HTTP/1.1\r\nHost: example.com\r\n\r\n".getBytes());
            buffer.flip();
            socketChannel.write(buffer);
            socketChannel.register(key.selector(), SelectionKey.OP_READ);
        }
    }

    private static void handleRead(SelectionKey key) throws IOException {
        SocketChannel socketChannel = (SocketChannel) key.channel();
        ByteBuffer buffer = ByteBuffer.allocate(256);
        int bytesRead = socketChannel.read(buffer);
        if (bytesRead == -1) {
            socketChannel.close();
            return;
        }
        buffer.flip();
        while (buffer.hasRemaining()) {
            System.out.print((char) buffer.get());
        }
    }
}
```

## 장단점
- Blocking IO
    - 장점
        - 단순성: 구현이 비교적 간단하고 이해하기 쉽다.
        - 디버깅 용이: 순차적 코드 실행으로 인해 디버깅이 쉬운 편이다.
    - 단점
        - 비효율성: IO 작업이 완료될 때까지 다른 작업을 수행하지 못해 CPU 자원이 낭비될 수 있다.
        - 확장성 문제: 많은 클라이언트를 동시에 처리할 때, 많은 스레드를 생성해야 하며, 이는 자원 소모가 크다.
- Non-Blocking IO
    - 장점
        - 효율성: 여러 IO 작업을 하나의 스레드로 처리할 수 있어 CPU 자원을 효율적으로 사용할 수 있다.
        - 확장성: 많은 동시 연결을 효율적으로 처리할 수 있어 서버의 확장성이 좋다.
    - 단점
        - 복잡성: 코드가 복잡해지고, 비동기적으로 처리되므로 이해하고 디버깅하기 어렵다.
        - 폴링 오버헤드: 폴링 방식으로 구현할 경우, 불필요한 반복 체크로 인해 성능 저하가 발생할 수 있다.
