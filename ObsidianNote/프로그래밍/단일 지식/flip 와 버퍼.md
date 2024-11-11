flip 버퍼를 쓰기 상태에서 읽기 상태로 전환시키기위 한 메서드이다.

기본적으로 버퍼는 쓰기 상태이다
```Java
ByteBuffer buffer = ByteBuffer.allocate(10); // 용량이 10인 버퍼 생성 
buffer.put((byte) 10); 
buffer.put((byte) 20); 
buffer.put((byte) 30);
```
이런식으로 쓴이후
```Java
buffer.flip(); // 읽기 모드로 전환

byte first = buffer.get(); // 10을 읽음
byte second = buffer.get(); // 20을 읽음
// 버퍼 상태: position = 2, limit = 3
```
읽기 상태로 전환해 데이터를 읽을수있다.

버퍼는 쓰기 상태에서 
- position은 0 이상
- limit와 capacity느 동일
- put 메서드로 데이터 기록
버퍼는 읽기 상태에서
- flip이 호출된 이후 postion이 0으로 재설정
- limit는 postion이 flip선언 직전 까지 증가한 값
- get메서드로 읽기

읽기 상태에서 다시 쓰기 상태로 전환하기 위해서는
clear()나 compatct()를 호출해서 다시 전환해야한다.

#flush #버퍼 #buffer
Flush는 버퍼에 저장되 데이터를 출력 대상으로 내보내는 역할을 하는 메서드다.
내보낸다. 는 말은 실제 저장소나 버퍼의 임시 저장소를 지나서 실제 위치로 이동시킨다는 말이다.

### 예시)
1. **파일**:
    
    - **출력 대상**: 파일 시스템에 있는 파일.
    - **역할**: 파일에 데이터를 쓰기 전에 버퍼에 저장된 데이터를 파일에 기록합니다. 예를 들어, `BufferedOutputStream`에서 `flush()`를 호출하면, 메모리에 있는 파일 버퍼의 데이터가 실제 파일에 기록됩니다.
    - **예시**:
        
        java
        
        코드 복사
        
        `try (BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("output.txt"))) {     bos.write("Hello".getBytes());     bos.flush();  // 버퍼의 내용을 "output.txt" 파일에 즉시 기록 }`
        
2. **네트워크 소켓**:
    
    - **출력 대상**: 네트워크 연결을 통해 데이터를 전송할 소켓.
    - **역할**: 네트워크 소켓의 버퍼에 저장된 데이터를 네트워크를 통해 원격 서버나 클라이언트에 전송합니다. 예를 들어, `Socket`의 `flush()` 메서드는 소켓 버퍼에 있는 데이터를 네트워크를 통해 즉시 전송합니다.
    - **예시**:
```Java
Socket socket = new Socket("example.com", 80);
OutputStream os = socket.getOutputStream();
os.write("GET / HTTP/1.1\r\n\r\n".getBytes());
os.flush();  // 버퍼의 HTTP 요청 데이터를 네트워크를 통해 서버로 전송        
```
3. **콘솔/터미널**:
    
    - **출력 대상**: 콘솔 또는 터미널 창.
    - **역할**: 콘솔 출력 버퍼에 저장된 데이터를 즉시 터미널에 출력합니다. 예를 들어, `System.out`의 `flush()` 메서드는 버퍼에 있는 출력을 즉시 콘솔에 표시합니다.
    - **예시**:
```Java
System.out.print("Processing"); 
System.out.flush();  // "Processing"을 콘솔에 즉시 출력 
```
        
4. **메모리**:
    
    - **출력 대상**: 메모리 또는 다른 시스템의 캐시.
    - **역할**: 버퍼에 저장된 데이터를 메모리에 즉시 기록하거나, 메모리와 관련된 캐시를 비우고 데이터를 다른 처리를 위해 준비합니다. 예를 들어, 메모리 매핑 파일에서 버퍼를 플러시하면 메모리에서 디스크로 데이터가 기록될 수 있습니다.
    - **예시**:
 ```Java
FileChannel fileChannel = new RandomAccessFile("data.bin", "rw").getChannel();
MappedByteBuffer buffer = fileChannel.map(FileChannel.MapMode.READ_WRITE, 0, 1024);
buffer.put(0, (byte) 42); buffer.force();  // 버퍼의 내용을 메모리에서 디스크로 즉시 기록
```
        
5. **데이터베이스**:
    
    - **출력 대상**: 데이터베이스의 트랜잭션 로그나 데이터 저장소.
    - **역할**: 데이터베이스의 버퍼나 캐시에 있는 트랜잭션 로그나 변경 사항을 디스크에 기록합니다. 예를 들어, 데이터베이스의 `flush()`는 변경된 데이터를 디스크에 기록하여 데이터의 영속성을 보장합니다.
    - **예시**:
        ``COMMIT;  -- 트랜잭션의 변경 사항을 디스크에 즉시 기록`


또한 특정 커스텀 클래스에 대해서 버퍼를 만들수도있다.
```jAVA
public class SimpleBuffer {
    private byte[] buffer;
    private int position;
    private int limit;

    public SimpleBuffer(int capacity) {
        buffer = new byte[capacity];
        position = 0;
        limit = capacity;
    }

    public void put(byte data) {
        if (position < limit) {
            buffer[position++] = data;
        } else {
            throw new BufferOverflowException();
        }
    }

    public byte get() {
        if (position > 0) {
            return buffer[--position];
        } else {
            throw new BufferUnderflowException();
        }
    }

    public int remaining() {
        return limit - position;
    }

    public void clear() {
        position = 0;
    }
}
```

이런 식으로 byte를 이용해서 만들수도있다.
또한 NIO에서 제공하는 ByteBuffer를 이용할수도 있다.

또한 Buffer 는 기본적으로 FIFO다.

버퍼는 데이터 처리의 중간 단계에서 효율성을 높이기 위해 사용되는 임시 저장소입니다. 버퍼의 역할과 사용 목적을 이해하는 것은 파일 입출력, 네트워크 통신, 데이터 스트림 등 다양한 분야에서 중요합니다

Buffer는 기본적으로 데이터를 완성하기위한 임시 저장소 느낌 임으로