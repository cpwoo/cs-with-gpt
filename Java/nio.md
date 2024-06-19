# NIO 란 무엇이며, 기존의 IO 와 어떻게 다른가? Java 에서 NIO 는 어떻게 활용하는지 설명하시오.

## 한 줄 요약: NIO 는 고성능 네트워크 서버나 파일 처리 애플리케이션에서 매우 유용하며, 특히 많은 동시 연결을 처리해야 하는 상황에서 기존 IO 보다 더 나은 성능을 발휘한다.

## NIO?
- Java 에서 NIO(New Input/Output) 는 Java 1.4 부터 도입된 새로운 I/O API 다.
- 기존의 IO 와 비교하여 여러 가지 면에서 개선된 기능을 제공한다.

## 차이점
- IO (java.io 패키지)
    - 스트림 지향(Stream Oriented)
        - IO 는 데이터의 연속적인 흐름을 처리한다.
        - 데이터를 바이트 또는 문자 단위로 읽고 쓰며, 이는 연속적인 스트림을 통해 데이터를 처리하는 방식이다.
    - 차단 모드(Block Mode)
        - IO 작업은 기본적으로 차단 모드로 동작한다.
        - 즉, 입력이나 출력 작업이 완료될 때까지 스레드가 블록되어 대기한다.
    - 버퍼링(Buffering)
        - 버퍼링을 지원하지만, 개발자가 직접 버퍼를 제어하기는 어렵다.
    - 다중 스레딩 사용
        - 고성능 서버 애플리케이션에서는 많은 스레드를 사용하여 다중 클라이언트를 처리한다.
        - 이는 높은 스레드 생성 및 관리 오버헤드를 초래할 수 있다.

- NIO (java.nio 패키지)
    - 버퍼 지향(Buffer Oriented)
        - NIO 는 데이터를 버퍼로 읽고 쓰며, 개발자는 직접 버퍼를 관리한다.
        - 버퍼는 데이터의 블록을 메모리에 저장하고, 이를 통해 더 효율적인 데이터 처리를 가능하게 한다.
    - 비차단 모드(Non-Blocking Mode)
        - NIO 는 비차단 모드를 지원한다.
        - 이를 통해 스레드가 특정 채널의 데이터가 준비될 때까지 기다리지 않고, 다른 작업을 계속 수행할 수 있다.
    - 셀렉터 기반 멀티플렉싱(Selector-based Multiplexing)
        - 하나의 스레드가 여러 채널을 감시할 수 있는 셀렉터를 사용한다.
        - 이는 적은 수의 스레드로 많은 클라이언트를 효율적으로 처리할 수 있게 한다.
    - 채널(Channel)과 버퍼(Buffer)
        - 채널은 데이터의 소스 또는 목적지를 나타내며, 버퍼는 데이터가 저장되는 메모리 공간이다.
        - 채널을 통해 버퍼로 데이터를 읽거나 버퍼에서 데이터를 쓴다.

## NIO 의 주요 구성 요소
- Buffer: 데이터를 읽고 쓰기 위한 메모리 공간이다. ByteBuffer, CharBuffer, IntBuffer 등 다양한 타입이 있다.
- Channel: 데이터의 소스 또는 목적지를 나타낸다. FileChannel, SocketChannel, DatagramChannel 등이 있다.
- Selector: 여러 채널의 이벤트를 감시하고, 하나의 스레드가 여러 채널을 효율적으로 처리할 수 있게 한다.
- Path 와 Files: 파일 시스템 작업을 단순화하고, 더 강력한 파일 처리 기능을 제공한다.

## NIO 의 활용
```Java
import java.io.IOException;
import java.net.InetSocketAddress;
import java.nio.ByteBuffer;
import java.nio.channels.SelectionKey;
import java.nio.channels.Selector;
import java.nio.channels.ServerSocketChannel;
import java.nio.channels.SocketChannel;
import java.util.Iterator;

/** NIO 를 사용하여 비차단 소켓 서버를 구현하는 기본적인 예제
 * 비차단 모드로 동작하는 간단한 에코 서버를 구현
 * 서버는 클라이언트의 연결을 받아들이고, 클라이언트가 보낸 메시지를 읽고 다시 클라이언트에게 돌려준다.
 * Selector 를 사용하여 단일 스레드로 여러 클라이언트 연결을 효율적으로 처리한다.
*/
public class NioServer {
    public static void main(String[] args) throws IOException {
        Selector selector = Selector.open();
        ServerSocketChannel serverSocketChannel = ServerSocketChannel.open();
        serverSocketChannel.bind(new InetSocketAddress("localhost", 8080));
        serverSocketChannel.configureBlocking(false);
        serverSocketChannel.register(selector, SelectionKey.OP_ACCEPT);

        while (true) {
            selector.select();
            Iterator<SelectionKey> iterator = selector.selectedKeys().iterator();
            while (iterator.hasNext()) {
                SelectionKey key = iterator.next();
                iterator.remove();

                if (key.isAcceptable()) {
                    SocketChannel clientChannel = serverSocketChannel.accept();
                    clientChannel.configureBlocking(false);
                    clientChannel.register(selector, SelectionKey.OP_READ);
                } else if (key.isReadable()) {
                    SocketChannel clientChannel = (SocketChannel) key.channel();
                    ByteBuffer buffer = ByteBuffer.allocate(256);
                    int bytesRead = clientChannel.read(buffer);
                    if (bytesRead == -1) {
                        clientChannel.close();
                    } else {
                        buffer.flip();
                        while (buffer.hasRemaining()) {
                            clientChannel.write(buffer);
                        }
                        buffer.clear();
                    }
                }
            }
        }
    }
}
```

## 장점
- 효율성: 비차단 I/O 와 셀렉터를 통해 더 적은 리소스로 많은 연결을 처리할 수 있다.
- 확장성: 많은 클라이언트를 동시에 처리할 수 있어, 고성능 서버 애플리케이션에 적합하다.
- 유연성: 버퍼와 채널을 통해 더 세밀한 데이터 제어가 가능하다.
